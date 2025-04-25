+++
date = '2025-04-25'
draft = false
title = 'Ssh Hugo'
slug = 'ssh-hugo'
+++
### summary
hugo is a popular static site builder.We need to edit the site file in markdown format in our computer.When we want to publish our site,we need to push the code to the remote computer.So that,for convenience,I write a bash script to automatically complete the task.
### code
In the below code,I run the cloudflared,because my remote host has only ipv6,and my virtual machine can't access the ipv6 network conveniently.Through cloudflared,I use it to get achieve an effect like proxy.
```bash
#!/bin/bash
HUGO_DIR=<your hugo dir>
REMOTE_USER=<your remote machine user>
WRB_DIR=<your website dir>
CLOUDFLARED_HOSTNAME=<your cloudflared domain>
LOCAL_PORT=<the port to establish the network bridge with cloudflare>
CLOUDFLARED_PATH=/home/lxz/app/cloudflared/cloudflared

# 暂时性使用cloudflared
nohup $CLOUDFLARED_PATH access tcp --hostname $CLOUDFLARED_HOSTNAME --url 127.0.0.1:$LOCAL_PORT >/dev/null 2>/dev/null &

cd $HUGO_DIR
# 构建
hugo
# 上传到github
git add .
git commit -m "$(date)"
git push
# 压缩
cd $HUGO_DIR/public
tar -cf site.tar.gz *
# sftp 上传
sftp -P $LOCAL_PORT $REMOTE_USER@127.0.0.1  <<EOF
cd /home/$REMOTE_USER
put site.tar.gz
bye
EOF
# ssh 控制
ssh "$REMOTE_USER@127.0.0.1" -p "$LOCAL_PORT" "
    cd $WRB_DIR && \
    rm -rf ./* && \
    mv /home/$REMOTE_USER/site.tar.gz . && \
    tar xf site.tar.gz && \
    rm site.tar.gz
"

# 关闭 cloudflared
pkill -f cloudflared
# 删除本地文件
rm site.tar.gz
```