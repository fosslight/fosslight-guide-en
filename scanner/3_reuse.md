---
published: true
title: FOSSLight Reuse
---
# FOSSLight Reuse

<img src="https://img.shields.io/pypi/l/fosslight-reuse" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_reuse" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_reuse" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_reuse)](https://api.reuse.software/info/github.com/fosslight/fosslight_reuse)
    

[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse) is a tool that can be used to comply with the [copyright/license writing rules][rule] in the source code using [reuse-tool][ret].

[ret]: https://github.com/fsfe/reuse-tool
[rule]: https://oss.lge.com/guide/process/osc_process/1-identification/copyright_license_rule.html

##  Functions
1. `lint` --- Check whether the [source code's copyright and license writing rules][rule] are complied with.    
2. `report` --- Convert [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml) to [FOSSLight-Report.xlsx](../learn/2_fosslight_report.md) and vice versa.
     - It converts oss-pkg-info.yaml to SRC Sheet of FOSSLight Report or    
     - BIN (Android) and BOM Sheet of FOSSLight Report to oss-pkg-info.yaml.
3. `add` --- Add copyright and license to source code which is missing copyright and license

## 🎉 How to install

It can be installed using pip3.     
It is recommended to install it in the [python 3.6 + virtualenv](https://fosslight.org/fosslight-guide-en/scanner/etc/guide_virtualenv.html) environment.

```
$ pip3 install fosslight_reuse
```

## 🚀 How to run
``` 
$ fosslight_reuse lint
```
### How to run by mode & Parameters
```fosslight_reuse [Mode] [option1] <arg1> [option2] <arg2>...```   
* Required parameter : **Mode**   
* Optional parameter : **Options**

```
Mode
    lint                  Check REUSE compliance
    report                oss_pkg_info.yaml <-> FOSSLight-Report
    add                   Add missing license and copyright
 
Options:
    -h                    Print help message
    -p <path>             Path to check
    -f <file1,file2,..>   List of files to check
    -o <file_name>        Output file name
    -n                    Don't exclude venv*, node_modules, and .*/ from the analysis
 
Options for only 'add' mode
    -l <license>          License name(SPDX format) to add
    -c <copyright>        Copyright to add(ex, 2015-2021 LG Electronics Inc.)
```
```
(ex1) $ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
(ex2) $ fosslight_reuse report -p /home/test/source
(ex3 )$ fosslight_reuse add -p tests/add -c "2019-2021 LG Electronics Inc." -l "MIT"
```

**(Only for Windows)** Run using executable file   
    1. Download fosslight_reuse_windows.exe from [FOSSLight Reuse](https://github.com/fosslight/fosslight_reuse) - Release.   
    2. Move the executable file to the path where oss-pkg-info.yaml file or FOSSLight-OSS-Report.xlsx is located.   
    3. Double-click the executable file to run it.   


## 📁 Example
### 🏷 lint
```
# ex.1) Analyze for specific folder
(venv)$ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
```
 > ex.1 Result  

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

```
# ex.2) Analyze for specific files
(venv)$ fosslight_reuse lint -p /home/soimkim/test/reuse-example -f "src/load.c,src/dummy.c,src/main.c"
```
 > ex.2 Result

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
▪️ **Demo**     
![demo_lint](images/lint.gif)   


### 🏷 report
```
# ex.1) Convert all oss-pkg-info.yaml or oss-pkg-info.yml in the path(/home/test/source) recursively.
$ fosslight_reuse report -p /home/test/source
```
```
# ex.2) Convert FOSSLight Report to oss-pkg-info.yaml
$ fosslight_reuse report -f src/FOSSLight-Report.xlsx
```

 > Result of oss-pkg-info.yaml <-> FOSSLight-Report.xlsx   
 
 
▪️ **_oss-pkg-info.yaml_**   
```yaml    
Open Source Software Package:
    - name: glibc
      version: 2.3
      source: https://github.com/fsfe/glibc
      license:
      - GPL-3.0
      - LGPL-2.1
      file : 
      - a.c
      - b.c
    - name : dbus
      version : 1.3
      source : https://github.com/fsfe/dbus
      license : GPL-2.0
      file : src/*
      copyright : |
        Copyright (c) 2020 Test
        Copyright (c) 2020 Test
    - name : reuse-tool
      source : https://github.com/fsfe/reuse
      homepage : http://google.com
      license : MIT
      copyright: Copyright (c) 2020 Test
    - name : build-tool
      source : http://gihub.com/bazel
      license : Apache-2.0
      exclude : True
```

▪️ **_FOSS-Report.xlsx_** 
![Report_xlsx](images/fosslight_reuse_report.JPG)

▪️ **Demo**     
![demo_lint](images/report.gif)   


### 🏷 add
```
# ex.1) Add copyright and license to file(s) in the input path
(venv)$ fosslight_reuse add -p tests/add -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
    
# ex.2) Add copyright and license to input file(s)
(venv)$ fosslight_reuse add -f "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```
 > Result  

  ▪️  **Changes in the file - Added copyright or license at the top of the file**  

|Before          |After          |
|:---------------|:--------------|
|![Before](images/fosslight_reuse_add_test.JPG)|![After](images/fosslight_reuse_add_test_result.JPG)|  

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
    Successfully changed header of tests/add_result/test_no_copyright.py
```
▪️ **Demo**   
![demo_lint](images/add.gif)   


## 🔍 How it works
### 🏷 lint
1. Find a OSS Package Information file.
    OSS Package Information File List  
    * Check if at least one of the following files exists (case-free)   
        - oss-pkg-info.yaml
        - oss-pkg-info.yml
        - requirement.txt
        - requirements.txt
        - package.json
        - pom.xml
        - build.gradle
        - Podfile.lock
        - Cartfile.resolved
        - oss-package.info 
        - File started with "MODULE_LICENSE_ "   


2. Run fsfe-reuse lint   
    2-1. When running on a project basis. (without -f parameter)   
    - If there is no ./reuse/dep5 file in the Root Path, it is created.   
    - If it already exists, copy it to bk file and append the default config value to the existing dep file.   
    - By creating dep5 files, exclude binary or .json, venv */*, node_modules/*,. */* from reuse.   
    - Run fsfe-reuse lint (If the OSS Package Information file exists, the list of missing license files is not printed.)   
    - Rollback dep5-related file creation part.   
    
    2-2. When executing in file unit (with -f option)   
    - Print the copyright text and license text extraction by file.   
    - However, if the file does not exist or the file is binary or .json, copyright text and license text are not printed.   
3. Print the execution result and save it in xml format.   

### 🏷 report
1. Check if there is an OSS Package Information or FOSSLight Report file.
    * file example : [oss-pkg-info.yaml][yml], [FOSSLight-Report.xlsx][xlsx]

[yml]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/oss-pkg-info.yaml
[xlsx]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/report/OSS-Report-Sample_0.xlsx

2. Convert oss-pkg-info.yaml file ↔ FOSSLight Report   
    2-1. When running on a project basis. (without -f option)   
    - Convert all files in the path (oss-pkg-info.yaml file ↔ FOSSLight Report)   
    
    2-2. When running in file unit (with -f option)
    - Convert the input file (oss-pkg-info.yaml ↔ FOSSLight-Report.xlsx)
    - However, if an output file name is specified with -o, a result file is created with that name.
    

### 🏷 add
1. Confirm to add copyright and license to missing file   
2. Add copyright and license to missing file(s) using -c and -l option   
    - Print file list that both has copyright and license(excluded from Adding)   
    - Add input copyright and license to missing file(s) using -c and -l option   
