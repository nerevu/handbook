# Logging into EC2 instance with different keys

*Note: These instructions work for Amazon Linux 2 AMIs by default, but they will not work with other AMIs unless you [configure them correctly](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-set-up.html).*

## Two Possible Solutions

To get access to your EC2 instance, you can connect via the online connect portal on AWS (easier) or you can grant yourself a temporary token to allow you access (a little less easy).

For either scenario, you will need to generate a new private/public key-pair set. Do that now.
   ```bash
   ssh-keygen -t rsa -f new_key
   ```


## AWS Connect Portal (easier)
1. Select your instance on AWS.
2. Click the `Connect` button.
3. Select `EC2 Instance Connect (browser-based SSH connection)`.
4. Change the username to `ec2-user`.
5. Open your public key (`new_key.pub`) in Notepad (or another editor) and copy it to your clipboard.
6. Add your public key the `authorized_keys` file.
   ```bash
   sudo nano ~/.ssh/authorized_keys

   # Paste the public key on a new line, then save the file.
   ```
   *NOTE: Vim is a terminal text editor. You can use any other terminal editor to do this as well (e.g. Nano)*
7. You should be able to ssh into your EC2 instance from your local computer with your private key now.
   ```bash
   ssh -i {path/to/new_key} ec2-user@{public_ip_address}
   ```

## Temporary User Token (a little less easy)

### Grant Correct User Permissions

First, you need to give your user permissions to use a different key file to log into the EC2 instance.

1. Log into the AWS Console
2. Search for `IAM` and click it
3. Click `Groups`
4. Click on `admins`
5. Click `Add Users to Group`
6. Select your user and click `Add Users`

### Connect to the Instance

Now that you have the permissions, access the ec2 through an ssh client.

1. [Download the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

   *You might have to copy/move your aws-cli installation.*

   ```bash
   sudo cp /usr/local/bin/aws /usr/bin/
   ```

2. Use the aws-cli to send your key to the ec2 instance for temporary access.

   *Make sure to change the `region`, `instance-id`, `availability-zone`, and `ssh-public-key` to your respective values.*

   ```bash
   aws ec2-instance-connect send-ssh-public-key --region us-east-1 --instance-id i-0dde2e21194e727ae --availability-zone us-east-1c --instance-os-user ec2-user --ssh-public-key file://new_key.pub
   ```

   *After running this command, you only have 60 seconds to log into your ec2 instance. If you miss the window to log in, just run the command again to get 60 more seconds.*

3. Log in to the ec2 instance using it's public IP address and your private key.

   ```bash
   ssh -i ./new_key ec2-user@{public_ip}
   ```

4. Copy your public key into the `~/.ssh/authorized_keys` file on a new line. This allows you to access the instance whenever you need.

   *I opened my public key file in a text editor (notepad) and copied it from there with Ctrl+C.*

You're in! Great job :) You should feel very satisfied with your accomplishments.
