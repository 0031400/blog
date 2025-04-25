---
date: '2025-04-24'
draft: false
title: 'linux swap'
slug: 'linux-swap'
---
### summary
linux swap is a method to use the hard disk as running memory.By the method,we can run program that needs much memory on the machine that lacks much memory.
### example
In an azure virtual machine which only has 1G running memory,the popular website builder named Halo can't run on it.Because the memory is not enough.Halo is written in java,so that Halo is a program which has a strict requirements for memory.Not to mention,I like to run mysql which is a popular database program on the same machine.Anyway,when I tried to run both of them,the machine is stuck and I can't control it by ssh.In the last,I have to reboot it by the panel supported by azure.
### main
create a file which is 2G in size named swapfile located on the root folder.
```bash
fallocate -l 2G /swapfile
```
set the right permissions on it
```bash
chmod 600 /swapfile
```
format the file as swap
```bash
mkswap /swapfile
```
enable the swap
```bash
swapon /swapfile
```
### make it permanent
write the below sentence on /etc/fstab
```
/swapfile none swap sw 0 0
```
### conclusion
swap is very useful.I like swap.I often set the swap on machine whose disk is more than 8G.