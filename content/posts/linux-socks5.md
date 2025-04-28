---
date: '2025-04-24'
draft: false
title: 'linux socks5'
slug: 'linux-socks5'
---
### summary
In the cloud machine,I usually need to download many big files which are on other country.In that situation,the download speed is very slow.Therefore,I need to use the proxy on the cloud machine.The most popular network protocol to use is socks5.How to let the program to use the socks5?A tool named proxychains is very convenient to use in system like debian.
### installation
```bash
apt install proxychains
```
### config
you need to edit a file `/etc/proxychains.conf` using text editor like vim , code.Write the below sentence on it.
```conf
[ProxyList]
socks5  127.0.0.1 10808
```
### usage
```bash
proxychains your_program
```