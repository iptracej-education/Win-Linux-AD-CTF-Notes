---
description: >-
  Establishing a standard for your project structures and files can enhance your
  workflow with targets and provide the necessary information for executing a
  series of commands.
---

# Working Directory and Files

#### Organize your findings and make it available for your attacks.

```bash
# Set some ENV variables for potential navigations
export $PROJECT=$HOME/YourProject
export $LHOST=<your attacker PC> 

# Store all Domain information into one directory
~/YourProject
|- Commands        # This is pentesting command library 
|- Findings |- Domains        
            |- ip.txt
            |- hashes.txt
            |- hashes-sevenkingdoms.local
            |- hashes-essos.local
            |- users.txt
            |- users-sevenkingdoms.local
            |- users-essos.local
            |- passwords.txt
            |- passwords-sevenkingdoms.local
            |- passwords-essos.local
|- nmap            # nmap scan output  
|- autorecon       # autorecon scan output 
|- exploit         # Initial foot-hold exploit tools and payloads
|- prviesc         # Privilege Escalation tools and payloads
|- post            # Post-Exploitation tool and payloads 
|- logs            # tmux logs goes here 
|- screenshots     # screenshot pictures 
|- notes           # Random notes here
|- tmp             # scrap files that eventually will be removed 
```
