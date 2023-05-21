# Enable SSH

- Cisco switches have 0 - 15 vty lines
- Cisco routers have 0 - 4 vty lines

Verify SSH support

```
> show ip ssh
```

Configure IP domain

```
(config)# ip domain-name domain name
```

Generate RSA key pair and enable SSH server

```
(config)# crypto key generate rsa # give 1024 when prompted or accept default 512
```

- Note: if you need to delete an RSA key pair and disable the SSH server, run

```
(config)# crypto key zeroize rsa
```
Configure user authentication (local)

```
(config)# username {username} secret {secret}
```

Configure vty lines

```
(config)# line vty 0 15
(config-line)# transport input ssh 
(config-line)# login local
(config-line)# exit
(config)# ip ssh version 2
(config)# exit
```

View SSH connections to the device

```
# show ssh
```

Configure a Switch Virtual Interface for remote management

```
# conf t
(config)# int vlan {management VLAN number}
(config-if)# ip address {ip} {mask}
(config-if)# no shut
(config-if)# exit
(config)# ip default-gateway {ip}
```


