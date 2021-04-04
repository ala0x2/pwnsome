# iptables

> A user-space utility program that allows sysadmins to configure IP packet filter rules of the Linux kernal firewall using the `netfliter` framework.

- Filters packets using `filter` table
- NAT (Network Address Resolution) using `nat` table
- Alters IP headers of packets using `mangle` table

```bash
# List chains and default policies
iptables -L

# Change default policy
iptables -P FORWARD ACCEPT

# Append rule
iptables -A CHAINNAME rule
# Insert one or more rules, by default rulename=1
iptables -I CHAINNAME [rulenum] rule
# Replace a rule in a chain
iptables -R CHAINNAME rulenum rule
# Delete a rule in a chain
iptables -D CHAINNAME rulenum 
iptables -D CHAINNAME rule
# Check a rule
iptables -C CHAINNAME rule

# Add chain
iptables [-t table] -N CHAINNAME
# Delete chain 
iptables [-t table] -X CHAINNAME
# Rename a chain
iptables [-t table] -E OLDNAME NEWNAME
# Flush selected chain / all chains
iptables -F [chain]

# Block outgoing traffic (HTTP port 80 and HTTPS port 443). 
iptables -A OUTPUT -p tcp -m multiport --dports 433,80 -j DROP
# Block incoming FTP TELNET SSH traffic
iptables -A INPUT -p tcp -m multiport --dports 21,22,23 -j DROP

# -s sourceIP
# -d destinationIP
# -m state --state NEW, ESTABLISHED, RELATED, INVALID
# -m mac --mac-source
# -m mac --mac-destination
# -m limit --limit mb/s
# -m limit --limit-burst bursts

# echo-request 
# time-exceeded
# destination-unreachable

# Log 
iptable ... -j --log-prefix "message"

# SYN Flooding
iptables -A INPUT -p tcp -syn -m limit --limit 5/s -j ACCEPT
iptables -A INPUT -p tcp -syn -j DROP

# Test if only SYN is set
–tcp-flags SYN,ACK,FIN SYN 

# Backup
iptables-save > file.txt
# Restore
iptables-restore < file.txt
iptables-persistent

iptables -t nat -A PREROUTING -d 82.140.102.18 -j DNAT --to-destination 192.168.0.2
iptables -t nat - A POSTROUTING -s 192.168.0.2 -j SNAT --to-source 82.140.102.18
```