# Developer Documentation
```note
How to install and run FOSSLight.
```

## How to install - 1
Build and run using Docker.

### Requirements
1. [Docker][docker]
2. [Docker Compose][doccompose]

[docker]: https://docs.docker.com/engine/install/
[doccompose]: https://docs.docker.com/compose/install/

### Build and run
```
docker-compose up --build
```

## How to install - 2
### Requirements
- Java 1.8 or higher
- MariaDB (10.0 or higher) or MySql (5.6 or higher)
- Memory : 8GB+

### Development environment
- Framework : Spring Boot 2.1.x
- Build Tool : Gradle 6.x
- Git : [https://github.com/fosslight/fosslight][src]
- IDE : [Spring Tool Suite][spring]
    - [lombock][lb] installation required.
- Project Character Set: UTF-8

### Download & Installation
1. Install Java.: [https://openjdk.java.net][java]
2. Download a DDL file. : [fosslight_create.sql][sql]
3. Install MariaDB or Mysql. : [https://mariadb.org/download][maria]
4. Create a database and initialize data.
```
mysql -u root -p < fosslight_create.sql
```
If the database already exists or if you want to change the database name, change 'fosslight' statement at the top.
```
mysql -u root -p <DATABASE_NAME> < fosslight_create.sql
```
Delete (or change) the CREATE USER and GRANT parts if the connection account already exists or if a different account is used.

### IDE Configuration
Download [Spring Tool Suite][spring].  

#### Project Import
※ Based on STS (Spring Tool suite) 4.x
1. Install lombok.: [https://projectlombok.org/setup/eclipse][lb]
2. File > Import > Gradle > Existing Gradle Project
3. Set up and import the [Git Source Directory][git_repo].
4. Set to UTF-8 in Project> Properties> Resource> Text file encoding.

[spring]: https://spring.io/tools
[lb]: https://projectlombok.org/setup/eclipse
[src]: https://github.com/fosslight/fosslight
[sql]: https://github.com/fosslight/fosslight/blob/main/db/initdb.d/fosslight_create.sql
[maria]: https://mariadb.org/download/
[java]: https://openjdk.java.net
[git_repo]: https://github.com/fosslight/fosslight

### How to run
#### Change the running options.
Change running options in [application.properties][props] file.
 - server.port=8180: Web server port (In case of 8180, [http://localhost:8180][local])
 - spring.datasource.url=127.0.0.1:3306/fosslight: Set the IP, Port, and Database Name of the DB server where FOSSLight Database is installed.
 - spring.datasource.username=fosslight: Database username.
 - spring.datasource.password=fosslight: Database password.
 - logging.path=./logs : Set log file path. ( Default "./logs" means an application execution path.)
 - logging.file=fosslight : The name of the log file. (Ref. logback-spring.xml)
 - root.dir=./data: Top path of file upload/download.

[props]: https://github.com/fosslight/fosslight/blob/main/src/main/resources/application.properties

#### Build & Run
You can build and run it in the following way.      
You can also download the official release version of the built [war file][war].

[war]: https://github.com/fosslight/fosslight/releases
  
- build (Create a war file.)
```
$ ./gradlew build
```
- run
```
$ ./gradlew bootRun
```
- build & run 2 (Run the application after building)
```
$ ./gradlew clean build && java -jar build/libs/FOSSLight-1.0.0.war
```

   -   Running options
        - Web server port
        ```
        --server.port=<PORT>
        ```
        - Work Directory (Default: /usr/share/fosslight)
        ```
        --root.dir=<WORK_DIRECTORY>
        ```
        - Database Connections (Default: 127.0.0.1:3306/fosslight)
        ```
        --spring.datasource.url=<IP>:<Port>/<Database>
        --spring.datasource.username=<USER_NAME>
        --spring.datasource.password=<PASSWORD>
        ```
        - log file path
        ```
        --logging.path=<LOG_PATH>
        ```

#### Run from IDE    
    - Boot Dashboard > local > FOSSLight, (right click) start (Crtl + Alt + Shift + B, R)


## Operation check
- If you connect to [http://localhost:8180][local] from a web browser, the sign in page is displayed.
- Default account :
    -  id: admin, pswd :admin

[local]: http://localhost:8180
