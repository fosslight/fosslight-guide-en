---
sort: 1
published: true
---
# 세부 실행 방법
```note
분석을 위한 Tool 기반으로 세부 실행 방법을 설명합니다.
1. Analyzer : 프로젝트의 dependency 정보와 metadata를 추출합니다.
2. Scanner : Source Code를 다운로드 받은 후 Source Code에서 License text를 분석하는 Tool로 분석합니다.
3. Evaluator : Scanner 결과에 대한 사용자 지정 License 정책 검사를 수행합니다.
4. Advisor : 보안 취약점을 조회합니다. (단, 사용을 위해서 [Nexus IQ Server](https://help.sonatype.com/iqserver)의 License가 필요합니다.)
5. Reporter : 분석 결과를 여러가지 형태로 출력합니다.
```
## 1. Analyzer 실행하기
지정된 입력 디렉토리 (-i) 내에서 프로젝트의 dependency을 결정하는 Software Composition Analysis (SCA) 도구입니다.  
감지 된 package manager를 이용하여 수행됩니다. 프로젝트의 transitive dependency의 tree와 package의 meta data를 지정된 출력 디렉토리(-o)에 analyzer-result.yml (또는 JSON, -f 참조) 파일로 작성합니다.   
- 지원하는 Package Manager: Bower, Bundler, Cargo, Carthage, Conan, DotNet, GoDep, GoMod, Gradle, Maven, NPM, NuGet, PhpComposer, PIP, Pipenv, Pub, SBT, SpdxDocumentFile, Stack, Yarn

### 실행 방법
```
$./cli/build/install/ort/bin/ort analyze -i [working-directory] -o [analyzer-output-dir]
```

|Parameters|Description |
|--|--|
| i |분석할 Working directory|
| o |Analyzer 실행 결과가 생성될 path |
| f |실행 결과 파일 형식 (JSON,XML,YAML 중 선택.) Default : YAML|

## 2. Scanner 실행하기
Analyzer 결과 (-i)가 전달되면 Scanner는 Downloader를 통해 소스를 다운로드한 후 스캔합니다.

### 실행 방법
```
$./cli/build/install/ort/bin/ort scan -i [analyzer-output-file] -o [scanner-output-dir]
```

|Parameters|Description |
|--|--|
| i |Analyzer 결과 파일|
| o |Scanner 실행 결과가 생성될 path |
| s |Scanner Tool을 [ScanCode](https://github.com/nexB/scancode-toolkit), [Askalono](https://github.com/amzn/askalono), [lc](https://github.com/boyter/lc), [Licensee](https://github.com/benbalter/licensee) 중에서 선택. (Default : [ScanCode](https://github.com/nexB/scancode-toolkit))|
| f |실행 결과 파일 형식 (JSON,XML,YAML 중 선택.) Default : YAML|


## 3. Evaluator 실행하기
Scanner 결과에 대한 사용자 지정 License 정책 검사를 수행하는 데 사용합니다.  
확인할 규칙은 스크립트로 구현됩니다.    

### 예제 파일
- [rules.kts](https://github.com/oss-review-toolkit/ort/blob/master/examples/rules.kts) : 사용자가 정의한 Policy Rule
- [license-classifications.yml](https://github.com/oss-review-toolkit/ort/blob/master/examples/license-classifications.yml) : 사용자가 정의한 License 카테고리
- [curations.yml](https://github.com/oss-review-toolkit/ort/blob/master/examples/curations.yml) : 사용자가 정의한 Package Curation.

```
$./cli/build/install/ort/bin/ort evaluate --package-curations-file [examples/curations.yml] --rules-file [examples/rules.kts] --license-configuration-file  [examples/license-classifications.yml] -i [analyzer-output-file] -o [evaluator-output-dir]
```

|Parameters|Description |
|--|--|
| i |Analyzer 결과 파일|
| o |Evaluator 실행 결과가 생성될 path |
| -rules-file |Policy Rule 파일|
| -package-curations-file |Package Curation 파일|
| -license-configuration-file |License 카테고리 파일|


## 4. Advisor 실행하기
보안 취약점을 조회합니다.  
실행하기 위해서 Analyzer 결과와 [Nexus IQ Server](https://help.sonatype.com/iqserver)의 계정, License가 필요합니다.

### 준비 사항
ORT Configuration file 생성
ort.conf 파일을 생성하고 하기와 같이 작성합니다.
```
ort {
  advisor {
    nexusiq {
      serverUrl = "https://nexusiq.ossreviewtoolkit.org"
      username = myUser
      password = myPassword
    }
  }
}
```

### 실행 방법
```
$./cli/build/install/ort/bin/ort -c [ort.conf] advise -o [advisor-output-dir] -i [analyzer-output-file] 
```

|Parameters|Description |
|--|--|
| i |Analyzer 결과 파일|
| o |Advisor 실행 결과가 생성될 path |
| c |ORT Configuration file|


## 5. Reporter 생성하기
분석 결과를 여러가지 형태로 출력합니다.

### 실행 방법
```
$./cli/build/install/ort/bin/ort report
  -f NoticeTemplate,StaticHtml,WebApp
  -i [evaluator-output-file]
  -o [reporter-output-dir]
```

|Parameters|Description |
|--|--|
| i |Evaluator 결과 파일 (Evaluator를 사용하지 않는 경우, Scanner 결과 파일)|
| o |Reporter 실행 결과가 생성될 path |
| f |출력하는 Report 형태 (AmazonOssAttributionBuilder,AntennaAttributionDocument, CycloneDx,EvaluatedModelJson, EvaluatedModelYaml, Excel,GitLabLicenseModel, NoticeTemplate,SpdxDocument, StaticHtml, WebApp)|
