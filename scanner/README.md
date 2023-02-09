---
sort: 5
published: true
title: 🚩FOSSLight Scanner
---
# FOSSLight Scanner

<a href="https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/fosslight_scanner" alt="FOSSLight Scanner is released under the Apache-2.0." /></a> <a href="https://pypi.org/project/fosslight-scanner/"><img src="https://img.shields.io/pypi/v/fosslight_scanner" alt="Current python package version." /></a> <img src="https://img.shields.io/pypi/pyversions/fosslight_scanner" />

FOSSLight Scanner can perform an analysis for open source compliance at once. It can perform open source analysis of source code, binary and dependency. Also, it can check whether the copyright/license writing rules in the source code are followed.  <br/>

It works using the following scanners:

1. [FOSSLight Prechecker](1_prechecker.md) : Check whether the source code's copyright and license writing rules are complied with.
2. [FOSSLight Source Scanner](2_source.md) : Analyze the source code and generate open source analysis result.
3. [FOSSLight Dependency Scanner](3_dependency.md) : Analyze the dependencies used through package manager or build system and generate open source analysis result. 
4. [FOSSLight Binary Scanner](4_binary.md) : Analyze the binaries and generate open source analysis result.
5. [FOSSLight Yocto Scanner](5_yocto.md) : Analyze open source for yocto-based projects.
<br />

**Github Repository** : [https://github.com/fosslight/fosslight_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE)

## Contents
- [📋 Prerequisite](#-prerequisite)
- [🎉 How to install](#-how-to-install)
- [🚀 How to run](#-how-to-run)
- [📁 Result](#-result)
- [🐳 Using Docker](#-how-to-install-and-run-using-docker)

## 📋 Prerequisite
1. FOSSLight Scanner needs a Python 3.7+.    
2. To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).
3. (Only for windows) Install Microsoft Build Tools from https://visualstudio.microsoft.com/en/vs/older-downloads/ > Redistributables packages and Build Tools.

## 🎉 How to install
It can be installed using pip3. It is recommended to install it in the [python 3.7 + virtualenv](etc/guide_virtualenv.md) environment.
```
$ pip3 install fosslight_scanner
```

## 🚀 How to run
### How to run by mode & Parameters
```
$ fosslight [Mode] [option1] <arg1> [option2] <arg2>...
```
```
 Parameters:
    Mode
        source                  Run FOSSLight Source
        dependency              Run FOSSLight Dependency
        binary                  Run FOSSLight Binary
        prechecker              Run FOSSLight Prechecker
        all                     Run all scanners
        compare                 Compare two FOSSLight reports
 
    Options:
        -h                      Print help message
        -p <path>               Path to analyze (ex, -p {input_path})
                                 * Compare mode input file: Two FOSSLight reports (supports excel, yaml)
                                   (ex, -p {before_name}.xlsx {after_name}.xlsx)
        -w <link>               Link to be analyzed can be downloaded by wget or git clone
        -f <format>             FOSSLight Report file format (excel, yaml)
                                 * Compare mode result file: supports excel, json, yaml, html
        -o <output>             Output directory or file
        -c <number>             Number of processes to analyze source
        -r                      Keep raw data
        -t                      Hide the progress bar
        -v                      Print FOSSLight Scanner version
 
    Options for only 'all' or 'bin' mode
        -u <db_url>             DB Connection(format :'postgresql://username:password@host:port/database_name')
 
    Options for only 'all' or 'dependency' mode
        -d <dependency_argument>        Additional arguments for running dependency analysis
```
- Enter the -d option only when argument input is required when running FOSSLight Dependency. : [Refer FOSSLight Dependency guide](3_dependency.md)

#### Ex.1 Local Source Analysis
```
fosslight all -p /home/source_path
```

#### Ex.2 Download Link and analyze
```
fosslight all -o test_result_wget -w "https://github.com/LGE-OSS/example.git"
```

#### Ex.3 Compare the BOM of two FOSSLight reports with yaml or excel format and check the oss status (change/add/delete).
```
fosslight compare -p FOSSLight_before_proj.yaml FOSSLight_after_proj.yaml -o test_result
```

## 📁 Result
### Result for the mode that analyze the open source (all, source, dependency, binary)
```
test_result/
├── fosslight_binary_220214_1824.txt
├── fosslight_log
│   └── fosslight_log_220214_1824.txt
├── fosslight_lint_220214_1824.yaml
├── fosslight_report_220214_1824.xlsx
└── fosslight_raw_data
    ├── fosslight_src_220214_1824.xlsx
    ├── fosslight_bin_220214_1824.xlsx
    └── fosslight_dep_220214_1824.xlsx
```
- fosslight_lint_(datetime).yaml : yaml file created as a result of running FOSSLight Prechecker.
- fosslight_binary_(datetime).txt : FOSSLight Binary result and checksum and tlsh values for each binary are extracted.
- fosslight_report_(datetime).xlsx : FOSSLight Report format file in which source code analysis, binary analysis, and dependency analysis results are written.
- fosslight_raw_data directory: The folder where the analysis result raw data file is created (with -r option)
  - fosslight_src_(datetime).xlsx : FOSSLight Report of FOSSLight Source Scanner
  - fosslight_dep_(datetime).xlsx : FOSSLight Report of FOSSLight Dependency Scanner
  - fosslight_bin_(datetime).xlsx : FOSSLight Report of FOSSLight Binary Scanner

### Result for compare mode
```
test_result/
├── fosslight_log
│   └── fosslight_log_20220817_114259.txt
└── fosslight_compare_20220817_114259.xlsx
```
- fosslight_compare_(datetime).xlsx : Two BOM comparison results in the form of (add/delete/change) table.

## 🐳 How to install and run using Docker
1. Build image using Dockerfile.
```
$docker build -t fosslight .
```
2. Run with the image you built.      
ex. Output: /Users/fosslight_scanner/test_output, Path to be analyzed: tests/test_files
```
$docker run -it -v /Users/fosslight_scanner/test_output:/app/output fosslight -p tests/test_files -o output
```
