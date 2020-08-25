# Setting up HTTPS on CKAN EC2

CKAN uses an Nginx server as a reverse proxy to an Apache server. This means that we will need to configure an HTTPS certificate for our Nginx server instead of our Apache server to get CKAN working on HTTPS.

*[This tutorial](https://www.youtube.com/watch?v=ng5DsxYp-Bk) will give you a basic idea of how reverse proxies work (we're using Apache instead of Node).*

## Prerequisites

- Enable inbound connections on port 443 (HTTPS) for your EC2 instance Security Group rules.

- Knowledge of terminal editors - I use `vim`, but feel free to use `nano` or whatever editor you like.

- Below are the versions for the technologies I'm using. You probably don't need the exact versions:

    - **Nginx** - nginx/1.12.2
    - **Apache Web Server** - Server version: Apache/2.4.41 ()
    - **CKAN** - CKAN 2.8.3 (2020-03-13) delivered by Link Digital (AWS Marketplace)

## Set up Certbot on your EC2 instance

Before you can get a certificate to use HTTPS, you need to verify that you are in control of the domain that you are trying to get a certificate for. To do this, we will complete an [`HTTP Challenge`](https://letsencrypt.org/docs/challenge-types/#http-01-challenge).

Certbot is a tool that works with LetsEncrypt to help you issue and renew TLS certificates for free. We will be using this to configure our https certificates. Our CKAN instance is hosted on an Amazon Linux 2 AMI, which is most closely related to Centos Linux distributions. To get Certbot working on your EC2 instance, do the following:

1. Stop nginx and apache servers.

    ```bash
    sudo systemctl stop nginx
    sudo systemctl stop httpd
    ```

2. Prepare Certbot

    Follow the instructions in [this tutorial (only step #1)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html#prepare). This worked better for me than the Certbot instructions.


3. Install Certbot

    ```bash
    sudo yum install -y certbot python2-certbot-apache
    ```


4. Run Certbot

    - the `certonly` option will allow us to configure our web servers the way we want.

    ```bash
    sudo certbot certonly --nginx

    # Saving debug log to /var/log/letsencrypt/letsencrypt.log
    # Plugins selected: Authenticator nginx, Installer nginx
    # No names were found in your configuration files. Please enter in your domain
    # name(s) (comma and/or space separated)  (Enter 'c' to cancel):
    ```

5. Type in the domain name and press enter. You should get a confirmation that your certificates have been downloaded, with a path to the `fullchain.pem` and `privkey.pem` file locations. Put these file paths somewhere you can access them easily for the next steps.

    ```bash
    # Obtaining a new certificate
    # Performing the following challenges:
    # http-01 challenge for data.openpeoria.com
    # nginx: [error] invalid PID number "" in "/run/nginx.pid"
    # Waiting for verification...
    # Cleaning up challenges
    #
    # IMPORTANT NOTES:
    # - Congratulations! Your certificate and chain have been saved at:
    #   /etc/letsencrypt/live/data.openpeoria.com/fullchain.pem
    #   Your key file has been saved at:
    #   /etc/letsencrypt/live/data.openpeoria.com/privkey.pem
    #   Your cert will expire on 2020-11-23. To obtain a new or tweaked
    #   version of this certificate in the future, simply run certbot
    #   again. To non-interactively renew *all* of your certificates, run
    #   "certbot renew"
    # - If you like Certbot, please consider supporting our work by:
    #
    #   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
    #   Donating to EFF:                    https://eff.org/donate-le
    ```


6. Configure Automated Certificate Renewal

    Ensure no other crontab exists

    ```bash
    sudo grep certbot < /etc/crontab
    ```

    If no other one exists, add it

    ```bash
    echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
    ```

References:

- [docs.aws.amazon.com](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html)
- [certbot.eff.org](https://certbot.eff.org/lets-encrypt/centosrhel7-nginx)

## Set up your servers to redirect to HTTPS

### Edit your Nginx Config File

Now we need to configure our Nginx and Apache servers to work with HTTPS.

1. Edit the `/etc/nginx/conf.d/ckan.conf` file.

   ```bash
   sudo nano /etc/nginx/conf.d/ckan.conf
   ```

2. We will need to remove the existing server block and add the following server block rule to listen for HTTPS traffic.

    ```nginx
    server {
        listen 443;
        ssl on;
        # REPLACE THESE 2 PATHS WITH THE PATHS YOU COPIED FROM CERTBOT EARLIER
        ssl_certificate /etc/letsencrypt/live/data.openpeoria.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/data.openpeoria.com/privkey.pem;

        client_max_body_size 100M;
        location / {
            #redirect all https requests to the apache server hosting our CKAN instance
            proxy_pass http://127.0.0.1:8000/;
            proxy_set_header X-Forwarded-For $remote_addr;
            proxy_set_header Host $host;
            proxy_cache cache;
            proxy_cache_bypass $cookie_auth_tkt;
            proxy_no_cache $cookie_auth_tkt;
            proxy_cache_valid 30m;
            proxy_cache_key $host$scheme$proxy_host$request_uri;
            # In emergency comment out line to force caching
            # proxy_ignore_headers X-Accel-Expires Expires Cache-Control;
        }
    }
    ```

3. Now add another rule for Port 80 (HTTP) traffic that redirects all traffic through the HTTPS rule we just made.

    ```nginx
    server {
       listen 80;
       server_name data.openpeoria.com;
       rewrite ^ https://$server_name$request_uri? permanent;
    }
    ```

4. Save and exit this file.

5. Restart your Nginx server.

    ```bash
    sudo systemctl restart nginx
    ```

6. If you get the error `Job for nginx.service failed because the control process exited with error code. See "systemctl status nginx.service" and "journalctl -xe" for details.`, kill the nginx process and retry.

    ```bash
    sudo pkill -f nginx & wait $!
    [1] 9588
    [1]+  Done                    sudo pkill -f nginx
    sudo systemctl restart nginx
    ```

### Edit Your Apache Config Files

If `mod_ssl` is installed on Apache, you will have an `/etc/httpd/conf.d/ssl.conf` file that is listening for HTTPS connections. We want Nginx to handle HTTPS, so we will remove this functionality.

1. Open an editor for `/etc/httpd/conf.d/ssl.conf`.

    ```bash
    sudo nano /etc/httpd/conf.d/ssl.conf
    ```

2. Remove the HTTPS listener

    ```bash
    # remove this line
    Listen 443
    ```

3. Ensure the correct server name is present

    ```bash
    # Search for lines like the following
    SSLCertificateFile /etc/letsencrypt/live/data.openpeoria.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/data.openpeoria.com/privkey.pem
    ```

4. Save and exit.

5. Comment out the `mod_http2` section of your `/etc/httpd/conf/httpd.conf` file.

    ```bash
    # <IfModule mod_http2.c>
    #    Protocols h2 h2c http/1.1
    # </IfModule>
    ```

6. Restart your Apache server.

    ```bash
    sudo systemctl restart httpd
    ```

7. If you are having trouble restarting your server, check the config file by running `apachectl configtest` from the command line. Don't worry if you get an `SSLCertificateFile does not exist` error. Apache isn't handling HTTPS, so we don't need to worry about this.

### Edit your CKAN Installation

After the following two steps, your site should run via https! However, static content may still be loaded via http. We can fix this by changing some settings in the CKAN installation.

1. Change the `ckan.site_url` variable in the `/etc/ckan/default/production.ini` file to your https domain name.

    ```ini
    ## Site Settings
    ckan.site_url = https://data.openpeoria.com
    ```

2. Restart your apache server

    ```bash
    sudo systemctl restart httpd
    ```

## Conclusion

Your site should be up and running now over HTTPS! If you don't see it working immediately, you may have caching issues. Try opening your site in a different browser.

You can also check your site at https://www.whynopadlock.com. This will give you helpful information for troubleshooting.
