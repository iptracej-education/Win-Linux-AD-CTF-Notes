# Page



{% code overflow="wrap" %}
```bash
# File Mining to find Creds on windows hosts

Get-ChildItem -Path C:\users -Include *.sam,*.txt,*.ps1,*.log,*.exe,*.kdbx,*.gitconfig,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue

Get-ChildItem -Path / -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue  // this one will look for kdbx files which contain a password manager database

Get-ChildItem -Path C:\ -Include backup.* -File -Recurse -ErrorAction SilentlyContinue

Get-ChildItem -Path C:\ -Include local.txt -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\ -Include proof.txt -File -Recurse -ErrorAction SilentlyContinue
```
{% endcode %}
