---
date: '2025-04-24'
draft: false
title: 'cloudflared'
slug: 'cloudflared'
---
### summary
cloudflared is a tool launched by the greatest cdn company wordwide cloudflare.It can map out the ports which are stricted in the private network.
### usage
We need to login on cloudflare zero trust panel `https://one.dash.cloudflare.com/` after we have a account and host one of our domains on it.  
After some easy setting on the website,we can get a toke with very long length to config in the remote machine which is a certificate of our account.The next step is to run on machine.
### installation
the product is some open source hosted on the popular online git repository github.We can get it on the link `https://github.com/cloudflare/cloudflared`
### run
we can run it as a one time terminal process
`cloudflared tunnel run --token <your token>`