# Extract credentials from images



{% code overflow="wrap" %}
```bash
# Download files
for i in {1..10}; do curl "http://absolute.htb/images/hero_$i.jpg" > hero_$i.jpg; done

# Check if you can get usernames
exiftool * | grep Author

#Extract usernames from images 
exiftool * | grep Author | awk '{print$3"."$4}'
```
{% endcode %}
