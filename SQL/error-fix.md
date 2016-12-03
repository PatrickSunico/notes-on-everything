# Fixing the "ERROR! The server quit without updating PID file" 

From my understanding, whenever we are updating mysql or the server is running while updating mysql. I think for best practice is to stop the server first before doing any major update changes on mysql.

Credits to the people at stack overflow.

```
sudo chown -R `whoami` /usr/local/var/mysql
cd /usr/local/var/mysql
mv ib_logfile0 ib_logfile0.bak
mv ib_logfile1 ib_logfile1.bak
[move the .err logfile to a new location]
```

