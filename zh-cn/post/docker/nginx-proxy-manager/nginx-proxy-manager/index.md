# Nginx Proxy Manager


Nginx Proxy Manager

## 目录

- [自签名证书](#自签名证书)

## 自签名证书

有些项目部署需要 https, 但是它们会在局域网上运行, 而且没有公共域名.
所以需要生成自签名证书.

### 创建密钥

```bash
openssl genrsa -des3 -out private.key 2048
```

输出:

```bash
root@unginx:/home/pi/certs# openssl genrsa -des3 -out private.key 2048
Enter PEM pass phrase:              # 输入 pass phrase
Verifying - Enter PEM pass phrase:  # 再次输入 pass phrase
```

### 生成公钥

```bash
openssl rsa -in private.key -out server.key
```

输出:

```bash
root@unginx:/home/pi/certs# openssl rsa -in private.key -out server.key
Enter pass phrase for private.key:   # 输入 pass phrase
writing RSA key
```

### 生成证书

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
Common Name (e.g. server FQDN or YOUR name) []:*.ubuntunginx.lan  # 非常重要: *.ubuntunginx.lan 或者 ip address
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

### 生成自签名证书

```bash
openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650
```

输出:

```bash
root@unginx:/home/pi/certs# openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650
Certificate request self-signature ok
subject=C = cn, ST = zj, L = sx, O = wt, OU = wt, CN = *.ubuntunginx.lan
```

### 创建用于 Diffie-Hellman 交换协议的自定义参数 (可选)

```bash
openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

输出:

```bash
root@VM-12-9-ubuntu:~/ca# openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
Generating DH parameters, 2048 bit long safe prime
.....................................................................................................................................................................................................................................................................................................................................................................+............................++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*
```

### 使用

```bash
root@unginx:/home/pi/certs# ls
private.key  server.crt  server.csr  server.key
```

加入 nginx 配置

```bash
ssl_certificate /home/pi/certs/server.crt
ssl_certificate_key /home/pi/certs/server.key
```

