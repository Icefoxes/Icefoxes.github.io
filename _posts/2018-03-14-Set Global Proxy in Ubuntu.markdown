---
layout:       post
title:        "Set Global Proxy in Ubuntu"
subtitle:     "Set Global Proxy in Ubuntu (Command Line)"
date:         2015-03-14 12:00:00
author:       "Kuo"
header-img:   "img/post-bg-ubuntu.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Ubantu
---

### Set Global Proxy in Ubuntu (Command Line)
System-wide proxies in CLI Ubuntu/Server must be set as environment variables.

Open the `/etc/environment` file. This file stores the system-wide variables initialized upon boot.
Add the following lines, modifying appropriately. You must duplicate in both upper-case and lower-case because (unfortunately) some programs only look for one or the other:

```bash
http_proxy="http://myproxy.server.com:8080/"
https_proxy="http://myproxy.server.com:8080/"
ftp_proxy="http://myproxy.server.com:8080/"
no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
HTTP_PROXY="http://myproxy.server.com:8080/"
HTTPS_PROXY="http://myproxy.server.com:8080/"
FTP_PROXY="http://myproxy.server.com:8080/"
NO_PROXY="localhost,127.0.0.1,localaddress,.localdomain.com"
```
`apt-get`, `aptitude`, etc. will not obey the environment variables when used normally with sudo. So separately configure them; create a file called 95proxies in `/etc/apt/apt.conf.d/`, and include the following:

```bash
Acquire::http::proxy "http://myproxy.server.com:8080/";
Acquire::ftp::proxy "ftp://myproxy.server.com:8080/";
Acquire::https::proxy "https://myproxy.server.com:8080/";
Finally, logout and reboot to make sure the changes take effect.
```