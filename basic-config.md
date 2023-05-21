# Basic Config

Perform security configuration

Configure the device name.

```
>hostname name
```

Secure user EXEC mode.

```
(config)# line console 0
(config)# password password
(config)# login
```

Secure remote Telnet / SSH access.

```
(config)# line vty 0 15
(config-line)# password password
(config-line)# login
```

Secure privileged EXEC mode.

```
(config)# enable secret password
```

Secure all passwords in the config file.

```
(config)# service password-encryption
```

Provide legal notification.

````
(config)# banner motd delimiter message delimiter
```

Configure the management SVI.

```
(config)# interface vlan 1
(config-if)# ip address ip-address subnet-mask
(config-if)# no shutdown
```

Save the configuration.

```
# copy running-config startup-config
```