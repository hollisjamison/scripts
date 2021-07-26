### Stop Qlik from Powershell
```
Get-Service "Qlik*" | Where-Object {($_.Name -like "QlikSense*" -and $_.Name -notlike "QlikSenseRepositoryDatabase") -or ($_.Name -eq "QlikLoggingService")} | Stop-Service -Force
```

### Start Qlik from Powershell
```
Start-Service -Name "Qlik Sense Service Dispatcher"
start-sleep -Seconds 5
Start-Service -Name "QlikSenseServiceDispatcher"
start-sleep -Seconds 5
Start-Service "Qlik*"
```
### Backup Qlik (run this from C:\program files\Qlik\Sense\Repository\PostgreSQL\<database version>\bin)
```
pg_dump.exe -h localhost -p 4432 -U postgres -b -F t -f "c:\QSR_backup.tar" QSR
```

### Restore Qlik DB If disaster strikes.. run from same location as above
1. Drop the database in PGAdmin
2. Create the database
```
createdb -h localhost -p 4432 -U postgres -T template0 QSR
```
3. Restore the database
```
pg_restore.exe -h localhost -p 4432 -U postgres -d QSR "c:\QSR_backup.tar"
```

### SQL to list all users with the issue
```
SELECT * from public."Users"
WHERE "UserDirectory" = 'STAFF'
AND "UserDirectoryConnectorName" <> 'STAFF';
```

### SQL to run the change
```
UPDATE public."Users"
SET "UserDirectoryConnectorName" = 'STAFF'
WHERE "UserDirectory" = 'STAFF'
AND "UserDirectoryConnectorName" <> 'STAFF';
```
