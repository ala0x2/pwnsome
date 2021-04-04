# Cisco Packet Tracer

## Routers

- When configuring a router you don't need to type the password, you can just click on an interface then get to the CLI

```bash
## Privleged mode
R1> enable
## Enable timestamp log messages
## Ctrl + Z to get to previous mode
R1#erase startup-config

```

### OSPF 

```bash
# Configure OSPF on a router
R1(config)# router ospf 1
R1(config)# show ip ospf interface
```

### MD5

```bash
# Configure MD5 on a router
R1(config-router)# area 0 authentication message-digest
R1(config)# interface s0/0/0
R1(config-router)# ip ospf message-digest-key 1 md5 MD5pa55
```

### NTP (Network Time Protocol)

```bash
# NTP service must be enabled on the computer use as NTP server
# Configure NTP on a router
R1# show ntp status
R1# show clock
R1(config)# ntp server @
R1(config)# ntp update-calendar
R1(config)# do show clock
# Configure NTP authentication on a router
R1(config)# ntp authenticate
R1(config)# ntp trusted-key 1
R1(config)# ntp authentication-key 1 md5 <PASS>
```

### Logging

```bash
R1(config)# service timestamps log datetime msec
# Configure router to identify Syslog server
R1(config)#logging host 192.168.1.6
R1(config)#do show logging
```

### SSH

```bash
# Configure router to support SSH connections

# Configure a domain name
R3(config)#ip domain-name ccnasecurity.com

# Create a user ID of SSHadmin with highest privilege and a secret
R3(config)#username SSHadmin privilege 15 secret ciscosshpa55

# Configure incoming vty lines of local user accounts for mandatory login and validation and to accept only SSH connections.
R3(config)#line vty 0 4
R3(config-line)#login local
R3(config-line)#transport input ssh

# Generate the RSA encryption key pair
R3(config)#crypto key generate rsa

# Verify SSH Configuration
R3#show ip ssh

# Restrictions
R3(config)#ip ssh ?
```