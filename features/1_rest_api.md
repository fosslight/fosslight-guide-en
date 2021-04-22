# REST API
```note
FOSSLight의 기능을 REST API로 호출할 수 있습니다.
```

## 시작하기
### TOKEN 발행
REST API를 호출하기 위해서 TOKEN을 발행해야 합니다.
1. Admin 계정으로 로그인합니다.
2. System > User Management 탭에서 User별로 Token을 발행할 수 있습니다.

## REST API 종류 

1\. OSS & License 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/downloadlocation_search |	JSON|	Download Location으로 OSS 정보를 조회합니다.|
|/api/v1/license_search|	JSON|	License Name으로 License 정보를 조회합니다.|
|/api/v1/oss_search	|JSON|	OSS Name, Version으로 OSS 정보를 조회합니다. |


2\. 3rd Party 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/partner_search|	JSON	|보기 권한이 있는 3rd Party에 대하여 하기 정보를 조회합니다. |

3\. Project 정보 조회, 생성, OSS Report 등록, Packaging 파일 업로드, BOM Export, Project 비교

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/create_project|	JSON|	Project를 생성하고, 생성된 Project ID를 return 받습니다.|
|/api/v1/oss_report_bin	|-	|BIN 탭에 OSS Report를 업로드합니다.이미 OSS Table이 작성된 경우, Reset한 후 업로드하는 OSS Report를 반영합니다.(반영 Sheet Name: "BIN")|
|/api/v1/oss_report_src|	-	|SRC 탭에 OSS Report를 업로드합니다.이미 OSS Table이 작성된 경우, Reset한 후 업로드하는 OSS Report를 반영합니다.(반영 Sheet Name : "SRC")|
|/api/v1/package_upload|-	|Packaging 탭에 Packaging 파일을 업로드합니다. 이미 Packaging 파일이 업로드되어 있는 경우, 추가로 Packaging 파일을 업로드합니다. Packaging 파일 업로드 결과가 Mail로 발송됩니다.|
|/api/v1/prj_bom_compare|	JSON	|두 개의 Project의 BOM의 OSS Name, OSS Version, License를 비교합니다.
|/api/v1/prj_bom_export	|File	|Project의 BOM에서 Export한 결과 파일을 다운로드 받습니다.
|/api/v1/prj_search	|-|보기 권한이 있는 Project에 대하여 하기 정보를 조회합니다. |

4\. Vulnerability 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/vulnerability_data|	JSON|	OSS Name, Version별 CVE ID, CVSS Score, NVD Link를 조회합니다. |
|/api/v1/vulnerability_max_data	|JSON	|OSS Name, Version별 max score와 CVE ID를 확인할 링크를 조회합니다.|

5\. Self-Check 생성, OSS Report 등록

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/create_selfcheck|	JSON	|Self-Check Project를 생성하고, 생성된 Self-Check ID를 return 받습니다.|
|/api/v1/oss_report_selfcheck|	-	|Self-Check에 OSS Report를 업로드합니다. 이미 OSS Table이 작성된 경우, Reset한 후 업로드하는 OSS Report를 반영합니다.(반영 Sheet Name : "Self-Check")|

6\. Binary DB 정보 조회

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/binary_search	|JSON	|Binary DB에서 하기 정보를 조회합니다.|

7\. API 활용시, Code 값 확인

| API  | 응답 형식 | 설명 |
| ------------- | ------------- | ------------- |
|/api/v1/code_search|	JSON	|Project, 3rd Party 조회, Project 생성시 사용할 하기 Parameter의 값 List를 조회합니다. |

## REST API 테스트 도구
```
http://fosslight.org/swagger-ui.html
```
