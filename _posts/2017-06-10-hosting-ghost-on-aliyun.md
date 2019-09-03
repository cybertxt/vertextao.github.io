---

layout: post
title: "Hosting Ghost on Aliyun ECS"
date: 2017-06-10 21:00:00 +0800

---

# Hosting Ghost on Aliyun ECS

## NGINX
* download nginx source
* download openssl source
* do configuration:

```shell
# example
./configure --with-openssl=../openssl-1.1.0f --with-http_ssl_module
make
make install
```

* now you can visit your site, you will see nginx welcome page!

### SSL for NGINX
use letsencrypt free SSL certificate
* install tools

```shell
yum -y install yum-utils
yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional
yum install certbot

certbot certonly --standalone -d example.com -d www.example.com
```

* here may be an error occur on centos7 like this:

```
raise ImportError("'pyOpenSSL' module missing required functionality. "
ImportError: 'pyOpenSSL' module missing required functionality. Try upgrading to v0.14 or newer.
```

* to fix the error

quote from: [https://github.com/certbot/certbot/issues/4514](https://github.com/certbot/certbot/issues/4514)

```shell
rpm --query centos-release  # centos-release-7-3.1611.el7.centos.x86_64
wget ftp://ftp.muug.mb.ca/mirror/centos/7.3.1611/cloud/x86_64/openstack-mitaka/common/pyOpenSSL-0.15.1-1.el7.noarch.rpm
rpm -Uvh pyOpenSSL-0.15.1-1.el7.noarch.rpm
```

* for successfully obtain the certificate, message like this will be shown:

```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/my-domain/fullchain.pem. Your cert
   will expire on date. To obtain a new version of the
   certificate in the future, simply run Let's Encrypt again.
   ...
```

* configure the nginx to use the certificate

```
server {
    listen 443 ssl default_server;
    server_name my-domain;

    ssl_certificate /etc/letsencrypt/live/my-domain/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/my-domain/privkey.pem;

    ...
}
```

* redirect http to https, add this block

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	server_name my-domain;
	return 301 https://$host$request_uri;
}
```

* automatically renew the certificate
    + write a shell script ```renew-cert.sh```
    + add **crontab** task:
    ```
    0 0 1 JAN,MAR,MAY,JUL,SEP,NOV * /path/to/renew-cert.sh
    ```