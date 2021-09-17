---
published: true
title: FOSSLight Reuse
---
# FOSSLight Reuse

<img src="https://img.shields.io/pypi/l/fosslight-reuse" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_reuse" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_reuse" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_reuse)](https://api.reuse.software/info/github.com/fosslight/fosslight_reuse)
    
[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse) is a tool that can be used to comply with the [copyright/license writing rules][rule] in the source code.    
It uses [reuse-tool][ret] to check whether the source code's copyright and license writing rules are complied with.

[ret]: https://github.com/fsfe/reuse-tool
[rule]: https://oss.lge.com/guide/process/osc_process/1-identification/copyright_license_rule.html

##  Functions
1. `lint` --- Check whether the [source code's copyright and license writing rules][rule] are complied with.    
2. `report` --- Convert [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml) to [FOSSLight-Report.xlsx](../learn/2_fosslight_report.md) and vice versa.
     - It converts oss-pkg-info.yaml to SRC Sheet of FOSSLight Report or    
     - BIN (Android) and BOM Sheet of FOSSLight Report to oss-pkg-info.yaml.
3. `add` --- Add copyright and icense to missing file(s)

## 🎉 How to install

It can be installed using pip3.     
It is recommended to install it in the [python 3.6 + virtualenv](https://fosslight.org/fosslight-guide-en/scanner/etc/guide_virtualenv.html) environment.

```
$ pip3 install fosslight_reuse
```

## 🚀 How to run - lint (Check copyright and license writing rules)
``` 
$ fosslight_reuse lint
```
### Parameters      

| Parameter  | Argument | Required  | Description |
| ------------- | ------------- | ------------- |------------- |
| p | root_path_to_check | O | Source path to check. | 
| h | None | X | Print help message. | 
| n | None | X | Add this parameter if you do not want to exclude venv*, node_modules, and .*/ from the analysis.|    
| o | result_file_name | X | xml format result file name. (Default: reuse_checker.xml) |    
| f | file1,file2,... | X | List of files to check copyright and license. |

### Ex 1. Run with minimal parameters 
``` 
$ fosslight_reuse lint -p [root_path_to_check]
```
### Ex 2. Check for specific files
Copyright text and License text are printed for /home/test/notice/sample.py, /home/test/src/init.py.
```
$ fosslight_reuse lint -p /home/test/ -f "notice/sample.py,src/init.py"
```
## How it works
1. Check if it exists in the directory received by parameter -p.
2. Find a OSS Package Information file.
3. Run a Reuse lint.    
    3-1. When running on a project basis. (without -f parameter)
    - If there is no ./reuse/dep5 file in the Root Path, it is created.
    - If it already exists, copy it to bk file and append the default config value to the existing dep file.
    - By creating the dep5 file, exclude binary or .json, venv*/*, node_modules/*,. */* from reuse.
    - Run the reuse lint. 
        ( If the OSS Package Information file exists, the list of missing license files is not printed.)
    - Remove dep5-related file.    
    
    3-2. When executing in file unit (with -f parameter)
    - Print the copyright text and license text extraction by file.
    - However, if the file does not exist or the file is binary or .json, copyright text and license text are not printed.

4. Print the execution result and save it in xml format.

## 📁 Result
### Ex 1. Analyze the files in path.
```bash
(venv)$ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
```
```bash
# SUMMARY
# Open Source Package info: File to which OSS Package information is written.
# Used licenses: License detected in the path.
# Files with copyright information: Number of files with copyright / Total number of files.
# Files with license information: Number of files with license / Total number of files.
 
* Open Source Package info: /home/test/reuse-example/oss-package.info
* Used licenses: CC-BY-4.0, CC0-1.0, GPL-3.0-or-later
* Files with copyright information: 6 / 7
* Files with license information: 6 / 7

```

### Ex 2. Analyze specific files. 
The detected License and Copyright information for each file is output.
```bash
(venv)$ fosslight_reuse lint -p /home/soimkim/test/reuse-example -f "src/load.c,src/dummy.c,src/main.c"
```
```bash
# src/load.c
* License:
* Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>
 
# src/dummy.c
* License:
* Copyright:
 
# src/main.c
* License: GPL-3.0-or-later
* Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>

```

## 🚀 How to run - report (Convert oss-pkg-info.yaml <-> FOSSLight-Report.xlsx)

- File examples : [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml), [FOSSLight-Report.xlsx](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/OSS-Report-Sample_0.xlsx)

``` 
$ fosslight_reuse report
```

### Parameters     

| Parameter  | Argument | Required  | Description |
| ------------- | ------------- | ------------- |------------- |
| p | path_to_check | O | Convert all oss-pkg-info*.yaml or oss-pkg-info*.yml in the path recursively | 
| h | None | X | Print help message. | 
| o | result_file_name | X | Output file name |    
| f | file1,file2,... | X | 1. Yaml files are converted as FOSSLight Report (separated by, if multiple) <br> ex) -f src/oss-pkg-info.yaml,main/setting.yml 2. FOSSLight Report file to be converted to oss-pkg-info.yaml. |

### Ex 1. Convert oss-pkg-info.yaml file to FOSSLight Report.
1-1. Convert all oss-pkg-info*.yaml or oss-pkg-info*.yml in the path recursively.
```bash 
$ fosslight_reuse report -p /home/test/source
```

1-2. Covert the specific oss-pkg-info.yaml files.
```bash 
$ fosslight_reuse report -f src/oss-pkg-info.yaml,main/setting.yml
```

### Ex 2. Convert FOSSLight Report to oss-pkg-info.yaml file.
```bash
$ fosslight_reuse report -f src/FOSSLight-Report.xlsx
```

## 📁 Result
If an output file name is specified with -o, a result file is created with that name.
- FOSSLight-Report_[datetime].xlsx : When the oss-pkg-info.yaml file is converted to FOSS-Report.xlsx
- oss-pkg-info_[datetime].yaml : FOSSLight-Report.xlsx is converted to oss-pkg-info.yaml.


## 🚀 How to run - report (Run as executable. Only for windows.)
1. Download the executable from [fosslight_reuse release][release]
2. Run the executable from the path where FOSSLight-Report*.xlsx or oss-pkg-info.yaml is located.
3. If oss-pkg-info.yaml exists, it will be converted to FOSSLight-Report.xlsx, and if FOSSLight-Report*.xlsx is found, it will be converted to oss-pkg-info.yaml.


## 🚀 How to run - add (Add copyright and license)
```bash 
$ fosslight_reuse add
```

### Parameters      

| Parameter  | Argument | Required  | Description |
| ------------- | ------------- | ------------- |------------- |
| p | path_to_check | O | path to check files | 
| f | file1,file2,... | X | file(s) to add copyright and license |
| c | copyright | O | copyright to add(Copyright <year> <holder name>) | 
| l | license | O | license name to add(recommended to use SPDX format) |

### Ex 1. Add copyright and license to file(s) in input path
```bash
$ fosslight_reuse add -p src/ -c "Copyright 2021 LG Electronics Inc." -l "GPL-3.0"
```
    
### Ex 2. Add copyright and license to input file(s)
```bash 
$ fosslight_reuse add -f "src/load.c,src/dummy.c,src/main.c" -c "Copyright 2021 LG Electronics Inc." -l "GPL-3.0"
```

## How it works
1. Check to present input path using -p option
2. Confirm to add copyright and license
3. Run Reuse Add
    3-1. When running on a project basis. (without -f parameter)
    - Filter available file(s) by file extension in the path
    - Print file list that both has copyright and license(excluded from Adding)
    - Add input copyright and license to missing file(s) using -c and -l option

    3-2. When executing in file unit (with -f parameter)
    - Print the copyright text and license text extraction by file.
    - Add input copyright and license to missing file(s) using -c and -l option

## 📁 Result
### Result print
 * File list that have both license and copyright : the file list which both have copyright and license
 * Missing License File(s) : the file list which has no license
 * Missing Copyright File(s) : the file list which has no copyright

### Changes in the file - Added copyright or license at the top of the file
<table>
<tr>
    <td>Before</td>
    <td>After</td>
</tr>
<tr>
<td>

 <pre lang="python">
  int x = 1
  const string y = "FOSSLight"
  z = sum(x, 1)
  
  </pre>
</td>
<td>
  <pre lang="python">
# SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.
#
# SPDX-License-Identifier: GPL-3.0-only   

 int x = 1
  const string y = "FOSSLight"
  z = sum(x, 1)
</pre>
</td>
</tr>
</table>
    
    
### Ex 1. Add copyright or license in the input path
```
(venv)$ fosslight_reuse add -p tests/add -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```
```bash
# File list that have both license and copyright : 3 / 7
# __init__.py
* License:
* Copyright:

# test_both_have_1.py
* License: GPL-3.0-only
* Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

# test_both_have_2.py
* License: MIT
* Copyright: SPDX-FileCopyrightText: Copyright (c) 2011 LG Electronics Inc.

# Missing license File(s)
  * test_no_license.py
  * Your input license : GPL-3.0-only
Successfully changed header of tests/add_result/test_no_license.py

# Missing Copyright File(s)
  * test_no_copyright.py
  * Your input Copyright : Copyright 2019-2021 LG Electronics Inc.
Successfully changed header of /home/jaekwonbang/commit_0915/tests/add_result/test_no_copyright.py
```
    
### Ex 2. Add copyright or license in the input file(s)
```
(venv)$ fosslight_reuse add -f "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```
```bash
# add/test_both_have_1.py
* License: GPL-3.0-only
* Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

# add/test_both_have_2.py
* License: MIT
* Copyright: SPDX-FileCopyrightText: Copyright (c) 2011 LG Electronics Inc.

# add/test_no_copyright.py
* License: GPL-3.0-only
* Copyright:

# add/test_no_license.py
* License:
* Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

  * Your input license : GPL-3.0-only
Successfully changed header of add/test_no_license.py
  * Your input Copyright : Copyright 2019-2021 LG Electronics Inc.
Successfully changed header of add/test_no_copyright.py
```

[release]: https://github.com/fosslight/fosslight_reuse/releases
