---
layout: post
title: Gerrit Setup with HTTP Authentication
categories: [Operation]
---



- Gerrit Version: gerrit-2.15.13

- Apache Version:

  Server version: Apache/2.4.18 (Ubuntu)

- nginx version: nginx/1.10.3 (Ubuntu)

  

## Gerrit Install and Configure

Follow  [this guide]( https://gerrit-review.googlesource.com/Documentation/install.html) to install Gerrit.

File content of  `$your_site_path/etc/gerrit.config`

```
[gerrit]
	basePath = git
	serverId = 4cb9a1e9-7e5e-406d-806e-e0a84a126541
	canonicalWebUrl = http://review.example.com:8080/
[database]
	type = h2
	database = /home/gerrit/gerrit_path/db/ReviewDB
[noteDb "changes"]
	disableReviewDb = true
	primaryStorage = note db
	read = true
	sequence = true
	write = true
[index]
	type = LUCENE
[auth]
	type = HTTP
[receive]
	enableSignedPush = false
[sendemail]
	smtpServer = localhost
	smtpUser = smtp
[container]
	user = gerrit
	javaHome = /usr/lib/jvm/java-8-openjdk-amd64/jre
[sshd]
	listenAddress = *:29418
[httpd]
	listenUrl = proxy-http://127.0.0.1:8081/gerrit/
[cache]
	directory = cache
```

Be careful about `canonicalWebUrl`, it is the real address that refer to gerrit service, here in my file is domain name,  cause I had add <address,server-name> in `/etc/hosts` file. You can  either  set your server domain name on your DNS server, as long as it mapping to the correct IP address.



```
127.0.0.1	localhost
127.0.1.1	ubuntu
192.168.31.109 review.example.com
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```



## Apache Configure

Follow [this guide]( https://vitux.com/how-to-install-and-configure-apache-web-server-on-ubuntu/) to install Apache.

`your_site.conf` in `/etc/apache2/sites-available/`, which have done this  command

`a2ensite /etc/apache2/sites-available/your_site.conf` 

before.

```h
<VirtualHost *:80>
	  ServerName review.example.com
	  ProxyRequests Off
	  ProxyVia Off
	  ProxyPreserveHost On
	  <Proxy *>
		Order deny,allow
		Allow from all
	  </Proxy>
	  <Location /gerrit/login/>
		AuthType Basic
		AuthName "Gerrit Code Review"
		Require valid-user
		AuthUserFile '/etc/apache2/gerrit.htpasswd'
	</Location>
	  ProxyPass /gerrit/ http://127.0.0.1:8081/gerrit/
</VirtualHost>

```

In which `AuthUserFile '/etc/apache2/gerrit.htpasswd'` is the file which store the user accounts of gerrit.

Use following commands to create users(assume you have apache2 installed)

```shell
sudo htpasswd -c /etc/apache2/gerrit.htpasswd admin # Create first user, need -c
sudo htpasswd /etc/apache2/gerrit.htpasswd $username # Create normal user
```



## Nginx Configuration

File `/etc/nginx/conf.d/gerrit.conf` is like this:

```
server {
   listen 80;
   server_name review.example.com;
   
   location ^~ /gerrit/ {
     auth_basic "Gerrit User Authentication";
     auth_basic_user_file /etc/apache2/gerrit.htpasswd;
     proxy_pass        http://127.0.0.1:8081;
     proxy_set_header  X-Forwarded-For $remote_addr;
     proxy_set_header  Host $host;
           }
} 

```

The auth_basic_user_file configuratioin is same as apache.





## Firewall Configure

Pay attention to configure your firewall rule,  which user `ufw` command to configure on Ubuntu, make sure that your firewall rules does allow your server listen to certain ports of gerrit configured.

```shell
ufw allow $your_app_name
ufw allow $your_port
```



Finally, I open my firefox browser, and type `review.example.com/gerrit/`,  I can log to the gerrit website, notice that `/gerrit/` must be added in the end of website address, cause in config file it is a  reverse proxy which set `/gerrit/` as sub-directory.

## Reference

[https://gerrit-review.googlesource.com/Documentation/install.html](https://gerrit-review.googlesource.com/Documentation/install.html)

[https://www.gerritcodereview.com/install.html](https://www.gerritcodereview.com/install.html)

[https://www.gerritcodereview.com/config-reverseproxy.html](https://www.gerritcodereview.com/config-reverseproxy.html)

[https://vitux.com/how-to-install-and-configure-apache-web-server-on-ubuntu/](https://vitux.com/how-to-install-and-configure-apache-web-server-on-ubuntu/)

[https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/)