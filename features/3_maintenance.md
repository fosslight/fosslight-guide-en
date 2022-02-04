# Maintenance
```note
A useful guide to operating the FOSSLight Hub.
```

## DB backup and recovery
### 1. Backup 
#### Option 1. Full DB backup    
mysqldump -u[id] -p[password] [database_name] > [backup_file_name].sql
```
$ mysqldump -ufosslight -pfosslight fosslight > fosslight_backup.sql
```

#### Option 2. DB backup for updating to the latest version of FOSSLight (data only)
1. Download DBMS. (Recommended DBMS: HeidiSQL https://github.com/HeidiSQL/HeidiSQL)
2. After connecting to the DB, click 'Export database as SQL'.
3. Extract data with DELETE + INSERT.
    ![config](./images/sql_backup.png)

### 2. Recovery    
mysql -u[id] -p[password] [database_name] < [backup_file_name].sql
```
$ mysql -ufosslight -pfosslight fosslight < fosslight_backup.sql
```

