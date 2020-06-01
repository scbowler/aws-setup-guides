# DNS Setup

This guide will walk you through setting up your DNS for your domain name.

## Adding an A Record

A Records are the most basic type of DNS record and are used to point a domain or subdomain to an IP address.  You only need to do this the first time you are setting up your EC2 instance.  Once you have received your elastic ip address you must setup an `A Record` to have your domain name service point to ip address where your nginx server is hosted.

### Setting an A Record on namecheap.com

1. Login to namecheap.com and land on account page

1. Click `Manage` on the domain name that you want to point to your server.

    ![namecheap home](images/dns_setup/namecheap-1.png)

1. Click on `Advanced DNS`

    ![namecheap domain manage](images/dns_setup/namecheap-2.png)

1. Click `Add New Record`

    ![namecheap add new record](images/dns_setup/namecheap-3.png)

1. Click the green check mark once you have added the following fields

    1. Set the first drop down to `A Record`
    1. The second field is `@` (the @ symbol represents your root domain.)
    1. The third field is the elastic ip address you got from aws.

    ![namecheap domain manage](images/dns_setup/namecheap-4.png)

### Setting an A Record on name.com

1. Login to name.com and land on the account page

1. Click on the domain name that you want to point to your server.

    ![name home](images/dns_setup/name-1.png)

1. Click on `Manage DNS Records`

    ![name domain manage](images/dns_setup/name-2.png)

1. Click the blue `Add Record` button once you have added the following fields

    1. Set the first drop down to `A Record`
    1. The second field is your domain name
    1. The third field is the elastic ip address you got from aws.

    ![name domain manage](images/dns_setup/name-3.png)

### Setting an A Record on hover.com

1. Login to hover.com and land on the account page

1. Click on the edit drop down of the domain name that you want to point to your server.

    ![hover domains](images/dns_setup/hover-1.png)

1. Click on `Edit DNS`

    ![hover domain dns](images/dns_setup/hover-2.png)

1. Click the `Add A Record` button.

    ![hover domain manage](images/dns_setup/hover-3.png)

1. Click the `Add Record` after inputting the following data

    1. Set the `TYPE` drop down to `A`
    1. Set `HOSTNAME` to your domain name
    1. Set `IP ADDRESS` to the elastic ip address you got from aws.

    ![hover domain manage](images/dns_setup/hover-4.png)

### Adding a CNAME Record

The DNS CNAME record works as an alias for domain names that share a single IP address.  Every time you want to deploy a web application for your portfolio, you will need to do this.

### Setting a CNAME Record on namecheap.com

1. Login to namecheap.com and land on account page

1. Click `Manage` on the domain name that you want to create the CNAME record for.

    ![namecheap home](images/dns_setup/namecheap-1.png)

1. Click on `Advanced DNS`

    ![namecheap domain manage](images/dns_setup/namecheap-2.png)

1. Click `Add New Record`

    ![namecheap add new record](images/dns_setup/namecheap-3.png)

1. Click the green check mark once you have added the following fields

    1. Set the first drop down to `CNAME Record`
    1. The second field is your subdomain.  For example if your address is `memory-match.yourdomainhere.com` then your subdomain would be `memory-match`.
    1. The third field is the root address of your domain. For example if your address is `memory-match.yourdomainhere.com` then your root would be `yourdomainhere.com`.

    ![namecheap domain manage](images/dns_setup/namecheap-5.png)

### Setting a CNAME Record on name.com

1. Login to name.com and land on the account page

1. Click on the domain name that you want to create the CNAME record for.

    ![name home](images/dns_setup/name-1.png)

1. Click on `Manage DNS Records`

    ![name domain manage](images/dns_setup/name-2.png)

1. Click the blue `Add Record` button once you have added the following fields

    1. Set the first drop down to `CNAME`
    1. The second field is your domain name with the subdomain you want.
    1. The third field is the root address of your domain.

    ![name domain manage](images/dns_setup/name-4.png)

### Setting a CNAME Record on hover.com

1. Login to hover.com and land on the account page

1. Click on the edit drop down of the domain name that you want to create the CNAME record for.

    ![hover domains](images/dns_setup/hover-1.png)

1. Click on `Edit DNS`

    ![hover domain dns](images/dns_setup/hover-2.png)

1. Click the `Add A Record` button.

    ![hover domain manage](images/dns_setup/hover-3.png)

1. Click the `Add Record` after inputting the following data

    1. Set the `TYPE` drop down to `CNAME`
    1. Set `HOSTNAME` to your subdomain.  For example if your address is `memory-match.yourdomainhere.com` then your subdomain would be `memory-match`.
    1. Set `TARGET NAME` to the root address of your domain. For example if your address is `memory-match.yourdomainhere.com` then your root would be `yourdomainhere.com`..

    ![hover domain manage](images/dns_setup/hover-5.png)
