# thor-deamon


Creating and Modifying thor systemd Unit Files.
This can be usefull in order to be able to use standard systemctl status commands and in order to start the deamon on reboot.

### thor.service
```
/etc/systemd/system/
```
```
[Unit]
Description=Thor VeChain Daemon
After=network.target auditd.service

[Service]
EnvironmentFile=/etc/thor/environ
ExecStart=/usr/local/thor/bin/thor $THORARGS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
Type=simple

[Install]
WantedBy=multi-user.target
Alias=thor.service
```
### ENV variables
```
/etc/thor/environ/
```
```
GOROOT=/usr/local/go
GOPATH=/usr/local/thor
PATH=$GOPATH/bin:$GOROOT/bin:$PATH
THORARGS="--network test"
```



