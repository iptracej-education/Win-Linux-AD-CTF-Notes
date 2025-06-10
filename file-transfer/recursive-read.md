# Recursive Read



```bash
# Read all files
for i in *; do echo ">> $i <<" && cat $i; done

# Read all files including hidden files
for file in $(find . -type f); do echo ">> $file <<"; cat $file; done
```
