# REST API
```note
Functions of FOSSLight can be called with REST API.
```

## How to start
### Create a TOKEN
TOKEN must be issued to call REST API.
1. Sign in as Admin account.
2. You can issue tokens for each user in the System > User Management tab.

## Test REST API

- [http://demo.fosslight.org/swagger-ui.html][swagger] 

[swagger]: http://demo.fosslight.org/swagger-ui.html

## REST API List

1\. Check OSS & License information

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/downloadlocation_search |	JSON|	Search OSS information by download location.|
|/api/v1/license_search|	JSON|	Search license information by license name.|
|/api/v1/oss_search	|JSON|	Search OSS information by OSS Name and Version.|

2\. Check 3rd Party information

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/partner_search|	JSON	|Get 3rd party information. |

3\. Check project information, upload FOSSLight Report/Packaging, export/comparison of BOM.

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/create_project|	JSON|Create a Project and the generated project's ID is returned.|
|/api/v1/oss_report_bin	|-	|Upload FOSSLight Report to BIN Tab. If data already exists in the OSS Table, the FOSSLight Report uploaded after reset. (Loaded Sheet Name : "BIN")|
|/api/v1/oss_report_src|	-	|Upload FOSSLight Report to SRC Tab. If data already exists in the OSS Table, the FOSSLight Report uploaded after reset. (Loaded Sheet Name : "SRC")|
|/api/v1/package_upload|-	|Upload the Packaging file to the Packaging tab. If packaging files have already been uploaded, an additional packaging file is uploaded. Packaging file upload result will be sent by mail.|
|/api/v1/prj_bom_compare|	JSON	|Compare the OSS Name, OSS Version, and License of the two projects' BOM.
|/api/v1/prj_bom_export	|File	|Download the result file exported from the BOM of the project.
|/api/v1/prj_search	| JSON |Get project information.|

4\. Check Vulnerability information

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/vulnerability_data|	JSON|	Search OSS Name, CVE-ID for each version, CVSS Score, and NVD Link. |
|/api/v1/vulnerability_max_data	|JSON	|Search max score and the NVD link by OSS Name and Version.|

5\. Create Self-Check and register FOSSLight Report

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/create_selfcheck|	JSON	|Create a Self-Check Project and the generated Self-Check's ID is returned.|
|/api/v1/oss_report_selfcheck|	-	|Upload FOSSLight Report to Self-Check. If data already exists in the OSS Table, the FOSSLight Report uploaded after reset. (Loaded Sheet Name : "Self-Check")|

6\. Check the value of the code used when using API

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/code_search|	JSON	|Search the list of values of the parameters to be used when searching for Project, 3rd Party, and creating a project. |

