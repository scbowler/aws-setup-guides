# AWS EC2 Initial Setup

This guide will walk you through provisioning a (Free Tier) EC2 instance on Amazon Web Services.

1. In your browser navigate to [https://aws.amazon.com](https://aws.amazon.com).
    - **NOTE:** If you have not already created an AWS account, do so now. This will require a credit card but nothing in this guide will cost anything for 12 months. If you continue to use your EC2 instance after a year it will cost approximately $10 per month.
1. In the navbar at the top-right of the screen, click the **Sign in to the console** button to sign in.
1. Once signed in you will be looking at the complete list of different AWS services. Locate the **Compute** section of **All services** and click on **EC2**.

    ![Select EC2 from the list of all services](images/aws_ec2_initial_setup/select-ec2-service.png)

1. You should now be on the EC2 Dashboard page, that looks something like the following image:
    - **NOTE:** Yours will probably have zeros for the different resources under the **Resources** section.

    ![EC2 Dashboard Page](images/aws_ec2_initial_setup/ec2-dashboard.png)

1. Click on the orange **Launch Instance** button. From the button's dropdown select **Launch Instance**.

    ![Launch Instance](images/aws_ec2_initial_setup/launch-instance.png)

1. You should now be on the **Step 1: Choose an Amazon Machine Image (AMI)** page. This page is where you select the operating system of your EC2 instance.

    ![Choose Amazon Machine Image](images/aws_ec2_initial_setup/choose-ami.png)

1. Locate **Ubuntu Server 18.04 LTS (HVM), SSD Volume Type** in the list of available options. Then select it by clicking on the blue **Select** button.

    ![Select Ubuntu for your OS](images/aws_ec2_initial_setup/select-ubuntu.png)

1. You should now be on the **Step 2: Choose an Instance Type** page. This page is where you select the power level of your instance.

    ![Choose an Instance Type](images/aws_ec2_initial_setup/choose-instance-type.png)

1. Ensure that **`t2.micro`** is selected and then in the bottom right of the screen click **Next: Configure Instance Details**.

    ![Select t2.micro then click next button](images/aws_ec2_initial_setup/t2micro-next.png)

1. You should now be on the **Step 3: Configure Instance Details** page. This page is for the configuration of your instance.

    ![Configure Instance Details](images/aws_ec2_initial_setup/configure-instance-details.png)

1. **DO NOT** change anything on this page, the defaults are good. Click **Next: Add Storage** in the bottom right of the screen.

    ![Click Next Add Storage](images/aws_ec2_initial_setup/next-add-storage.png)

1. You should now be on the **Step 4: Add Storage** page. This page is where you select your hard drive size and type.

    ![Add Storage](images/aws_ec2_initial_setup/add-storage.png)

1. Change the storage **Size (GiB)** from `8` to `16`.

    ![Increase storage size to 16 GiB](images/aws_ec2_initial_setup/increase-storage-size.png)

1. Click **Next: Add Tags** in the bottom right corner.

    ![Next add tags](images/aws_ec2_initial_setup/next-add-tags.png)

1. You should now be on the **Step 5: Add Tags** page.

    ![Add Tags](images/aws_ec2_initial_setup/add-tags.png)

1. There is nothing to do on this page. Click the **Next: Configure Security Group** button.

    ![Next Configure Security Group](images/aws_ec2_initial_setup/next-configure-security-group.png)

1. You should now be on the **Step 6: Configure Security Group** page. This page is where you configure your instance's firewall. You can set which ports are open to your instance and from which IP addresses.

    ![Configure Security Group](images/aws_ec2_initial_setup/configure-security-group.png)

1. Click the **Add Rule** button. On the newly added rule select `HTTP` under **Type**. This will allow access to and from your instance on port `80`.

    ![Add Rule to Security Group](images/aws_ec2_initial_setup/add-rule.png)
    ![Add HTTP rule](images/aws_ec2_initial_setup/add-http-rule.png)

1. Click the **Add Rule** button again. On the newly added rule select `HTTPS` under **Type**. This will allow access to and from your instance on port `443`.

    ![Add Rule to Security Group](images/aws_ec2_initial_setup/add-rule-2.png)
    ![Add HTTPS rule](images/aws_ec2_initial_setup/add-https-rule.png)

1. Click the **Review and Launch** button in the bottom right corner.

    ![Review and Launch Button](images/aws_ec2_initial_setup/review-and-launch.png)

1. You should now be on the **Step 7: Review Instance Launch** page. Compare your settings to the image below. The most important sections are shown in the image: AMI Details, Instance Type, and Security Groups.

    ![Review Instance Launch](images/aws_ec2_initial_setup/review-instance-launch.png)

1. Once you have ensured everything is correct, click the blue **Launch** button in the bottom right corner.

    ![Click blue launch button](images/aws_ec2_initial_setup/launch.png)

1. You should see a modal pop up that says **Select an existing key pair or create a new key pair**.
    ### THESE STEPS ARE VERY IMPORTANT!! Without this file you cannot access your EC2 instance!
    1. In the first dropdown select **Create a new key pair**.
    1. In the input give your key pair a name. Name it `aws-ec2`.
        - **NOTE** You can name it something different just be aware it will change some of the below commands. Any command that references the key file needs to match whatever name you choose. If this is your first time you should stick with the suggested name: (`aws-ec2`)
    1. Click the **Download Key Pair** button to get your `aws-ec2.pem` file.

    ![Create Key Pair Modal](images/aws_ec2_initial_setup/key-pair-modal.png)

1. Move your newly created key file to your home directory
    ### On macOS or Linux
    1. Open **Terminal** and navigate to your home directory.
        - `cd ~`
    1. Move your key file to your home directory with the following command:
        - `mv ~/Downloads/aws-ec2.pem ~/aws-ec2.pem`
    1. Verify that the file was moved successfully. Run the following command and verify you see the `aws-ec2.pem` file listed.
        - `ls ~`
    1. Change the permissions for the file
        - `chmod 600 aws-ec2.pem`
    ### On Windows
    1. Open **Git Bash** and navigate to your home directory.
        - `cd ~`
    1. Move your key file to your home directory with the following command:
        - `mv ~/Downloads/aws-ec2.pem ~/aws-ec2.pem`
    1. Verify that the file was moved successfully. Run the following command and verify you see the `aws-ec2.pem` file listed.
        - `ls ~`
    1. On Windows you do not need to change the file permissions.
1. Go back to your browser, click the blue **Launch Instances** button.
1. You should now be on the **Launch Status** page.

    ![Launch Status Page](images/aws_ec2_initial_setup/launch-status.png)

1. Click the blue **View Instances** button on the right.
1. You should now be on the instances page
    - **NOTE:** You will probably only have one instance listed.

    ![Instances page](images/aws_ec2_initial_setup/instances.png)

1. Hover over your instance in the table, in the name column, and a pencil icon should appear. Click the pencil to edit your instance's name. Name it **portfolio**.

    ![Edit instance name](images/aws_ec2_initial_setup/edit-instance-name.png)
    ![Instance name set](images/aws_ec2_initial_setup/instance-name-set.png)

1. In the left hand menu find the **Network & Security** section, click on the **Elastic IPs** link.

    ![Elastic IPs Link](images/aws_ec2_initial_setup/elastic-ips-link.png)

1. On the **"Elastic IP addresses"** page, click on the orange **Allocate Elastic IP address** button.

    ![Allocate IP address button](images/aws_ec2_initial_setup/allocate-elastic-ip-address-button.png)

1. On the **Allocate Elastic IP address** page, click the orange **Allocate** button on the bottom right of the page.
    - **NOTE:** Your version of this page may look a little different than the image below.

    ![Allocate Button](images/aws_ec2_initial_setup/allocate-button.png)

1. You should now see this page with a green success message box at the top that says **Elastic IP address allocated.**

    ![Elastic IP address allocated](images/aws_ec2_initial_setup/elastic-ip-address-allocated.png)

1. Write down your new IP address, you will need it again.

    ![Your IP Address](images/aws_ec2_initial_setup/your-ip-address.png)

1. Click on the **Actions** button in the top right of the page, then click on the **Associate Elastic IP address** option.

    ![Associate IP address option](images/aws_ec2_initial_setup/associate-ip-address.png)

1. On the **Associate Elastic IP address** page, click on the **Instance** input then select your instance it should say **(portfolio)** next to its ID.

    ![Select Your Instance](images/aws_ec2_initial_setup/select-instance.png)

1. Click the orange **Associate** button at the bottom right of the page.

    ![Associate button](images/aws_ec2_initial_setup/associate.png)

1. In the left side menu under the **Instances** section click on the **Instances** link.

    ![Instances Link](images/aws_ec2_initial_setup/instances-link.png)

1. In the table of the **Instances** view you should see your Elastic IP Address associated with your instance. You may need to scroll to the left depending on your screen size.

    ![Instance with associated elastic IP Address](images/aws_ec2_initial_setup/instance-with-elastic-ip.png)

1. Now it's time to log into your instance!
    - ### macOS or Linux
        - Open **Terminal**
    - ### Windows
        - Open **Git Bash**
    - Run the following command in your terminal, but use the Elastic IP address you allocated instead:
        - `ssh -i ~/aws-ec2.pem ubuntu@52.13.121.237`
    - Type `yes` you want to continue and press enter.
    - If everything worked you should see the following output in your terminal and your prompt should now say `ubuntu@ip-172-31-24-185:~$` *(Your IP may vary)*:
        ![Terminal Logged into instance](images/aws_ec2_initial_setup/terminal-logged-in.png)

### NOTE: To log out of your instance type `exit` then press `enter`

## You are done! You have successfully set up an EC2 instance with an Ubuntu OS!

Now that you have completed your setup for your EC2 instance, please setup an "A record" using your new IP Address and "CNAME records" for any web applications you are ready to deploy by clicking [here.](./DNS_SETUP.md)
