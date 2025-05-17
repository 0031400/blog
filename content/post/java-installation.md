+++
date = '2025-05-10'
draft = false
title = 'java-installation'
slug = 'd98f5c91-5980-4f4e-9e18-d114ea7da6ac'
+++
[https://openjdk.org](https://openjdk.org) is the openjdk official website.We can download java from [https://jdk.java.net](https://jdk.java.net) like the version 24 [https://jdk.java.net/24](https://jdk.java.net/24)  
The linux java 24 download link is [https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_linux-x64_bin.tar.gz](https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_linux-x64_bin.tar.gz)  
The windows java 24 download link is [https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_windows-x64_bin.zip](https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_windows-x64_bin.zip)  
We need to add the java bin folder path to our environment variable `PATH`.Edit our `.bashrc` file.
```bash
export PATH=<jdk-path>/bin:$PATH
```