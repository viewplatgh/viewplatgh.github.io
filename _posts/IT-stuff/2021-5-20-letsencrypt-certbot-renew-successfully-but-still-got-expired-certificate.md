---
layout: post
title: LetsEncrypt Certbot renew successfully, but still got expired certificate from client(browser, curl or wget) side, what's the problem?
categories:
  - it-stuff
tags:
  - certbot
  - letsencrypt
---

This article is just a trouble-shooting blog.
It's rarely happen. After you even did a force renew by using

```console
sudo certbot renew --force-renewal
```

And then check the certificate by using

```console
sudo certbot certificates
```

Everything looks just fine.
But when you use curl or wget from a remote machine to test the website, you still get the error of expired certificate. This can be frustrating.

```console
wget -O test.html https://yourwebsite.com

Resolving yourwebsite.com (yourwebsite.com)... xx.xx.xx.xx
Connecting to yourwebsite.com (yourwebsite.com)|xx.xx.xx.xx|:443... connected.
ERROR: cannot verify yourwebsite.com's certificate, issued by ‘/C=US/O=Let's Encrypt/CN=Let's Encrypt Authority X3’:
  Issued certificate has expired.
To connect to yourwebsite.com insecurely, use `--no-check-certificate'.
```

However, don't worry. The solution is simple. Simply restart the nginx server can save it.

```console
sudo systemctl restart nginx
```

Everything should go back to normal now.
