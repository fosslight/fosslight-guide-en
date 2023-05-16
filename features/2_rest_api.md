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
|/api/v1/export_selfcheck|	File	|Download the result file exported from the Self-Check project.|

6\. Check the value of the code used when using API

| API  | Response format | Description |
| ------------- | ------------- | ------------- |
|/api/v1/code_search|	JSON	|Search the list of values of the parameters to be used when searching for Project, 3rd Party, and creating a project. |


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
