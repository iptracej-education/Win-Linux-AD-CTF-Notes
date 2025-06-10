---
description: >-
  Let's take a look at the ldap information including the user description
  object.
---

# Port 389/636 - LDAP

### Enumeration

```bash
ldapsearch -H ldap://172.16.1.20 -x -s base namingcontexts
ldapsearch -H ldap://172.16.1.20 -x -b "DC=DANTE,DC=local"
ldapsearch -H ldap://172.16.1.20 -D '' -w '' -b "dc=dante,dc=local"
# -H: Host
# -x: simple authentication
# -s base: Scope of the search
# -b: base DN (distinguished name)
# -w: empty password
# -D: bind DN

# With User name and password
ldapsearch -H ldap://dc.absolute.htb -x -D d.klay@absolute.htb -w Darkmoonsky248girl -s base

# With Kerberos authentication
impacket-getTGT 'absolute.htb/d.klay:Darkmoonsky248girl' -dc-ip $RHOST
export KRB5CCNAME=d.klay.ccache 
crackmapexec ldap dc.absolute.htb --use-kcache --users 

# With GSSAPI 
sudo apt install libsasl2-modules-gssapi-mit 
ldapsearch -H ldap://dc.absolute.htb -Y GSSAPI -b "cn=users,dc=absolute,dc=htb" users description
```

Also, take a look at [AD-User Recon](https://app.gitbook.com/o/sbYQpaBkV0ueQjvLIdTt/s/nA4bAkddGXesk1QCLYAY/active-directory/user-recon) section.&#x20;
