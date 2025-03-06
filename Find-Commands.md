# Linux `find` Command Guide

## 1. Search by Name
```bash
find /etc -name "password"  # Search for a file named 'password' in /etc
find / -name "passwd" -type f  # Search only files named 'passwd'
find / -name "passwd" -type d  # Search only directories named 'passwd'
find / -name "*+*+"  # Search for files with '+*+' in the name
find / -name "*.pdf"  # Search for all PDF files
```

## 2. Search Empty Files/Folders
```bash
find /root -empty  # Find empty files and folders
```

## 3. Search by Permissions
```bash
find / -perm 000  # Find files with no permissions (000)
find / -perm 644  # Find files with permission 644 (rw-r--r--)
find / -perm -u+x  # Find files that are executable by the user
```

## 4. Search by Size
```bash
find / -size 10M  # Find files exactly 10MB in size
find /root -size -10M  # Find files smaller than 10MB
find /root -size +10M -size -20M  # Find files between 10MB and 20MB
```

## 5. Search by User and Group
```bash
find / -user rahul  # Find files owned by user 'rahul'
find / -group wheel  # Find files owned by group 'wheel'
```

## 6. Search by Access Time
```bash
find / -atime +3 -atime -5  # Find files accessed more than 3 days ago but less than 5 days
find / -atime 2  # Find files accessed exactly 2 days ago
find / -atime +2  # Find files accessed more than 2 days ago
find / -amin 10  # Find files accessed exactly 10 minutes ago
find / -amin -10  # Find files accessed in the last 10 minutes
find / -amin +10 -amin -10  # Find files accessed between 10 minutes ago
```

## 7. Search by Modification Time
```bash
find / -mmin -5  # Find files modified in the last 5 minutes
find / -mmin +5  # Find files modified more than 5 minutes ago
find / -mtime 2  # Find files modified exactly 2 days ago
find / -mtime +2  # Find files modified more than 2 days ago
```

## 8. Search and Execute Commands
```bash
find /tmp -empty -exec rm -rf {} \;  # Delete empty files/folders
find / -user rahul -exec cp -iv {} /tmp/ \;  # Copy files owned by rahul to /tmp
find /etc/ -type f -mtime -15 | grep "Aug 25"  # Find files modified in the last 15 days and filter by date
```

## 9. Search Using Multiple Conditions
```bash
find / -user rahul -name "demo" -size 30k -type f -perm 644  # Find a file with multiple conditions
```

## 10. Ignore Case in Search
```bash
find / -iname "passwd"  # Ignore case when searching for 'passwd'
