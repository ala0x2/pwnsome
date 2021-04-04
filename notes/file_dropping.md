# File dropping

## Python
```
python2 -m SimpleHTTPServer <port>
python3 -m http.server <port> -d <dir>
```

## SMB
```bash
mkdir <dir>
chmod 0555 <dir>
chown -R nobody:nogroup <dir>
```
```conf
#/etc/samba/smb.conf
[hermes]
path = <dir>
writable = no
guest ok = yes
guest only = yes
read only = yes
directory mode = 0555
force user = nobody
```

## ngrok
```bash
sudo php -S localhost:<port>
./ngrok http https://localhost:<port>
```

## NFS (Network File System)
On arch `nfs-server.service`
```bash
# /etc/exports
/srv/nfs/music  192.168.1.0/24(rw,sync)
```