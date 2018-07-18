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
/etc/thor/environ
```
```
GOROOT=/usr/local/go
GOPATH=/usr/local/thor
PATH=$GOPATH/bin:$GOROOT/bin:$PATH
THORARGS="--network test"
```

If you want to switch in between Vechain's main and test net, just change the thor arguments in the environ file.
```
THORARGS="--network test"
THORARGS="--network main"
```
```
solo client runs in solo mode for test & dev
bin/thor solo --on-demand               # create new block when there is pending transaction
bin/thor solo --persist                 # save blockchain data to disk(default to memory)
bin/thor solo --persist --on-demand     # two options can work together
```

### commands
```
systemctl status thor
```
```
● thor.service - Thor VeChain Daemon
   Loaded: loaded (/etc/systemd/system/thor.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-07-18 09:42:07 UTC; 41min ago
 Main PID: 5345 (thor)
    Tasks: 15
   Memory: 419.3M
      CPU: 14.835s
   CGroup: /system.slice/thor.service
           └─5345 /usr/local/thor/bin/thor --network test

Jul 18 10:21:29 vthor-chain thor[5345]: t=2018-07-18T10:21:29+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=129.856µs|343.06µs mgas/s=0.000 id=[#189475…1829d56f]
Jul 18 10:21:39 vthor-chain thor[5345]: t=2018-07-18T10:21:39+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=455.641µs|353.739µs mgas/s=0.000 id=[#189476…ef0e796b]
Jul 18 10:21:50 vthor-chain thor[5345]: t=2018-07-18T10:21:50+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=152.974µs|448.72µs mgas/s=0.000 id=[#189477…f21009d9]
Jul 18 10:21:59 vthor-chain thor[5345]: t=2018-07-18T10:21:59+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=758.219µs|456.086µs mgas/s=0.000 id=[#189478…ab2a58f8]
Jul 18 10:22:09 vthor-chain thor[5345]: t=2018-07-18T10:22:09+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=188.573µs|465.877µs mgas/s=0.000 id=[#189479…0bf526ff]
Jul 18 10:22:19 vthor-chain thor[5345]: t=2018-07-18T10:22:19+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=420.698µs|317.048µs mgas/s=0.000 id=[#189480…4498a254]
Jul 18 10:22:29 vthor-chain thor[5345]: t=2018-07-18T10:22:29+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=431.503µs|313.412µs mgas/s=0.000 id=[#189481…5eabbd08]
Jul 18 10:22:39 vthor-chain thor[5345]: t=2018-07-18T10:22:39+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=454.416µs|320.077µs mgas/s=0.000 id=[#189482…24018be9]
Jul 18 10:22:49 vthor-chain thor[5345]: t=2018-07-18T10:22:49+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 et=116.818µs|275.931µs mgas/s=0.000 id=[#189483…89a9367d]
Jul 18 10:22:59 vthor-chain thor[5345]: t=2018-07-18T10:22:59+0000 lvl=info msg="imported blocks (1)" pkg=node txs=0 mgas=0.000 e
```

```
systemctl status stop
```

Stop the thor deamon 

```
● thor.service - Thor VeChain Daemon
   Loaded: loaded (/etc/systemd/system/thor.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Wed 2018-07-18 10:30:15 UTC; 1s ago
  Process: 5780 ExecStart=/usr/local/thor/bin/thor $THORARGS (code=exited, status=0/SUCCESS)
 Main PID: 5780 (code=exited, status=0/SUCCESS)

Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="exit signal received" signal=terminated
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="stopping communicator..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="stopping P2P server..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="saving peers cache..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="stopping API server..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="closing tx pool..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="closing log database..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg="closing main database..."
Jul 18 10:30:15 vthor-chain thor[5780]: t=2018-07-18T10:30:15+0000 lvl=info msg=exited
Jul 18 10:30:15 vthor-chain systemd[1]: Stopped Thor VeChain Daemon.
```
```
systemctl status start
```
Start the thor deamon 
```
● thor.service - Thor VeChain Daemon
   Loaded: loaded (/etc/systemd/system/thor.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-07-18 10:30:54 UTC; 2s ago
 Main PID: 5806 (thor)
    Tasks: 13
   Memory: 31.0M
      CPU: 95ms
   CGroup: /system.slice/thor.service
           └─5806 /usr/local/thor/bin/thor --network test

Jul 18 10:30:54 vthor-chain thor[5806]: t=2018-07-18T10:30:54+0000 lvl=warn msg="low fd limit, increase it if possible" limit=1024
Jul 18 10:30:54 vthor-chain thor[5806]: Starting Thor/v1.0.0-e8260bf-dev/linux/go1.10.3
Jul 18 10:30:54 vthor-chain thor[5806]:     Network      [ 0x000000000b2bce3c70bc649a02749e8687721b09ed2e15997f466536b20bb127 test
Jul 18 10:30:54 vthor-chain thor[5806]:     Best block   [ 0x0002e45771dac9e99dbed69216d9c7363c711c5c05721caa5ab7768511a46043 #189
Jul 18 10:30:54 vthor-chain thor[5806]:     Master       [ 0x0111745bb560b2f6f507e0cd959c1e5d7ba3c64c ]
Jul 18 10:30:54 vthor-chain thor[5806]:     Beneficiary  [ not set, defaults to endorsor ]
Jul 18 10:30:54 vthor-chain thor[5806]:     Instance dir [ /root/.org.vechain.thor/instance-7f466536b20bb127 ]
Jul 18 10:30:54 vthor-chain thor[5806]:     API portal   [ http://127.0.0.1:8669/ ]
Jul 18 10:30:54 vthor-chain thor[5806]: t=2018-07-18T10:30:54+0000 lvl=info msg="starting P2P networking"
Jul 18 10:30:56 vthor-chain thor[5806]: t=2018-07-18T10:30:56+0000 lvl=info msg="waiting for synchronization..." pkg=node
```
