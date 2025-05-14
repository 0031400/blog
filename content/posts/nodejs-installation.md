+++
date = '2025-05-10'
draft = false
title = 'nodejs-installation'
slug = '3f8bd767-5f88-47fd-a58b-283899413ed2'
cover = 'https://i.withme.top/2025/05/10/3f8bd767-5f88-47fd-a58b-283899413ed2.webp'
+++
[https://nodejs.org](https://nodejs.org) is the nodejs official website.  
[https://nodejs.org/dist/v22.15.0/node-v22.15.0-linux-x64.tar.xz](https://nodejs.org/dist/v22.15.0/node-v22.15.0-linux-x64.tar.xz) is the linux nodejs 22.15.0 download file.  
[https://nodejs.org/dist/v22.15.0/node-v22.15.0-x64.msi](https://nodejs.org/dist/v22.15.0/node-v22.15.0-x64.msi) is the windows 22.15.0 download file.  
We need to add the nodejs bin folder path to our environment variable `PATH`.Edit our `.bashrc` file.
```bash
export PATH=<nodejs-path>/bin:$PATH
```