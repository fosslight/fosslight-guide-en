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

## Download NVD Data from 2002
FOSSLight Hub downloads [NVD Data Feeds](https://nvd.nist.gov/vuln/data-feeds) provided from NVD(NATIONAL VULNERABILITY DATABASE) once a day and stores them in the database, and the stored NVD data is viewed in the [Vulnerability List](../started/2_try/7_vulnerability.md).    
At this time, when downloading NVD data from 2002 data, set as follows.      
(If you set it only once for the first time, there is no need to set it additionally because the data will be accumulated afterwards.)           
        
**Change the setting value in DB**    
```
UPDATE T2_CODE_DTL SET CD_DTL_NM = 'Y' WHERE CD_NO = '990' AND CD_DTL_NO = '100';
```
The default value of NVD Data Feed initialize flag Code is set to “N”, and if you change it to “Y” directly as above, all NVD data is cleaned during the next NVD schedule operation, and registration is processed sequentially from the 2002 data file.     
The value is changed to the default value ("N") regardless of whether there is an error when performing NVD Data initialization.     
