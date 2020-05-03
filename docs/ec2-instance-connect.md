# Logging into EC2 instance with different keys

*Note: These instructions work for Amazon Linux 2 AMIs by default, but they will not work with other AMIs unless you [configure them correctly](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-set-up.html).*

## Grant Correct User Permissions

First, you need to give your user permissions to use a different key file to log into the EC2 instance.

1. Log into the AWS Console
2. Search for `IAM` and click it
3. Click `Groups`
4. Click on `admins`
5. Click `Add Users to Group`
6. Select your user and click `Add Users`

## Connect to the Instance

Now that you have the permissions, access the ec2 through an ssh client.

1. Generate a pair of keys to use with the ec2 instance.

   ```bash
   ssh-keygen -t rsa -f new_key
   ```

2. [Download the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

   *You might have to copy/move your aws-cli installation.*

   ```bash
   sudo cp /usr/local/bin/aws /usr/bin/
   ```

3. Use the aws-cli to send your key to the ec2 instance for temporary access.

   *Make sure to change the `region`, `instance-id`, `availability-zone`, and `ssh-public-key` to your respective values.*

   ```bash
   aws ec2-instance-connect send-ssh-public-key --region us-east-1 --instance-id i-0dde2e21194e727ae --availability-zone us-east-1c --instance-os-user ec2-user --ssh-public-key file://new_key.pub
   ```

   *After running this command, you only have 60 seconds to log into your ec2 instance. If you miss the window to log in, just run the command again to get 60 more seconds.*

4. Log in to the ec2 instance using it's public IP address and your private key.

   ```bash
   ssh -i ./new_key ec2-user@{public_ip}
   ```

5. Copy your public key into the `~/.ssh/authorized_keys` file on a new line. This allows you to access the instance whenever you need.

   *I opened my public key file in a text editor (notepad) and copied it from there with Ctrl+C.*

You're in! Great job :) You should feel very satisfied with your accomplishments.
