# Installation (Quick Start)
```note
FOSSLight를 빠르고 쉽게 설치 및 실행하는 방법을 설명합니다.
```
## 설치
### 요구사항
- JAVA 1.8 이상
- MariaDB 10.0 이상 또는 MySql 5.6 이상

### 설치
1. JDK를 설치합니다.: https://openjdk.java.net/install/
2. DDL : http://nas.think-tree.com/sharing/aInqZiu0Z 
3. MariaDB 또는 Mysql 설치합니다. 
4. Database 생성 및 초기 Data 등록
```
mysql -u root -p < fosslight_data.sql
```
만약 Database가 이미 존재하거나 Database 이름을 변경하려면 상단의 create database 문과 USE 'fosslight' 문을 변경합니다.
단, 사용자 생성과 권한 쿼리는 포함되어 있지 않습니다. 

## 실행
java가 설치되어 있는 환경에서 명령어를 실행합니다. 
```
java -jar fosslight.war
```
### 실행 옵션
- 포트 변경
```
java --server.port=8180 -jar fosslight.war
```
- database 접속정보 변경 (Default: 127.0.0.1:3306/fosslight)
```
spring.datasource.url=<IP>:<Port>/<Database>
spring.datasource.username=<userName>
spring.datasource.password=<password>
```
- log 파일 경로 지정
```
logging.path=<path>
```
- ⚠️Database 생성 없이 Demo Site의 Database 사용
```
java --spring.datasource.url=35.73.164.212:3306/fosslight -jar fosslight.war
```
