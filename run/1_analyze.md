---
sort: 1
published: true
---
# Source Install
```note
OSS Review Toolkit Source Code가 [Github Repository](https://github.com/oss-review-toolkit/ort)에서 배포됩니다.
```

## 필요 사항
- Linux / windows / macOS
- OpenJDK 11 or later
- Git
- Gradle

## 설치  방법

### 0. Source Code 다운로드
```
$ git clone https://github.com/oss-review-toolkit/ort.git
```

### 1. Build 

#### Build using Docker
1. 필요 사항
- Docker 18.09 이후 버전
- Enable BuildKit for Docker
2. Docker build 명령으로 Docker 이미지 생성
```
$ cd ort
~/ort$ docker build -t ort .
```
3. 생성된 Docker 이미지 확인
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ort                 latest              b8348af14a4f        3 days ago          4.24GB
```

#### Build natively
```
$ cd ort
~/ort$ ./gradlew installDist
```
