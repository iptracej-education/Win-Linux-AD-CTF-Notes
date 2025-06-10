# SYSVOL and NETLOGON

SYSVOL is the domain-wide share in Active Directory to which all authenticated users have read access. SYSVOL contains logon scripts, group policy data, and other domain-wide data which needs to be available anywhere there is a Domain Controller (since SYSVOL is automatically synchronized and shared among all Domain Controllers).

{% hint style="info" %}
All domain Group Policies are stored here: \\\\\<DOMAIN>\SYSVOL\\\<DOMAIN>\Policies\\
{% endhint %}



**Credentials in SYSVOL/NETLOGON**

The issue is that frequently the password is stored in clear-text within the script (such as a vbs file) which is often in SYSVOL. and NETLOGON&#x20;

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
Login script can be abused to embed a reverse shell.  Check logon Script abuse section.&#x20;
{% endhint %}

**Group Policy Preferences**

Check out [GPP](../group-policy-abuse/gpp-group-policy-preference-credential-discovery.md) section.  Often you can check mannualy under SYSVO directory like active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml







