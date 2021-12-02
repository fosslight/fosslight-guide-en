---
published: true
title: FOSSLight Reuse
---
# FOSSLight Reuse

<img src="https://img.shields.io/pypi/l/fosslight-reuse" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_reuse" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_reuse" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_reuse)](https://api.reuse.software/info/github.com/fosslight/fosslight_reuse)
    

[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse) is a tool that can be used to comply with the [copyright/license writing rules][rule] in the source code using [reuse-tool][ret].

[ret]: https://github.com/fsfe/reuse-tool
[rule]: https://oss.lge.com/guide/process/osc_process/1-identification/copyright_license_rule.html

**Github Repository** : [https://github.com/fosslight/fosslight_reuse]()  
**License** : [GPL-3.0-only](https://github.com/fosslight/fosslight_reuse/blob/main/LICENSE)

## Contents
- [Prerequisite](#-prerequisite)
- [How to install](#-how-to-install)
- [How to run](#-how-to-run)
- [Result](#-result)
- [How it works](#-how-it-works)

## 📋 Prerequisite
[**FOSSLight Reuse**](https://github.com/fosslight/fosslight_reuse) needs a Python 3.6+.   

## 🎉 How to install
It can be installed using pip3.     
It is recommended to install it in the [python 3.6 + virtualenv](https://fosslight.org/fosslight-guide-en/scanner/etc/guide_virtualenv.html) environment.

```
$ pip3 install fosslight_reuse
```

## 🚀 How to run
FOSSLight Reuse has 3 modes as following:
1. `lint` --- Check whether the [source code's copyright and license writing rules][rule] are complied with.    
2. `convert` --- Convert [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/oss-pkg-info.yaml) to [FOSSLight-Report.xlsx](../learn/2_fosslight_report.md) and vice versa.
     - It converts oss-pkg-info.yaml to SRC Sheet of FOSSLight Report or    
     - BIN (Android) and BOM Sheet of FOSSLight Report to oss-pkg-info.yaml.
3. `add` --- Add copyright and license to source code which is missing copyright and license

``` 
fosslight_reuse [Mode] [option1] <arg1> [option2] <arg2>...
```

### How to run by mode & Parameters
* Required parameter : **Mode**   
* Optional parameter : **Options**

```
Mode
    lint                  Check REUSE compliance
    convert               Convert oss_pkg_info.yaml <-> FOSSLight-Report
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

**(Only for Windows)** Run using executable file   
    1. Download fosslight_reuse_windows.exe from [FOSSLight Reuse](https://github.com/fosslight/fosslight_reuse) - Release.   
    2. Move the executable file to the path where [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/oss-pkg-info.yaml) file or [FOSSLight-OSS-Report.xlsx](../learn/2_fosslight_report.md) file is located.   
    3. Double-click the executable file to run it.   


## 📁 Result
### 🔖 lint mode
**1) Analyze for specific folder**
```
(venv)$ fosslight_reuse lint -p /home/test/reuse-example -o result.xml
```
- Result  
    <pre>
    # SUMMARY
    # Open Source Package info: File to which OSS Package information is written.
    # Used licenses: License detected in the path.
    # Files with copyright information: Number of files with copyright / Total number of files.
    # Files with license information: Number of files with license / Total number of files.

    * Open Source Package info: /home/test/reuse-example/oss-package.info
    * Used licenses: CC-BY-4.0, CC0-1.0, GPL-3.0-or-later
    * Files with copyright information: 6 / 7
    * Files with license information: 6 / 7 </pre>

**2) Analyze for specific files**
```
(venv)$ fosslight_reuse lint -p /home/soimkim/test/reuse-example -f "src/load.c,src/dummy.c,src/main.c"
```
- Result
    <pre>
        # src/load.c
        * License:
        * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com>
        
        # src/dummy.c
        * License:
        * Copyright:
        
        # src/main.c
        * License: GPL-3.0-or-later
        * Copyright: SPDX-FileCopyrightText: 2019 Jane Doe <jane@example.com> </pre>

<details>
    <summary markdown="span" style="font-weight:bold">Demo Video (lint)</summary>
    <img src="images/lint.gif" alt="demo video for lint mode">
</details>


### 🔖 convert mode
**1) Convert all oss-pkg-info.yaml or oss-pkg-info.yml in the path recursively.**
```
$ fosslight_reuse convert -p /home/test/source
```

**2) Convert FOSSLight Report to oss-pkg-info.yaml**
```
$ fosslight_reuse convert -f src/FOSSLight-Report.xlsx
```

**3) Result file example**

{::options parse_block_html="true" /}
> <details>
> <summary markdown="span">oss-pkg-info.yaml</summary>
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
> </details>
> <details>
> <summary markdown="span">FOSSLight-Report.xlsx</summary>
<img src="images/fosslight_reuse_report.JPG" alt="FOSSLight Report">
> </details>

<details>
<summary markdown="span" style="font-weight:bold">Demo Video (convert)</summary>
<img src="images/convert.gif" alt="demo video for convert mode">
</details>
{::options parse_block_html="false" /}


### 🔖 add mode
**1) Add copyright and license to file(s) in the input path**
```
(venv)$ fosslight_reuse add -p tests/add -c "Copyright 2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```

**2) Add copyright and license to input file(s)**
```
(venv)$ fosslight_reuse add -f "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "2019-2021 LG Electronics Inc." -l "GPL-3.0-only"
```

**3) Result**
▪️ Changes in the file - Added copyright or license at the top of the file

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

<details>
    <summary markdown="span" style="font-weight:bold">Demo Video (add)</summary>
    <img src="images/add.gif" alt="demo video for add mode">
</details>

## 🔍 How it works
### 🔖 lint mode
1. Find a OSS Package Information file.
    <details>
    <summary markdown="span">Check if at least one of the following files exists (case-free)</summary>
    <ul>
    <li>oss-pkg-info.yaml</li>
    <li>oss-pkg-info.yml</li>
    <li>requirement.txt</li>
    <li>requirements.txt</li>
    <li>package.json</li>
    <li>pom.xml</li>
    <li>build.gradle</li>
    <li>Podfile.lock</li>
    <li>Cartfile.resolved</li>
    <li>oss-package.info </li>
    <li>File started with "MODULE_LICENSE_ "</li>
    </ul>
    </details>

2. Run fsfe-reuse lint   
    2-1. When running on a project basis. (without -f parameter)   
    - If there is no ./reuse/dep5 file in the Root Path, it is created.   
    - If it already exists, copy it to bk file and append the default config value to the existing dep file.   
    - By creating dep5 files, exclude binary or .json, venv */*, node_modules/*,. */* from reuse.   
    - Run fsfe-reuse lint (If the OSS Package Information file exists, the list of missing license files is not printed.)   
    - Recover to existing dep5-related file if it originally existed, delete if it doesn't exist.
    
    2-2. When executing in file unit (with -f option)   
    - Print the copyright text and license text extraction by file.   
    - However, if the file does not exist or the file is binary or .json, copyright text and license text are not printed.   
3. Print the execution result and save it in xml format.   

### 🔖 convert mode
1. Check if there is an OSS Package Information or FOSSLight Report file.
    * file example : [oss-pkg-info.yaml][yml], [FOSSLight-Report.xlsx][xlsx]

[yml]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/oss-pkg-info.yaml
[xlsx]: https://github.com/fosslight/fosslight_reuse/blob/main/tests/convert/OSS-Report-Sample_0.xlsx

2. Convert oss-pkg-info.yaml file ↔ FOSSLight Report   
    2-1. When running on a project basis. (without -f option)   
    - Convert all files in the path (oss-pkg-info.yaml file ↔ FOSSLight Report)   
    
    2-2. When running in file unit (with -f option)
    - Convert the input file (oss-pkg-info.yaml ↔ FOSSLight-Report.xlsx)
    - However, if an output file name is specified with -o, a result file is created with that name.
    

### 🔖 add mode
1. Confirm to add copyright and license to missing file   
2. Add copyright and license to missing file(s) using -c and -l option   
    - Print file list that both has copyright and license(excluded from Adding)   
    - Add input copyright and license to missing file(s) using -c and -l option   
