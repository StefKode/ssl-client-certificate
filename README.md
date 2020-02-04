# Crypto Setup for using Client Side Certificates

# Use Case
I created this tooling to setup and maintain a webservice with is private for my family only. The workflow produces the following relevant files:
* certs.ca/ca.pem: this is the server CA certificate
* certs.client/client.p12: this is the client certificate

# Usage
## Configuration
First copy/rename the example configuration to get the real *.conf files. Then fill out as needed. The example value should sufficiently indicate the meaning. Note, you need to conform to the Relative Distinguished Names (RDNs) system.

## Certificate Generation
1. run ./ca-create which provides a new CA certificate along with the keys.
2. run ./client-create which provides the client certificate

# Security considerations
1. Openssl assignes each file the recommended file permission setting. Nevertheless I recommend to place the certs.* directories into a password protected archive. I use darkpack for that.

2. If you extend the two script files with functionality please be aware that all passwords are readable in the process environment of the script. Any child process can read them. I decided for the env:* method because its easy and the password do not appear in the process list of the computer.

For any feedback please open a ticket.
Stefan

