---
sort: 2
published: true
---
# 설치 확인 방법

## Docker를 이용한 실행 방법
```
$docker run -v /workspace:/project ort --info analyze -f JSON -i /project -o /project/ort/analyzer
```
### Parameter
```
$docker run \
  -v $PWD/:/project  \ # Mount current working directory into /project to use as input.
  ort --info analyze \
  -c /project/ort/config.hocon \ # Use file from "<workingdirectory>/ort" as config.
  analyze (...) # Insert further arguments for the command.
```
- v 옵션 : 공유할 호스트 디렉토리와 컨테이너 디렉토리를 설정
- c 옵션 : Config 파일 설정
- analyze (...) : [ort의 실행 arguments](https://lge-oss.github.io/oss-review-toolkit-guide/use/1_analyze.html)

## Build한 소스 코드 기반 실행 방법
### 추가적인 Tool이 미설치된 것인지 확인
```
~/ort$./cli/build/install/ort/bin/ort requirements
```
### 실행 Command 확인 방법
```
~/ort$./cli/build/install/ort/bin/ort --help
```
