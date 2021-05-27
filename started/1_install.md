# Quick Start
```note
How to quickly and easily install and run FOSSLight.
```

## 🔆 Using the demo site
If you use [https://demo.fosslight.org](https://demo.fosslight.org), you can experience FOSSLight without installation.
- How to register an account: [Sign In/Sign Up](2_try/1_sign.md)
- (Sample) Admin Account: You can experience admin mode through the following admin account.
    - id: admin, pswd: admin

## 🖥️ Install  & Run
### Requirements
- JAVA 1.8 or higher
- MariaDB 10.0 or higher or MySql 5.6 or higher

### How to install
1. Install Java. : [https://openjdk.java.net][java]
2. Install MariaDB or Mysql. : [https://mariadb.org/download][maria]
3. Download the DDL file. : [fosslight_create.sql][sql]
4. Create a database and initialize data.
```
mysql -u root -p < fosslight_create.sql
```
If the database already exists or if you want to change the database name, change 'fosslight' statement at the top.
```
mysql -u root -p <DATABASE_NAME> < fosslight_create.sql
```
Delete (or change) the CREATE USER and GRANT parts if the connection account already exists or if a different account is used.

[java]: https://openjdk.java.net
[sql]: https://github.com/fosslight/fosslight/blob/main/install/db/fosslight_create.sql
[maria]: https://mariadb.org/download

### How to run
1. Download [FOSSLight.war][war]
- How to build Source code : [Development Guide](https://fosslight.org/fosslight-guide-en/learn/1_developer.html)

2. Run the command in the environment where java is installed.
    ```
    java -jar FOSSLight.war --root.dir=/data/fosslight --server.port=8180
    ```

[war]: https://github.com/fosslight/fosslight/releases

#### Running options
- Change web server port.
    ```
    --server.port=<PORT>
    ```
- Setting a work directory. (Default: /usr/share/fosslight)
    ```
    --root.dir=<WORK_DIRECTORY>
    ```
- Change Database information. (Default: 127.0.0.1:3306/fosslight)
    ```
    --spring.datasource.url=<IP>:<Port>/<Database>
    --spring.datasource.username=<USER_NAME>
    --spring.datasource.password=<PASSWORD>
    ```
- Change the log file path.
    ```
    --logging.path=<LOG_PATH>
    ```
