# AD Interesting file location

## **AD Recycle Bin**

{% code overflow="wrap" %}
```bash
#Start with checking group for a user logged-in

PS>net user arksvc

#This isn't a powerview command, it's a feature from the AD management powershell module of Microsoft
#You need to be in the "AD Recycle Bin" group of the AD to list the deleted AD objects.
# You may obtain jucy information.

PS>Get-ADObject -filter 'isDeleted -eq $true' -includeDeletedObjects -Properties *
```
{% endcode %}
