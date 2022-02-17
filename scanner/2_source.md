---
published: true
---
# FOSSLight Source Scanner

<img src="https://img.shields.io/pypi/l/fosslight_source" alt="FOSSLight Source is released under the Apache-2.0 License." /> <img src="https://img.shields.io/pypi/v/fosslight_source" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_source" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_source_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_source_scanner)

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner) uses source code scanners, [ScanCode][sc] and [SCANOSS][scanoss]. [ScanCode][sc] detects copyright and license phrases contained in the file and [SCANOSS][scanoss] searches OSS Name, OSS Version, download location, copyright and license information from [OSSKB][osskb]. Some files (ex- build script), binary files, directory and files in specific directories (ex-test) are excluded from the result. And removes words such as "-only" and "-old-style" from the license name to be printed. The output result is generated in spreadsheet format.

[sc]: https://github.com/nexB/scancode-toolkit
[scanoss]: https://github.com/scanoss/scanoss.py
[osskb]: https://osskb.org/

**Github Repository** : [https://github.com/fosslight/fosslight_source_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_source_scanner/blob/main/LICENSE)

## Contents
  - [Prerequisite](#-prerequisite)
  - [How to install](#-how-to-install)
  - [How to run](#-how-to-run)
    - [1. fosslight_source](#1-fosslight_source)
    - [2. fosslight_convert](#2-fosslight_convert)
  - [Result](#-result)


## 📋 Prerequisite
FOSSLight Source Scanner needs a Python 3.6+.    
For windows, you need to install [Microsoft Visual C++ Build Tools][ms_build].
To use SCANOSS feature, Python 3.7+ is recommended.

[ms_build]: https://visualstudio.microsoft.com/vs/older-downloads/

## 🎉 How to install
It can be installed using pip3. It is recommended to install it in the [python 3.6 + virtualenv](etc/guide_virtualenv.md) environment.
```
$ pip3 install fosslight_source
```
```warning
If you are using python 3.6 and you wish to use scanoss feature, you'll need to install ['scanoss'](https://pypi.org/project/scanoss/) and ['dataclasses'](https://pypi.org/project/dataclasses/) seperately.
```

## 🚀 How to run
### 1. fosslight_source
After executing ScanCode, the source code scanner, print the FOSSLight Report.
````
$ fosslight_source [option] <arg>
````  
#### Options
```
  Mandatory
    -p <source_path>               Path to analyze source

  Optional
    -h                             Print help message
    -j                             Generate raw result of scanners in json format
    -m                             Print the Matched text for each license on a separate sheet (Scancode Only)
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
  Mandatory
    -p <path_dir>                  Path of ScanCode json files

  Optional
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
├── FOSSLight-Report_20220103_154024_SRC_FL_Source.csv
├── FOSSLight-Report_20220103_154024.xlsx
├── fosslight_src_log_20220103_154024.txt
├── scancode_raw_result.json
├── scanoss_fingerprint.wfp
├── scanoss_raw_result.json
└── Opossum_input_20220103_154024.json
```
- FOSSLight-Report_[datetime]_[sheet_name].csv : FOSSLight Source Scanner result in csv format.
- FOSSLight-Report_[datetime].xlsx : FOSSLight Source Scanner result in spreadsheet format.
- fosslight_src_log_[datetime].txt : The execution log.
- scancode_raw_result.json : The ScanCode raw result. (Generated only when the -j option is enabled.)
- scanoss_fingerprint.wfp : The finger prints generated by SCANOSS. (Generated only when the -j option is enabled.)
- scanoss_raw_result.json : The SCANOSS raw result. (Generated only when the -j option is enabled.)
- Opossum_input_[datetime].json : FOSSLight Source Scanner result for [OpossumUI](https://github.com/opossum-tool/OpossumUI)