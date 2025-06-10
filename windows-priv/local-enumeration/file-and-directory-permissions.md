# File and Directory permissions

#### [icacls](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#icacls) <a href="#user-content-icacls" id="user-content-icacls"></a>

```bash
# Check permissions

icacls Desktop # Directory

• (OI)  —  object inherit;
• (CI)  —  container inherit;
• (I)  — permission inherited from parent container.
• (F)  —  full access;

icacls Root.txt # Files

#Grant permissions

icacls root.txt /grant CHATTERBOX\Alfred:R
```

#### [Command to hide or unhide the files](https://github.com/iptracej/MyPentestCheatsheet/blob/main/PrivEsc/Windows.md#command-to-hide-or-unhide-the-files) <a href="#user-content-command-to-hide-or-unhide-the-files" id="user-content-command-to-hide-or-unhide-the-files"></a>

```bash
attrib +h c:\autoexec.bat (Hide)
attrib -h c:\autoexec.bat (Unhide)

attrib -h C:\Users\Administrator\Desktop\proof.txt (Unhide proof
```
