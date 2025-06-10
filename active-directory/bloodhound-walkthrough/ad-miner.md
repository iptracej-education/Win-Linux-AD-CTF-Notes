# AD-Miner

Sometimes you might have more complex with a large volume of the AD objects to analyze and understand attack vectors with high-risk targets. AD-Miner might help you better and useful.&#x20;

### Installation

{% code overflow="wrap" %}
```bash
# https://github.com/Mazars-Tech/AD_Miner

# @Kali
pipx install 'git+https://github.com/Mazars-Tech/AD_Miner.git'
```
{% endcode %}

### Enumeration&#x20;

```bash
# @Kali
sudo neo4j console
bloodhound & 

# Ensure that you have imported the zip file to the bloodhound. 

mkdir ad-miner
cd ad-miner
AD-miner -c -cf My_Report -u neo4j -p <your neo4j password> 

cd render_My_report 
firefox index.html 
```

### AD Miner UI&#x20;

You can check immediate risks, which are typically privilege-escalated to Domain Admin, etc. You can also check users and groups easily.&#x20;

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>
