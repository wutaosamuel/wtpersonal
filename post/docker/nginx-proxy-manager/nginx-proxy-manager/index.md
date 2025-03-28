# Nginx Proxy Manager


Nginx Proxy Manager

## Contents

- [Self Signed Certificate](#self-signed-certificate)
- [Cloudflare Certificate](#cloudflare-certificate)

## Install

- create data and configure folders

```bash
mkdir nginxProxyManager
mkdir nginxProxyManager/data
mkdir nginxProxyManager/mysql
mkdir /letsencrypt
mkdir /letsencrypt/data
```

- docker create network

```bash
docker network create reverseProxyNetwork
```

```bash
docker network create nginxProxyManagerNetwork
```

- docker-compose.yml

- start

```bash
docker compose up -d
```

## Self Signed Certificate

There are some of self-host project requiring the https security.
These services might be run on local network without public domain.
Create self signed certificate handles this requirement

### Create Private Key

```bash
openssl genrsa -des3 -out private.key 2048
```

output:

```bash
root@unginx:/home/pi/certs# openssl genrsa -des3 -out private.key 2048
Enter PEM pass phrase:    # enter pass phrase
Verifying - Enter PEM pass phrase:
```

### Generate Public Key

```bash
openssl rsa -in private.key -out server.key
```

output:

```bash
root@unginx:/home/pi/certs# openssl rsa -in private.key -out server.key
Enter pass phrase for private.key:   # enter pass phrase
writing RSA key
```

### Generate Certificate

```bash
openssl req -new -days 3650 -key server.key -out server.csr
```

output:

```bash
root@unginx:/home/pi/certs# openssl req -new -days 3650 -key server.key -out server.csr
Ignoring -days without -x509; not generating a certificate
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:cn
State or Province Name (full name) [Some-State]:zj
Locality Name (eg, city) []:sx
Organization Name (eg, company) [Internet Widgits Pty Ltd]:wt
Organizational Unit Name (eg, section) []:wt
Common Name (e.g. server FQDN or YOUR name) []:*.ubuntunginx.lan  # Important: *.ubuntunginx.lan or ip address
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

### Generate Self Signed Certificate

```bash
openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650
```

output:

```bash
root@unginx:/home/pi/certs# openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650
Certificate request self-signature ok
subject=C = cn, ST = zj, L = sx, O = wt, OU = wt, CN = *.ubuntunginx.lan
```

### Make Use

```bash
root@unginx:/home/pi/certs# ls
private.key  server.crt  server.csr  server.key
```

Directly add server.crt and server.key to **SSL Certificate -> Add SSL Certificate**

OR

In nginx configuration

```bash
# /etc/nginx/snippets/self-signed.conf
ssl_certificate /home/pi/certs/server.crt
ssl_certificate_key /home/pi/certs/server.key
```

## Cloudflare Certificate

Create Certificate at **$YourDomain -> SSL/TLS -> Origin Server -> Create Certificate**

