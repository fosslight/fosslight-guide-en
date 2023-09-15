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
|/api/v1/partner_watcher_add|-|Add watcher of 3rd Party project.|

3\. Check project information, upload FOSSLight Report/Packaging, export/comparison of BOM.

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/create_project|	JSON|Create a Project and the generated project's ID is returned.|
|/api/v1/model_search| JOSN| Get the Model List for the Project. (Maximum number of Return Items: 1000) <br>  - Project ID, Category, Model, Name, Release Date|
|/api/v1/model_update|  JSON| Update model information in Project's Basic Information and Distribution tab with model information string list. <br>  - Model information String list (format . MODEL_NAME\|Category\|Release Date) <br>  - ex) MODEL_NAME\|ETC > Etc\|20220428|
|/api/v1/model_update_upload_file	|JSON| Update model information in Project's Basic Information and Distribution tab with excel file of model list. <br>  - Excel file of model list : Project > Basic Information tab > Click Download button.|
|/api/v1/oss_report_bin	|-	|Upload FOSSLight Report to BIN Tab. If data already exists in the OSS Table, the FOSSLight Report uploaded after reset. (Loaded Sheet Name : "BIN")|
|/api/v1/oss_report_src|	-	|Upload FOSSLight Report to SRC Tab. If data already exists in the OSS Table, the FOSSLight Report uploaded after reset. (Loaded Sheet Name : "SRC")|
|/api/v1/package_upload|-	|Upload the Packaging file to the Packaging tab. If packaging files have already been uploaded, an additional packaging file is uploaded. Packaging file upload result will be sent by mail.|
|/api/v1/prj_bom_compare|	JSON	|Compare the OSS Name, OSS Version, and License of the two projects' BOM.|
|/api/v1/prj_bom_export	|File	|Download the result file exported from the BOM of the project.|
|/api/v1/prj_bom_export_json|JSON |Returns the result exported from the project's BOM in json format.|
|/api/v1/prj_search	| JSON |Get project information.|
|/api/v1/prj_watcher_add|-|Add watcher of project.|

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
|/api/v1/export_selfcheck|	File	|Download the result file exported from the Self-Check project.|
|/api/v1/selfcheck_watcher_add|-|Add watcher of Self-Check project.|

6\. Check the value of the code used when using API

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/code_search|	JSON	|Search the list of values of the parameters to be used when searching for Project, 3rd Party, and creating a project. |

7\. Error Code

| Return Code  | Description | Error Message |
| ------------- | ------------- | ------------- |
|200|User does not exist.|User does not exist.|
|210|There is an error in the TOKEN value.|There is an error in the TOKEN value.|
|310|The parameter is invalid.|The parameter is invalid.|
|320|The number of project and self-check creations has been exceeded. (Up to 3 creations per day using API)|The number of projects and self-checks that can be created has been exceeded. (Up to 3 per day)|
|330|When registering files such as OSS Report, an error occurred during validation check for the created data.|There is an error in the data written in the file.|
|400|Files to upload (ex-OSS Report, NOTICE, result.txt, Packaging files) are missing.|The file to upload is missing.|
|410|It is the case that the size of OSS Report and Packaging file is exceeded. (Maximum Size: OSS Report -5MB, Packaging file- 4GB)|File size exceeded. (Max size: 5MB for oss report, 4GB for packaging file)|
|420|The registered files are extensions that are not supported.|The registered files are extensions that are not supported.
|430|The tab you are trying to upload is not active. Please check the Distribution type and the tab to upload. |The tab you are trying to upload is not active.|
|440|When there is no Sheet name to load|The [Sheet_name] sheet name cannot be found|
|440|When there are no rows to load in the uploaded file|There is no data to load.|
|500|You do not have permission. (ex: When a non-public project is viewed by a user other than a Watcher or Creator) |You do not have permission.|
|999|Unknown Error.  |Unknown error.|

## REST API Sample
Example of searching project information of user and admin accounts using prj_search
```python
# SPDX-FileCopyrightText: Copyright 2023 LG Electronics Inc.
# SPDX-License-Identifier: Apache-2.0

import csv
import requests
from datetime import datetime
from collections import OrderedDict

header_list = ['prjId', 'prjName', 'prjVersion', 'createDate', 'updateDate', 'identificationStatus', 'verificationStatus', 'distributionStatus', 'status', 'vulnerabilityScore', 'distributionType', 'notice', 'networkService', 'priority', 'noticePlatform']

def get_data(period, user):
    url = "https://demo.fosslight.org/api/v1/prj_search"

    querystring = {"createDate":period,"creator":user}

    payload = ""
    headers = {"_token": "abCDe...."}

    response = requests.request("GET", url, data=payload, headers=headers, params=querystring, verify=False)

    # print(response.text)
    data = response.json()['data']

    content_list = data['content']

    data_list = []
    for content in content_list:
        data = OrderedDict() 
        for header in header_list:
            if header in content.keys():
                data[header] = content[header]
            else:
                data[header] = ''
        data_list.append(data)

    return data_list

if __name__ == "__main__":
    current_year = datetime.now().year
    
    content_list = []
    content_list.extend(get_data('20220101-{0}1231'.format(current_year), "user"))
    content_list.extend(get_data('20220101-{0}1231'.format(current_year), "admin"))

    content_list.sort(key=lambda x:x['prjId'],reverse=True)
    with open('api_data.csv','w',encoding='utf-8',newline='') as f:
        wr = csv.writer(f)
        wr.writerow(header_list)
        for content in content_list:
            wr.writerow(list(content.values()))

```
