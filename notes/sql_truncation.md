# SQL Truncation

## Vulnerability

- MySQL truncates strings with size bigger than the defined maximum column width
- MySQL by default uses relaxed comaprisons

```sql
'admin' == 'admin ' -- true
```

- Giving the string below for example as input could lead to the creation of an account with `admin ` (**admin + whitespace**) as username.

```powershell
admin           hermes
```