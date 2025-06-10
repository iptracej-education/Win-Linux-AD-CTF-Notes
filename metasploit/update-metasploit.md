# Update Metasploit

Steps

```bash
# Update metasploit 
msfconsole -v
msfupdate (no longer supported) 
sudo apt update; sudo apt install metasploit-framework
sudo gem install bundler
sudo apt-get upgrade metasploit-framework
sudo msfdb reinit
msfconsole -v

# Start metasploit
sudo msfconsole
sudo systemctl start postgresql && sudo msfconsole  
```

