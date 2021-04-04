# Dictionary attacks

- **Attacking SSH with 4 tasks or more may lead to account lockout when SSH configurations limit the number of parallel tasks.**

## John the Ripper

### Conversion to compatible format
```bash
unshadow /etc/passwd /etc/shadow > users.john
```
```bash
ssh2john id_rsa > id_rsa.john
```
```bash
# Find other conversion scripts
locate *2john
```

## Hydra
```bash
hydra -L user.txt -P pass.txt <target> <service>
```

## ncrack
```bash
ncrack -U user.txt -P pass.txt <target> -p <port>
```

# Generate dictionaries

## CUPP
```bash
cupp -i
```

## pydictor
```bash
# Decimal lowercase capital
python pydictor.py --len 2 5 -base dLc
# All possible combinations of 'f' 'o' 'o'
python pydictor.py -char foo --len 3 3
# All possible combinations of chunks in specified file
python pydictor.py -chunk foo oof */.:@ -o foo.txt
```

## dymerge
`-f` simply ignores `sleep()` used for verbosity