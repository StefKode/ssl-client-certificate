# Crypto Setup for using Client Side Certificates

# Use Case
I created this tooling to setup and maintain a private webservice which is for my family only. The workflow produces the following relevant files:
* certs.ca/ca.pem: this is the server CA certificate
* certs.client/client.p12: this is the client certificate

# Usage
## Configuration
First copy/rename the example configuration to get the real *.conf files. Then fill out as needed. The example value should sufficiently indicate the meaning. Note, you need to conform to the Relative Distinguished Names (RDNs) system.

## Certificate Generation
1. run ./ca-create which provides a new CA certificate along with the keys.
2. run ./client-create which provides the client certificate

## Apply on NGINX
* create a directory /etc/nginx/certs
* chown www-data.www-data /etc/nginx/certs (not sure if necessary)
* chmod 700 /etc/nginx/certs (not sure if necessary)
* copy the ca.pem file into this directory
Then append the following lines into your site-enabled file. Here is how I have done it:

```
  listen [::]:XXXXXXX ssl ipv6only=on;                           # managed by Certbot
  listen XXXXXXX ssl;                                            # managed by Certbot
  ssl_certificate /etc/letsencrypt/live/XXXXXXX/fullchain.pem;   # managed by Certbot
  ssl_certificate_key /etc/letsencrypt/live/XXXXXXX/privkey.pem; # managed by Certbot
  include /etc/letsencrypt/options-ssl-nginx.conf;               # managed by Certbot
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;                 # managed by Certbot

  # Require cient certificate for connection
  ssl_client_certificate /etc/nginx/certs/ca.pem;
  ssl_verify_client on;
  access_log /var/log/nginx/nginxcc.log;

  client_max_body_size 20M;
  # add all reverse proxy routes here
  include "/etc/nginx/revproxy/*.location";

```


# Security considerations
1. Openssl assignes each file the recommended file permission setting. Nevertheless I recommend to place the certs.* directories into a password protected archive. I use [darkpack](https://github.com/StefKode/darkpack) for that.

2. If you extend the two script files with functionality please be aware that all passwords are readable in the process environment of the script. Any child process can read them. I decided for the env:* method because its easy and the password do not appear in the process list of the computer.

For any feedback please open a ticket.

Stefan

