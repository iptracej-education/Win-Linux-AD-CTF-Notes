# Compiling

### [Linux](https://github.com/iptracej/MyPentestCheatsheet/blob/main/3.Exploitation.md#linux) <a href="#user-content-linux" id="user-content-linux"></a>

```
# search gcc

which gcc
find / -name gcc -type f 2>/dev/null

# 64 bits
gcc -o foo foo.c

# 32 bits
gcc -m32 -o foo foo.c

# Python to Exe
To compile python scripts, pyinstaller --onefile <SCRIPT.py>
```

### [Compile windows .exe on Linux (Kali)](https://github.com/iptracej/MyPentestCheatsheet/blob/main/3.Exploitation.md#compile-windows-exe-on-linux-kali) <a href="#user-content-compile-windows-exe-on-linux-kali" id="user-content-compile-windows-exe-on-linux-kali"></a>

```bash
#Installing cross complier on Kali 64 bit machine
apt-get install mingw-w64 

#For compiling Windows 64 bit code on 64 bit Linux
i686-w64-mingw32-gcc foo.c -o foo.exe
x86_64-w64-mingw32-gcc foo.c -o foo.exe 

#For compiling 32 bit code on 64 bit Linux
i686-w64-mingw32-gcc foo.c -o foo.exe -lws2_32 
x86_64-w64-mingw32-gcc foo.c -o foo.exe -lws2_32

# C# 
# https://github.com/mono/mono 
mcs ReverseShell.cs
```
