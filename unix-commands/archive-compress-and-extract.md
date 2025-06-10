# archive, compress, and extract

### Tar, gzip, bzip2, 7z, and cpio

{% code overflow="wrap" %}
```bash
# Decompress and Extract gzipped tar file
tar xvfz <file>.tar.gz 
tar xvfz <file.tgz 

# Create archive and compress
tar cvfz <file>.tgz ./*.c

# Decompress and Extract the bzip2ed tar file
tar xvfj <file>.tar.bz2
tar xvfj <file>tbz --non 

# Decompress file to bzip2
bzip2 -d lab-connection.bz2

# Extract 7z file
7z e <file>.7z 
7z x -so oscp.tar.7z | tar xfv - -C ./tmp  

# Create 7z file and archive it
7z a <file>7z ./oscp/* 
tar cf - ./oscp/* | 7z a -si <file>.tar.7z 

# cpio - extract
cpio -idv --no-abolute-filename < back 

```
{% endcode %}

