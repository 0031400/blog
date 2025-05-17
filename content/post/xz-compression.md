+++
date = '2025-05-10'
draft = false
title = 'xz-compression'
slug = 'ae68fef2-9562-4eea-a4c4-ee650b945ff9'
+++
xz is a emerging compression method.But the linux is not equipped with it by default.We need to run `sudo apt install xz-utils` to download the xz.
### compress
```bash
tar cJf <filename.tar.xz> <files>
```
|parameter|meaning|
|-|-|
|c|compression|
|J|xz|
|f|filename|

We need to put the parameter `f` to the last.