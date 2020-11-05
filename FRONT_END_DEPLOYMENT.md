# Deploying a Front-End Web App to NGINX on an AWS EC2 Host w/Ubuntu 18.04

This guide outlines steps for deploying a web application without any custom back-end code to an NGINX web server running on an AWS EC2 instance with an Ubuntu 18.04 operating system. For instructions on how to deploy a full-stack application, please see the [Full-Stack Deployment Guide](FULL_STACK_DEPLOYMENT.md).

## Required Tools

This guide assumes that you have already [provisioned an AWS EC2 instance with SSH access](AWS_EC2_INITIAL_SETUP.md), with both [NGINX](INSTALL_NGINX_ON_UBUNTU.md) and [certbot](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx) installed on it. It also assumes you have already purchased a domain name.

### Create a Subdomain

Visit your domain name registrar and create a new `CNAME` DNS record for your project. The `CNAME` record should point to your root domain name.

> For example, if your domain name is `yourdomain.com` and your project's name is `code-journal`, then you'll create a `CNAME` record for `code-journal.yourdomain.com` that points to `yourdomain.com`.

For additional help on creating a subdomain, follow the [DNS setup guide](DNS_SETUP.md#adding-a-cname-record).

## Connect to EC2

All of the instructions in this guide require you to issue commands to your EC2 instance in an SSH session.

- On a **Mac OS** or **Linux** computer, open **Terminal**.
- On a **Windows** computer, open **Git Bash**.

Use the following `ssh` command to sign in. Replace `<your ip address>` with the Elastic IP address of your EC2 instance. Replace `path/to/key.pem` with the location of your own SSH key (_e.g._ `~/Desktop/aws-ec2.pem`).

```bash
ssh -i path/to/key.pem ubuntu@<your ip address>
```

> For example, if your SSH key is located on your `Desktop` and is named `aws-ec2.pem`, and your EC2 instance's Elastic IP address is 111.222.333.444, then your command would be:<br>
```bash
ssh -i ~/Desktop/aws-ec2.pem ubuntu@111.222.333.444
```

After successfully connecting, your terminal window should show a prompt that looks something like this:

```bash
Welcome to Ubuntu 18.04...
...
ubuntu@some-ip-address:~$
```

> **Note**: `some-ip-address` is an internal IP address and will be _different_ than your EC2 instance's Elastic IP address.

If you are unable to connect, notify an instructor right away.

### Clone the Project

While connected to your EC2 instance over SSH, you'll want to clone the project's source code into your home directory. Confirm that your current working directory is `/home/ubuntu` with the `pwd` command.

```bash
ubuntu@some-ip-address:~$ pwd
/home/ubuntu
ubuntu@some-ip-address:~$
```

Ubuntu comes with `git` pre-installed, so you can clone the project now. Use the `git clone` command to clone the project, passing it both the `<repository>` and `<directory>` arguments. `<repository>` should equal the project's clone address and `<directory>` should equal the [fully qualified domain name (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), minus the trailing period (`.`), of the subdomain set up with the domain name registrar. If the repository is private, you'll be prompted for your GitHub username and password.

> **Example**: If the project's GitHub repository is `username/project-name` and the project is being deployed to the `blog` subdomain of the `yourdomain.com` root domain, then cloning the project would look like:

```bash
ubuntu@some-ip-address:~$ git clone https://github.com/username/project-name.git blog.yourdomain.com
```

After the project is successfully cloned, running the `ls` command should show the project directory.

```bash
ubuntu@some-ip-address:~$ ls
...     ...                     ...
...     blog.yourdomain.com     ...
...     ...                     ...
ubuntu@some-ip-address:~$
```

### Configure a Virtual Host for NGINX

When web browsers visit your project, they'll be making HTTP requests to your NGINX web server. However, by default NGINX doesn't know HTTP requests to the project's subdomain are valid. It needs to be configured to respond to those requests with the project's files. A special configuration file needs to be created.

#### Create the Configuration File

Use `nano` to create and edit an NGINX config for your project. It should be created in the `/etc/nginx/sites-available/` directory, with a file name equal to the project's [fully qualified domain name (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), minus the trailing period (`.`).

**Note:** The default `ubuntu` user account of your EC2 instance does not have permission to modify files outside of its home directory, so the `nano` command will need to start with `sudo` to temporarily use the `root` user account.

> **Example**:  If the project is being deployed to the `code-journal` subdomain of the `yourdomain.com` root domain, then you'd start editing the file like:

```bash
ubuntu@some-ip-address:~$ sudo nano /etc/nginx/sites-available/code-journal.yourdomain.com
```

Fill out the contents of the file to match the example config below, replacing the value of the `server_name` field with your FQDN.

```conf
server {
    # The following server_name rule should equal the fully qualified domain
    # name for the project, minus the trailing period.
    server_name code-journal.yourdomain.com;

    # The following root rule should equal the full directory path of the
    # project's `index.html` file.
    root /home/ubuntu/$server_name;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

After you're done editing the file's contents, save (write-out) the file, then quit `nano`.

#### Enable the Site

Once your site's NGINX configuration file has been created, it's time to turn it on.

**Note:** The default `ubuntu` user account of your EC2 instance does not have permission to modify files outside of its home directory, so the `ln` command will need to start with `sudo` to temporarily use the `root` user account. Replace `code-journal.yourdomain.com` with your own configuration file's name.

1. Add the site to the list of enabled sites.
    ```bash
    ubuntu@some-ip-address:~$ sudo ln -s /etc/nginx/sites-available/code-journal.yourdomain.com /etc/nginx/sites-enabled/
    ```
1. Test your new configuration file for valid syntax using the `nginx -t` tool. You should see confirmation messages that your configuration is valid.
    ```bash
    ubuntu@some-ip-address:~$ sudo nginx -t
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    ubuntu@some-ip-address:~$
    ```
1. Restart NGINX so it loads the changes into memory.
    ```bash
    ubuntu@some-ip-address:~$ sudo service nginx restart
    ubuntu@some-ip-address:~$
    ```

#### Try it out!

Your project is now deployed! You should be able to visit your fully qualified domain name in a web browser to see the landing page of the app. If your domain has a `.dev` extension you need to complete the next step before viewing your website. That is because the `dev` extension is a secure namespace, so you need HTTPS and an SSL certificate for your website to load on most browsers.

#### Enable SSL with Certbot

At this point, web browsers are communicating with your application over an insecure connection. Let's fix that! Certbot makes it easy to configure SSL for your project with one command.

**Note:** The default `ubuntu` user account of your EC2 instance does not have permission to run the `certbot` command, so it will need to begin with `sudo` to temporarily use the `root` user account.

```bash
ubuntu@some-ip-address:~$ sudo certbot --nginx
```

The following items may be requested of you by `certbot` if this is your first time running it:

1. Your _real_ email address is required for renewal and security notices.
1. You _must_ agree to the Let's Encrypt terms of service.
1. You _may_ opt to receive a newsletter from the EFF. You don't have to.
1. Choose your project for HTTPS activation.
1. Enable redirects to make all requests redirect to secure HTTPS connections.

#### Try it out again!!

Visit your FQDN again in a web browser and you should see a lock in the URL bar indicating that you are visiting the app over a private SSL connection!! 🔒🔒🔒

## Deploying Updates

"Redeploying" your project is required whenever fixes or new functionality has been added to its codebase. This process is much less involved than the initial deployment and the vast majority of it is simple repetition of some steps taken during your first deployment.

To get started, SSH into your EC2 instance.

### Pull the Latest Commits

Change directories to your project; it should be located at `/home/ubuntu/code-journal.yourdomainhere.com`. Change `code-journal.yourdomainhere.com` to your project's subdomain.

```bash
ubuntu@some-ip-address:~$ cd /home/ubuntu/code-journal.yourdomainhere.com
```

Pull the default branch (`main`) of your project's GitHub repository.

```bash
ubuntu@some-ip-address:~/code-journal.yourdomainhere.com$ git pull origin main
```

Now all of your most recent changes are downloaded!

### Done!

Congratulations, your project has bee redeployed. 🎉🎉🎉 **Note:** You may need to "Empty Cache and Hard Reload" in your browser to see the latest updates.
