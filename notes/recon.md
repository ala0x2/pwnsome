# Recon

## DNS

### nslookup
```bash
nslookup
> server 8.8.8.8
> set type=ns # mx, ptr, srv, all, any
> domain.com
```

## Network

### Ping sweep

```bash
fping -g 10.10.10.42 10.10.10.98 > hermes.txt
```
```bash
nmap -sP -v 10.10.10.42/24
```

### arping

```bash
arping -c 1 -i wlan0 192.168.1.42
arping -d -c 1 -i wlan0 192.168.1.42 # -d for IP conflicts
```

### nbtscan

Internal pentest through SMB
```bash
nbtscan 10.10.10.0/24
nbtscan 10.10.10.42-98
```
### nmap

- Scans ports in ```nmap-services``` by default (1000 most used)
- ```-sT``` Full TCP scan by default (Normal user)
- ```-sS``` Half (SYN) scan as root (May harm applications)

#### Normal scans

```bash
nmap -oA nmap/init.txt <target>
```
```bash
nmap -p <port1>,<port2> -oA nmap/details.txt <target>
```
```bash
nmap -p- -oA nmap/alltcp.txt <target>
```
```bash
nmap -sV -sC -p- --min-rate 5000 -oA nmap/alltcp.txt <target>
```

#### Idle scan

```bash
# Using a zombie host with incremental seq
# Open port: IPID + 2
# Closed/Filtered port: IPID + 1
nmap -sI 10.10.10.42 10.10.10.98 
```
Finding zombies
```bash
hping3 -S -r 10.10.10.42/24
```
```bash
msf> use auxiliary/scanner/ip/ipidseq
msf> set RHOSTS 10.10.10.42-10.10.10.98
msf> run
```

#### Decoy scan

```bash
nmap -sS 10.10.10.98 -D 10.10.10.2,10.10.10.10,10.10.10.42
```
```bash
# Use 42 random adresses
nmap -sS 10.10.10.98 -D RND:42
```