+++
date = '2025-05-10'
draft = false
title = 'swap-setup'
slug = 'fb3691c9-7d5a-4eaf-bf77-99b43b135a25'
+++
Swap can transform the disk to memory.
```bash
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
To make the swap permanet,We need to edit the `/etc/fstab` file.
```fstab
/swapfile none swap sw 0 0
```