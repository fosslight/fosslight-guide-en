---
published: true
---
# FOSSLight Source Scanner

<img src="https://img.shields.io/pypi/l/fosslight_source" alt="FOSSLight Source is released under the Apache-2.0 License." /> <img src="https://img.shields.io/pypi/v/fosslight_source" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_source" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_source_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_source_scanner)

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner) uses source code scanners, [ScanCode][sc] and [SCANOSS][scanoss]. [ScanCode][sc] detects copyright and license phrases contained in the file and [SCANOSS][scanoss] searches OSS Name, OSS Version, download location, copyright and license information from [OSSKB][osskb]. Some files (ex- build script), binary files, directory and files in specific directories (ex-test) are excluded from the result. And removes words such as "-only" and "-old-style" from the license name to be printed. The output result is generated in spreadsheet format.

[sc]: https://github.com/nexB/scancode-toolkit
[scanoss]: https://github.com/scanoss/scanoss.py
[osskb]: https://osskb.org/

**Github Repository** : [https://github.com/fosslight/fosslight_source_scanner](https://github.com/fosslight/fosslight_source_scanner)    
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_source_scanner/blob/main/LICENSE)

## Contents
  - [Prerequisite](#-prerequisite)
  - [How to install](#-how-to-install)
  - [How to run](#-how-to-run)
    - [1. fosslight_source](#1-fosslight_source)
    - [2. fosslight_convert](#2-fosslight_convert)
  - [Result](#-result)
  - [Using docker](#-how-to-install-and-run-using-docker)


## 📋 Prerequisite
FOSSLight Source Scanner needs a Python 3.7+.    
To use SCANOSS feature, Python 3.7+ is recommended.    
       
⚠️For **windows** and **mac m1**, installation is not possible. In this case, it is recommended to install and use by [using Docker](#-how-to-install-and-run-using-docker).

## 🎉 How to install
It can be installed using pip3. It is recommended to install it in the [python 3.7 + virtualenv](etc/guide_virtualenv.md) environment.
```
$ pip3 install fosslight_source
```

## 🚀 How to run
### 1. fosslight_source
After executing ScanCode, the source code scanner, print the FOSSLight Report.
````
$ fosslight_source [option] <arg>
````  
#### Options
```
  Optional
    -p <source_path>               Path to analyze source (Default: current directory)
    -h                             Print help message
    -j                             Generate raw result of scanners in json format
    -m                             Print additional information for scan result on separate sheets
    -o <output_path>               Output path
                                   (If you want to generate the specific file name, add the output path with file name.)
    -f <format>                    Output file format (excel, csv, opossum)
    -s <scanner>                   Select which scanner to be run (scancode, scanoss, all)

```
If scanner is not specified with -s option, all scanners (ScanCode, SCANOSS) will be run and the result will be merged.

#### Example
Print result to FOSSLight Report and results of ScanCode and SCANOSS in json file.
```
$ fosslight_source -p /home/source_path -j
```

### 2. fosslight_convert
Converts the result of executing ScanCode in json format into FOSSLight Report format.  
````
$ fosslight_convert [option] <arg>
```` 
#### Options
```
  Optional
    -p <path_dir>                  Path of ScanCode json files (Default: current directory)
    -h                             Print help message
    -m                             Print the Matched text for each license on a separate sheet
    -o <output_path>               Output path
                                   (If you want to generate the specific file name, add the output path with file name.)
    -f <format>                    Output file format (excel, csv, opossum)

```
#### Example
Converting scancode json result to FOSSLight report
```
$ fosslight_convert -p /home/jsonfile_dir
```

## 📁 Result

```
$ tree
.
├── fosslight_log_220103_1540.txt
├── fosslight_opossum_220103_1540.json
├── fosslight_report_220103_1540.xlsx
├── fosslight_report_220103_1540.csv
├── scancode_raw_result.json
├── scanner_output.wfp
└── scanoss_raw_result.json
```
- fosslight_log_[datetime].txt : The execution log.
- fosslight_opossum_[datetime].json : FOSSLight Source Scanner result for [OpossumUI](https://github.com/opossum-tool/OpossumUI)
- fosslight_report_[datetime].xlsx : FOSSLight Source Scanner result in spreadsheet format.
- fosslight_report_[datetime].csv : FOSSLight Source Scanner result in csv format.
- scancode_raw_result.json : The ScanCode raw result. (Generated only when the -j option is enabled.)
- scanner_output.wfp : The finger prints generated by SCANOSS. (Generated only when the -j option is enabled.)
- scanoss_raw_result.json : The SCANOSS raw result. (Generated only when the -j option is enabled.)


## 🐳 How to install and run using Docker
1. Build image using Dockerfile.
```
$docker build -t fosslight_source .
```
2. Run with the image you built.      
ex. Output: /Users/fosslight_source_scanner/test_output, Path to be analyzed: tests/test_files
```
$docker run -it -v /Users/fosslight_source_scanner/test_output:/app/output fosslight_source -p tests/test_files -o output
```
