# Generating a Self signed TLS/SSL certificate with OpenSSL
Like the name implies, we'll be creating a self signed TLS certificate for our web application using the cryptography tool OpenSSL

## What is an SSL certificate?
A TLS/SSL certificate is essentially a digital document that Authenticates or provides proof of the identity of a wesite/server. It usually contains a public key and is signed by a Certificate Authority. An SSL is used for the following:
- Encrypting web traffic via HTTPS (TLS)
- Authenticating the server to clients
- Used in SSL termination 

### Generating the certificate
Step 1: Using the command below generate an RSA private key. I've chosen to use a 2048bit sized key but a 4096 key is more secure.
```bash
openssl genrsa -out <key_name>.key 2048
```
Step 2: Create a certificate signing request(CSR). The CSR is a file that is sent to a certificate authority(CA) when requesting an SSL certificate. Using the command below we'll be creating a CSR using the private key we generated in the previous step.
```bash
openssl req -new -key <key_name>.key -out <domain>.csr
```
After that's complete you'd be prompted to fill a bunch of information, it doesn't matter what you fill in since we are signing it ourselves, Though set the `common name` to the name of the domain.

Step 3: Generate the self signed certificate using the CSR and private key using the commmand below
```bash
openssl x509 -req -days 365 -in <domain>.csr -signkey <key_name>.key -out <certificate_name>.crt
```
