# Binary/Image Analysis

### Common

{% code overflow="wrap" %}
```bash
# Check real file type
file file.xxx

# Analyze strings
strings file.xxx
strings -a -n 15 file.xxx # Check the entire file and outputs strings longer than 15 chars

# Check embedded files
binwalk file.xxx # Check
binwalk -e file.xxx # Extract

# Check as binary file in hex
ghex file.xxx

# Check metadata
exiftool file.xxx

# Stego tool for multiple formats
wget https://embeddedsw.net/zip/OpenPuff_release.zip
unzip OpenPuff_release.zip -d ./OpenPuff
wine OpenPuff/OpenPuff_release/OpenPuff.exe
```
{% endcode %}

### Disk Files

{% code overflow="wrap" %}
```bash
# guestmount can mount any kind of disk file
sudo apt-get install libguestfs-tools
guestmount --add yourVirtualDisk.vhdx --inspector --ro /mnt/anydirectory
```
{% endcode %}





