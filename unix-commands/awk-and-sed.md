# awk and sed

Set a delimiter and show the output, and then remove a space in the output.

```
awk -F":" '{print $4}' | sed 's/ //g' 
```
