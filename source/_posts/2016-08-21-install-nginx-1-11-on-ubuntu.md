---
title: Implementing ECDSA/RSA Dual Certificate with Free Let's Encrypt
tags:
  - Safety
  - Internet
id: '1904'
categories:
  - - Development
date: 2016-08-21 10:52:13
languages:
  zh-CN: https://guozeyu.com/2016/08/install-nginx-1-11-on-ubuntu/
cover: https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/e8b62e3d-921d-453e-1b56-20cd930fa500/large
---

The software source that comes with Ubuntu 16.04.01 is Nginx 1.10.0, but there are bugs in the HTTP/2 module of this version of Nginx, [see here for details](https://imququ.com/post/nginx-http2 -post-bug.html). Now Nginx 1.12 Stable has been launched, just install the Stable version directly. Update 2018-06: If you are using Ubuntu 18.04 or later, the default Nginx version (1.14) from the system's repositories is sufficient.
<!-- more -->

Regarding dual certificates, **only recommended for those who use independent IP**, if there is no independent IP, then the SNI function needs to be enabled - however almost all browsers that support SNI function also support ECC certificates, so you can skip to After the upgrade steps, directly replace the ECC certificate of Let's Encrypt without using the RSA certificate. I have more than one server, and it would be too troublesome to use Nginx compiled by myself, so I decided to use the method of adding software sources and upgrade through `apt`, the method is as follows: First, you need to add the software sources of Nginx mainline:

```
$ sudo add-apt-repository ppa:nginx/stable # sudo apt install software-properties-common
$ sudo apt update
```

Then remove the existing Nginx and install the new version:

```
$ sudo apt remove nginx nginx-common nginx-core
$ sudo apt install nginx
```

During installation, you may be asked whether to replace the original default configuration file, just select N. The Nginx installed at this time already contains almost all necessary and common modules, such as but not limited to GeoIP Module, HTTP Substitutions Filter Module, and HTTP Echo Module. The OpenSSL version of Nginx I installed is 1.0.2g-fips, so it does not support CHACHA20. If you want to support CHACHA20, you can only use [CloudFlare's Patch](https://github.com/cloudflare/sslconfig) and compile it yourself. After the installation is complete, you can verify the Nginx version:

```
$ nginx -V
nginx version: nginx/1.12.0
built with OpenSSL 1.0.2g 1 Mar 2016
TLS SNI support enabled
configure arguments: --with-cc-opt='-g -O2 -fPIE -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt ='-Wl,-Bsymbolic-functions -fPIE -pie -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx /nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock /nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib /nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module - -with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_geoip_module=dynamic --with-http_gunzi p_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_xslt_module=dynamic --with-stream=dynamic --with-stream_ssl_module --with-stream_ssl_preread_module --with-mail=dynamic - -with-mail_ssl_module --add-dynamic-module=/build/nginx-DYnRGx/nginx-1.12.0/debian/modules/nginx-auth-pam --add-dynamic-module=/build/nginx-DYnRGx/nginx -1.12.0/debian/modules/nginx-dav-ext-module --add-dynamic-module=/build/nginx-DYnRGx/nginx-1.12.0/debian/modules/nginx-echo --add-dynamic- module=/build/nginx-DYnRGx/nginx-1.12.0/debian/modules/nginx-upstream-fair --add-dynamic-module=/build/nginx-DYnRGx/nginx-1.12.0/debian/modules/ngx_http_substitutions_filter_module
```

At this point, your server has no Nginx HTTP/2 bugs. Since you use the latest version of Nginx, you can configure the ECDSA/RSA dual certificate.

### Nginx Upgrade Pit

When I upgraded, I encountered the problem that the GeoIP module could not be used. After research, it was found that the new version changed GeoIP to a dynamic call module method. Adding the following code to the `http {}` configuration of Nginx solved it:

```
load_module "modules/ngx_http_geoip_module.so";
```

## Issue Free Multi-Domain Certificates with Let's Encrypt

Let's Encrypt provides a completely free and automatically issued certificate. A certificate can sign up to 100 domain names, and wildcarding is not supported. In order to configure dual certificates, you should first issue two certificates. The following takes [acme.sh](https://github.com/Neilpang/acme.sh) as an example, first create a directory (all the following cases use ` example.com` As an example, the actual use needs to be replaced by yourself):

```
$ mkdir -p /etc/letsencrypt
$ mkdir -p /etc/letsencrypt/rsa
$ mkdir -p /etc/letsencrypt/ecdsa
```

Then modify the Nginx configuration file to ensure that all those listening on port 80 have the `location ^~ /.well-known/acme-challenge/` block. This configuration file is a case of forcing a jump to HTTPS. This is the configuration of the origin site :

```
server {
listen 80 default_server;
listen [::]:80 default_server;
location ^~ /.well-known/acme-challenge/ {
root /var/www/html;
}
location / {
# Redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
return 301 https://$host$request_uri;
}
}
```

Before issuing, make sure all domain names to be issued point to your own server! Then issue an RSA certificate (if you need a multi-domain certificate, you only need multiple `-d`, the same below, but the saved file directory and certificate display name are the first domain name):

$ acme.sh --issue --reloadcmd "nginx -s reload" -w /var/www/html -d example.com --certhome /etc/letsencrypt/rsa

Then issue the ECDSA certificate:

$ acme.sh --issue --reloadcmd "nginx -s reload" -w /var/www/html -d example.com -k ec-256 --certhome /etc/letsencrypt/ecdsa

Uninstall the cron that comes with `acme.sh` and reconfigure it yourself:

$ acme.sh --uninstallcronjob
$ vim /etc/cron.d/renew-letsencrypt

Enter the following, taking care to replace the path of `acme.sh` with the absolute path of your installation:

```
15 02 * * * root /path/to/acme.sh --cron --certhome /etc/letsencrypt/rsa
20 02 * * * root /path/to/acme.sh --cron --ecc --certhome /etc/letsencrypt/ecdsa
```

Then you're done, and the certificate automatically renews.

### Add or Delete Domain Name to Certificate

Because Let's Encrypt does not use wildcard domain names, it often encounters new subdomains. At this time, you need to add a domain name to the certificate. A certificate can add up to 100 domain names. The easiest way to add is as follows: First, modify the configuration file of the certificate. The configuration files of both certificates must be modified:

```
$ vim /etc/letsencrypt/rsa/example.com/example.com.conf
$ vim /etc/letsencrypt/ecdsa/example.com\_ecc/example.com.conf
```

Find the `Le_Alt` line and add the new domain names to the back (each domain name is separated by a comma, the total cannot exceed 100). Then to start re-issuing this certificate, you need to add `-f`.

```
$ acme.sh --renew -d example.com --certhome /etc/letsencrypt/rsa -f
$ acme.sh --renew -d example.com --ecc --certhome /etc/letsencrypt/ecdsa -f
```

It should be noted that the intermediate certificate and root certificate of the ECC certificate issued by Let's Encrypt are currently not ECC certificates, which will be supported in the future. For details, see [Upcoming Features](https://letsencrypt.org/upcoming-features/ #ecdsa-intermediates).

## Configure Nginx

First you need to generate a few keys:

```
$ openssl rand 48 > /etc/nginx/ticket.key
$ openssl dhparam -out /etc/nginx/dhparam.pem 2048
```

Then add the following content into Nginx and put it under the http or server block. Although CHACHA20 is not supported, adding it in has no effect.

```
##
# SSL Settings
##
ssl_certificate /etc/letsencrypt/rsa/example.com/fullchain.cer;
ssl_certificate_key /etc/letsencrypt/rsa/tlo.xyz/example.com.key;
ssl_certificate /etc/letsencrypt/ecdsa/example.com_ecc/fullchain.cer;
ssl_certificate_key /etc/letsencrypt/ecdsa/example.com_ecc/example.com.key;
ssl_session_timeout 1d;
ssl_session_cache shared:SSL:50m;
ssl_session_tickets off;
ssl_dhparam dhparam.pem;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE- RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128- SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256: DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES- CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
ssl_stapling on;
ssl_stapling_verify on;
```

Don't forget to `nginx -s reload` at the end, then [go to SSL Labs](https://www.ssllabs.com/ssltest/index.html) to check the configuration, you can see that the old browsers use RSA certificates (I The server has a dedicated IP, so it can also be accessed without SNI support):

![Supported Clients](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/5d7521dc-aac5-42ad-4fa4-55df51692000/large)

At this point, the ECDSA/RSA dual certificate configuration is complete, and you can view the certificate type in the browser:

![ECDSA certificate](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/e8b62e3d-921d-453e-1b56-20cd930fa500/large)
