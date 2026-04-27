---
sort: 5
published: true
title: 🚩FOSSLight Prechecker
---
# FOSSLight Prechecker

<img src="https://img.shields.io/pypi/l/fosslight-prechecker" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_prechecker" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_prechecker" /><a href="https://github.com/fosslight/fosslight_prechecker"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_prechecker)](https://api.reuse.software/info/github.com/fosslight/fosslight_prechecker)

[**FOSSLight Prechecker**](https://github.com/fosslight/fosslight_prechecker) is a tool used to check whether the [copyright and license writing rules][rule] in source code are properly applied, and to supplement them if necessary.


[rule]: https://opensource.lge.com/guide/19    


## How to Install
{: .left-bar-title} 

FOSSLight Prechecker can be installed using pip3.
It is recommended to install it in a [python 3.10 + virtualenv](../scanner/etc/guide_virtualenv.md) environment.
```
$ pip3 install fosslight_prechecker
```
<br><br>

## How to Run
{: .left-bar-title}

FOSSLight Prechecker provides the following four modes, each performing different functions for managing copyright and license information in source code.  
1. `lint` --- Check whether the [copyright and license writing rules][rule] in source code are complied with.     
2. `convert` --- Convert [sbom-info.yaml](https://github.com/fosslight/fosslight_prechecker/blob/main/tests/convert/sbom-info.yaml) or [oss-pkg-info.yaml](https://github.com/fosslight/fosslight_prechecker/blob/main/tests/convert/oss-pkg-info.yaml) to [Fosslight_Report.xlsx](https://fosslight.org/hub-guide-en/learn/2_fosslight_report.html) format.
     - The contents of the yaml file are converted to the SRC Sheet of Fosslight_Report.xlsx.  
3. `add` --- Add copyright, license, and download location information to files that are missing copyright or license information.  
4. `download` --- Download the license text of each license specified in the sbom-info.yaml file as individual text files.  

``` 
$ fosslight_prechecker [Mode] [option1] <arg1> [option2] <arg2>...
```

### How to Run by Mode & Parameters
{: .specific-title}
* Required parameter : **Modes**   
* Optional parameter : **Options**

```
 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_prechecker [modes] [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Prechecker checks and corrects copyright and license writing
    rules in source code. It can lint, add, convert, and
    download license information.

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/1_prechecker.html

    🔧 Modes
    ────────────────────────────────────────────────────────────────────
    lint (default)         Check copyright and license writing rules compliance
    add                    Add missing license, copyright, and download location
    convert                Convert sbom-info.yaml to FOSSLight-Report.xlsx
    download               Download license text specified in sbom-info.yaml

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Path to check (default: current directory)
    -o <file>              Output file name
    -f <format>            Result format (yaml, xml, html)
    -e <pattern>           Exclude paths from checking (files and directories)
                           (only works with 'lint' mode)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight_prechecker -e "test/" "*.pyc"
    -h                     Show this help message
    -v                     Show version information
    -i                     Don't write log file and show progress bar
    --notice               Print the open source license notice text

    🔍 Mode-Specific Options
    ────────────────────────────────────────────────────────────────────
    lint mode:
      -n                   Don't exclude venv*, node_modules, .*/, and
                           FOSSLight Scanner results from analysis

    add mode:
      -l <license>         Add license name in SPDX format (ex: "Apache-2.0")
      -c <copyright>       Add copyright text (ex: "2015-2021 LG Electronics Inc.")
      -u <url>             Add download location URL(ex: "https://www.sampleurl.com")

    download mode:
      -l <license>         License to be representative license

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Lint current directory (check compliance)
    fosslight_prechecker lint

    # Lint specific path with exclusions
    fosslight_prechecker lint -p /path/to/source -e "test/" "node_modules/"

    # Add license and copyright to a file
    fosslight_prechecker add -p test.py -l "GPL-3.0-only" -c "2019-2021 LG Electronics Inc."

    # Add license, copyright, and download location
    fosslight_prechecker add -p src/main.py -l "Apache-2.0" -c "2023 MyCompany" -u "https://github.com/user/repo"

    # Convert sbom-info.yaml to Excel report
    fosslight_prechecker convert -p sbom-info.yaml

    # Download license text
    fosslight_prechecker download -l "MIT"
```
- [Pattern Matching Guide](https://scancode-toolkit.readthedocs.io/en/stable/reference/scancode-cli/cli-pre-scan-options.html#glob-pattern-matching) for the `-e` option
   - ⚠️ Always use double quotes ("") when entering values.
       - Example) fosslight_prechecker -e "dev/" "tests/"
   - ⚠️ File names and extensions are **case-sensitive**, so please enter them exactly as intended. <br>

- **(For Windows)** Run using executable file  
  - Download fosslight_prechecker_windows.exe from [FOSSLight Prechecker - Release](https://github.com/fosslight/fosslight_prechecker/releases)  
  - Two methods available:
    - 1) Move the executable to the desired path and double-click to run  
      - Runs only the default Lint mode  
    - 2) Run via command line     
      - Open 'cmd'    
      - Run from the path where the file is located, following the 'How to Run by Mode and Parameters' instructions  
         ex) fosslight_prechecker lint -p src/
    
<br><br>    

## lint mode
{: .left-bar-title}
- Result files
  ```
  $ tree
  .
  ├── fosslight_lint_260423_1630.yaml
  └── fosslight_log_pre_260423_1630.txt

  ```

### 1. Analyze a specific path
{: .specific-title}

```
(venv)$ fosslight_prechecker lint -p /home/tests -o result.yaml
```
- Result
    <pre>
       Checking copyright/license writing rules:
          Compliant: Not-OK
          Summary:
            Open Source Package File:
            - convert/sbom-info.yaml
            - add/oss-pkg-info.yaml
            - convert/oss-pkg-info.yaml
            - lint/sub1/sbom-info.yaml
            - lint/sub2/oss-pkg-info.yaml
            Detected Licenses:
            - Apache-2.0
            - GPL-3.0-only
            - MIT
            Files without license / total: 3 / 16
            Files without copyright / total: 3 / 16
          Files without license and copyright:
          - lint/fosslight_lint_260424_1022.yaml
          - lint/fosslight_log_pre_260424_1022.txt
          Files without license:
          - lint/sub1/source_missing_lic.py
          Files without copyright:
          - lint/sub2/source_missing_cop.py
          Tool Info:
            OS: Linux 5.15.0-138-generic
            Analyze path: tests
            Python version: 3
            fosslight_prechecker version: fosslight_prechecker v4.0.8
    </pre>

### 2. Analyze specific files
{: .specific-title}  
```
(venv)$ fosslight_prechecker lint -p "src/file1.py,src/file2.py"
```
- Result
    <pre>
        [FOSSLIGHT_PRECHECKER] Tool Info : fosslight_prechecker v4.0.8
        [FOSSLIGHT_PRECHECKER] # src/fosslight_prechecker/cli.py
        [FOSSLIGHT_PRECHECKER] * License: GPL-3.0-only
        [FOSSLIGHT_PRECHECKER] * Copyright: Copyright (c) 2021 LG Electronics Inc.
        Copyright to add(used in only 'add' mode)", type=str, dest='copyright', default="")

        [FOSSLIGHT_PRECHECKER] # src/fosslight_prechecker/_result.py
        [FOSSLIGHT_PRECHECKER] * License: GPL-3.0-only
        [FOSSLIGHT_PRECHECKER] * Copyright: Copyright (c) 2022 LG Electronics Inc.
        Copyright and License Writing Rules in Source Code. : " + RULE_LINK

        Checking copyright/license writing rules:
          Compliant: OK
          Summary:
            Open Source Package File: N/A
            Detected Licenses: N/A
            Files without license / total: 0 / 2
            Files without copyright / total: 0 / 2
          Files without license and copyright: N/A
          Files without license: N/A
          Files without copyright: N/A
          Tool Info:
            OS: Linux 5.15.0-138-generic
            Analyze path:
            - src/fosslight_prechecker/cli.py
            - src/fosslight_prechecker/_result.py
            Python version: 3
            fosslight_prechecker version: fosslight_prechecker v4.0.8
    </pre>
<ul>
 <li>
 <details>
   <summary><strong>Contents of result</strong></summary>

   <p>Depending on the format, the resulting output may differ.
   (Default format: yaml)</p>

   <ul>
     <li><b>Compliant</b>: Whether the lint result is compliant (OK or Not OK)</li>
     <li><b>Files without copyright</b>:
       List of files without copyright</li>
     <li><b>Files without license</b>:
       List of files without a license</li>
     <li><b>Files without license and copyright</b>:
       List of files missing both copyright and license</li>

     <li>
       <b>Summary</b>
       <ul>
         <li><b>Detected Licenses</b>: Licenses detected in the source code</li>
         <li><b>Files without copyright / total</b>:
             Number of files without copyright / Total number of files</li>
         <li><b>Files without license / total</b>:
             Number of files without license / Total number of files</li>
         <li><b>Open Source Package File</b>:
             List of sbom-info*.yaml or oss-pkg-info*.yaml files</li>

         <li>
           <b>Tool Info</b>
           <ul>
             <li><b>Analysis path</b>: Path that was analyzed</li>
             <li><b>OS</b>: OS version on which FOSSLight Prechecker was executed</li>
             <li><b>Python version</b>:
                 Python version on which FOSSLight Prechecker was executed</li>
             <li><b>fosslight_prechecker version</b>:
                 FOSSLight Prechecker version</li>
           </ul>
         </li>
       </ul>
     </li>

     <li>
       <details>
         <summary><strong>Items excluded when calculating the number of files</strong></summary>
         <ul>
           <li>Hidden files and files within hidden directories</li>
           <li>Binary files</li>
           <li>Files with specific extensions (jar, png, exe, so, a, dll, jpeg, jpg, ttf, lib, ttc, pfb, pfm, otf, afm, dfont, json)</li>
           <li>Files within venv and node_modules directories</li>
           <li>Files defined in .gitignore</li>
           <li>Untracked files based on git repo</li>
           <li>User-defined excluded paths (-e option)</li>
           <li>
             Paths with exclude set to True in
             sbom-info.yaml or oss-pkg-info.yaml
           </li>
         </ul>
       </details>
     </li>
   </ul>
 </details>
 </li>
</ul>

<br><br>

## add mode
{: .left-bar-title} 
If a file does not have existing copyright or license information, the missing information will be added to each file.  

### 1. Add copyright and license to files in a specific path
{: .specific-title}

```
(venv)$ fosslight_prechecker add -p tests/add -c "2025-2026 LG Electronics Inc." -l "GPL-3.0-only" -u "https://www.testurl.com"
```
<ul>
 <li>
 <details markdown="1">
 <summary markdown="span">Result</summary>

  ```bash
  [FOSSLIGHT_PRECHECKER] 
  # File list that have both license and copyright : 2 / 5
  # test_both_have_1.py
  * License: GPL-3.0-only
  * Copyright: SPDX-FileCopyrightText: Copyright 2019-2021 LG Electronics Inc.

  # test_both_have_2.py
  * License: GPL-3.0-only
  * Copyright: SPDX-FileCopyrightText: Copyright (c) 2011 LG Electronics Inc.

  # Missing license File(s)
    * oss-pkg-info.yaml
    * test_no_license.py
    * Your input license : GPL-3.0-only
  Successfully changed header of tests/add/oss-pkg-info.yaml
  Successfully changed header of tests/add/test_no_license.py

  # Missing Copyright File(s) 
    * test_no_copyright.py
    * Your input Copyright : Copyright 2025-2026 LG Electronics Inc.
  Successfully changed header of tests/add/test_no_copyright.py

  # Adding Download Location into your files
    * Your input DownloadLocation : https://www.testurl.com
  Successfully changed header of tests/add/test_both_have_1.py
  Successfully changed header of tests/add/test_both_have_2.py
  Successfully changed header of tests/add/test_no_copyright.py
  Successfully changed header of tests/add/oss-pkg-info.yaml
  Successfully changed header of tests/add/test_no_license.py
  OS: Linux 5.15.0-138-generic
  Path to analyze: tests/add
  Python version: 3
  Tool Info: fosslight_prechecker v4.0.8
  ```

 </details>
 </li>
</ul>

### 2. Add copyright and license to specific files
{: .specific-title} 

```
(venv)$ fosslight_prechecker add -p "tests/add/test_both_have_1.py,tests/add/test_both_have_2.py,tests/add/test_no_copyright.py,tests/add/test_no_license.py" -c "2025-2026 LG Electronics Inc." -l "GPL-3.0-only" -u "https://www.testurl.com"
```
<ul>
 <li>
 <details markdown="1">
 <summary markdown="span">Result</summary>

  ```python
  # SPDX-FileCopyrightText: Copyright 2025-2026 LG Electronics Inc.
  #
  # SPDX-License-Identifier: GPL-3.0-only
  #
  # SPDX-PackageDownloadLocation: https://www.testurl.com

  import os
  from fosslight_util.set_log import init_log

  ```

 </details>
 </li>
</ul>

<br><br>

## convert mode
{: .left-bar-title} 
- Result files 
  ```
  $ tree
  ├── fosslight_report_pre_260424_1010.xlsx
  └── fosslight_log_pre_260424_1010.txt
  ```

### Convert OSS information in YAML format to Fosslight Report  
{: .specific-title}  
- Analyzes sbom-info.yaml or oss-pkg-info.yaml files in the specified path and converts them to an Excel report in Fosslight_Report.xlsx format.  
- If there are multiple YAML files, each file is converted to a separate individual sheet.  
```
$ fosslight_prechecker convert -p tests/
```
<ul>
<li>Result
<ul>
<li>
<details markdown="1">
<summary markdown="span">oss-pkg-info.yaml</summary>

When writing a path in the yaml file, if it starts with a special character ({, }, [, ], &, *, #, ?, |, -, <, >, =, !, %, @), use double quotation marks ("").

```yaml
glibc:
- version: '2.3'
  source name or path:
  - tests/b.c
  - tests/a.c
  license:
  - GPL-3.0
  - LGPL-2.1
  download location: https://github.com/fsfe/glibc
dbus:
- version: '1.3'
  source name or path:
  - tests/src/*
  license:
  - GPL-2.0
  download location: https://github.com/fsfe/dbus
  copyright text: 'Copyright (c) 2020 Test Copyright (c) 2020 Sample'
reuse-tool:
- version: ''
  source name or path:
  - tests/
  license:
  - MIT
  download location: https://github.com/fsfe/reuse
  homepage: http://google.com
  copyright text: Copyright (c) 2020 Test
build-tool:
- version: ''
  source name or path:
  - tests/
  license:
  - Apache-2.0
  download location: http://gihub.com/bazel
  exclude: true
```

</details>
</li>
<li>
<details markdown="1">
<summary markdown="span">fosslight_report.xlsx</summary>

<img src="images/fosslight_reuse_report.png" alt="FOSSLight Report">

</details>
</li>
</ul>
</li>
</ul>

<br><br>



## download mode
{: .left-bar-title} 
1. **Download licenses written in sbom-info.yaml as text files**
```
(venv)$ fosslight_prechecker download -p tests/
```
 - Result
 ```bash
  Successfully downloaded tests/LICENSES/GPL-3.0-only.txt.
  Successfully downloaded tests/LICENSES/MIT.txt.
  Successfully downloaded tests/LICENSES/LGPL-2.1-only.txt.
  Successfully downloaded tests/LICENSES/GPL-2.0-only.txt.
 ``` 

2. **Example of creating a representative license file**
```
(venv)$ fosslight_prechecker download -p tests/ -l "Apache-2.0"
```
- Result
```bash
 Successfully downloaded tests/temp_lic/LICENSES/Apache-2.0.txt.
 # Created Representative License File (tests/temp_lic/LICENSES/Apache-2.0.txt -> LICENSE)
```
