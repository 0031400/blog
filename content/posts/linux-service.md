+++
date = '2025-04-25'
draft = false
title = 'Linux Service'
slug = 'linux-service'
+++
### summary
linux is a popular system used in server.To make program keep running after you stop the ssh connection,you have two methods.One of them,your can use the script `nohup ... &`,but when the server reboot ,the program will not run again.And you can't see the log if you don't leave the log file.The default log file `nohup.out` is very easy to get comfused.Another method is to set up a system service.With this method,you can let it start after the server reboot.And you can conveniently get the status and log of the program
### service file
```service
[Unit]
Description=<service name>
After=network.target

[Service]
ExecStart=<the command to run the program>
Restart=always
User=<user>
Group=<group>

[Install]
WantedBy=multi-user.target
```
### setup
we need to put the file to `/etc/systemd/system` and name it like `halo.service`
### command
`systemctl status <name>` to see its status  
`systemctl start <name>` to start it  
`systemctl restart <name>` to restart it  
`systemctl stop <name>` to stop it  
`systemctl enable <name>` to let it run after reboot  