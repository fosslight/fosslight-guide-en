---
sort: 5
published: true
title: 🚩FOSSLight Prechecker
---
# FOSSLight Prechecker

<img src="https://img.shields.io/pypi/l/fosslight-prechecker" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_prechecker" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_prechecker" /> <a href="https://github.com/fosslight/fosslight_prechecker"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_prechecker)](https://api.reuse.software/info/github.com/fosslight/fosslight_prechecker)

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
2. `convert` --- Convert [sbom-info.yaml](https://github.com/fosslight/fosslight_prechecker/blob/main/tests/convert/sbom-info.yaml) into the [Fosslight_Report.xlsx](https://fosslight.org/hub-guide-en/learn/2_fosslight_report.html) format and reflect its contents in the SRC sheet.  
3. `add` --- Add copyright, license, and download location information to files that are missing copyright or license information.  
4. `download` --- Download the license text of each license specified in the sbom-info.yaml file as individual text files.  

``` 
$ fosslight_prechecker [Mode] [option1] <arg1> [option2] <arg2>...
```

- **(For Windows)** Using the executable  
  - Download `fosslight_prechecker_windows.exe` from [FOSSLight Prechecker - Release](https://github.com/fosslight/fosslight_prechecker/releases). Double-clicking it runs lint mode in that directory; open **cmd** to run other modes.  

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

    📚 Guide:  https://fosslight.org/fosslight-guide/prechecker

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
- **Result output fields**  
Depending on the format, the fields written to the result may differ. (Default format: yaml)

  - Compliant: Whether the lint result is compliant (OK or Not OK)
  - Files without copyright: List of files without copyright
  - Files without license: List of files without a license
  - Files without license and copyright: List of files missing both copyright and license
  - Summary
      - Detected Licenses: Licenses detected in the source
      - Files without copyright / total: Count of files without copyright / total file count
      - Files without license / total: Count of files without license / total file count
      - Files without license and copyright / total: Count of files missing both / total file count
      - Open Source Package File: List of sbom-info*.yaml or oss-pkg-info*.yaml files
      - Tool Info
        - Analysis path: Path that was analyzed
        - OS: OS version on which FOSSLight Prechecker was executed
        - Python version: Python version on which FOSSLight Prechecker was executed
        - fosslight_prechecker version: FOSSLight Prechecker version

  
  <ul>
  <li>
    <details>
      <summary><strong>※ Items excluded when counting files ※ </strong></summary>
      <ul>
      <li>Hidden files and files inside hidden directories</li>
      <li>Binary files</li>
      <li>Files with certain extensions (jar, png, exe, so, a, dll, jpeg, jpg, ttf, lib, ttc, pfb, pfm, otf, afm, dfont, json)</li>
      <li>Files inside venv and node_modules directories</li>
      <li>Files listed in .gitignore</li>
      <li>Untracked files relative to the git repository</li>
      <li>User-defined excluded paths (`-e` option)</li>
      <li>
        Paths in sbom-info.yaml where
        exclude is True
      </li>
      </ul>
    </details>      
  </li>
  </ul>

<ul>
<li>
<details markdown="1">
<summary markdown="span">Example 1) Analyze a specific path</summary>

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
            - lint/sub1/sbom-info.yaml
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

</details>
</li>
</ul>

<ul>
<li>
<details markdown="1">
<summary markdown="span">Example 2) Analyze specific files</summary>

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

</details>
</li>
</ul>


<br><br>

## add mode
{: .left-bar-title} 
If a file has no existing copyright or license information, the corresponding information is added for each item.  

- **Add copyright and license to files under a specific path**  

```
(venv)$ fosslight_prechecker add -p tests/add -c "2025-2026 LG Electronics Inc." -l "GPL-3.0-only" -u "https://www.testurl.com"
```
- **Add copyright and license to specific files**  

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
- Analyzes sbom-info.yaml files under the specified path and converts them to an Excel report in Fosslight_Report.xlsx format.  
- If there are multiple YAML files, each file is converted to a separate individual sheet.  
```
$ fosslight_prechecker convert -p tests/
```
<ul>
<li>Result
<ul>
<li>
<details markdown="1">
<summary markdown="span">sbom-info.yaml file</summary>

<p>When writing a path in the yaml file, if it starts with a special character ({, }, [, ], &, *, #, ?, |, -, <, >, =, !, %, @), enclose it in double quotes ("").</p>

```yaml
libidn:
- version: "1.5"
  source name or path:
  - a.c
  - b.c
  license:
  - "GPL-3.0"
  - "LGPL-2.1"
  download location: "http://ftp.gnu.org/gnu/libidn"
  homepage: "https://www.gnu.org/software/libidn"
  copyright text: "Copyright (c) 2002-2007, Simon Josefsson"
node-backoff:
- version: "2.5.0"
  source name or path: "src/*"
  license: "MIT"
  download location: "https://github.com/MathieuTurcotte/node-backoff"
  homepage: "https://www.npmjs.com/package/backoff"
  copyright text: "Copyright (c) 2012 Mathieu Turcotte"
  exclude: True
rsync:
- version: "2.6.9"
  source name or path: "test/tool"
  license: "GPL-2.0"
  download location: "https://download.samba.org/pub/rsync/src"
  homepage: "http://rsync.samba.org"
- version: "3.1.2"
  source name or path: "test/tool"
  license: "GPL-3.0"
  download location: "https://download.samba.org/pub/rsync/src"
  homepage: "http://rsync.samba.org"
  copyright text:
  - "Copyright (c) 1996 Andrew Tridgell"
  - "Copyright (c) 1996 Paul Mackerras"
  - "Copyright (c) 2003-2015 Wayne Davison"
```

</details>
</li>
<li>
<details markdown="1">
<summary markdown="span">fosslight_report.xlsx file</summary>

<img src="images/fosslight_reuse_report.png" alt="FOSSLight Report">

</details>
</li>
</ul>
</li>
</ul>

<br><br>



## download mode
{: .left-bar-title} 
1. **Example: download each license from sbom-info.yaml as a text file**
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

2. **Example: create a representative license file**
```
(venv)$ fosslight_prechecker download -p tests/ -l "Apache-2.0"
```
- Result
```bash
 Successfully downloaded tests/temp_lic/LICENSES/Apache-2.0.txt.
 # Created Representative License File (tests/temp_lic/LICENSES/Apache-2.0.txt -> LICENSE)
```
