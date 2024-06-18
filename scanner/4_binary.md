---
published: true
---
# FOSSLight Binary Scanner

<img src="https://img.shields.io/pypi/l/fosslight_binary" alt="FOSSLight Binary is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_binary" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_binary" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_binary_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_binary_scanner)

[**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner) searches for a binary and outputs OSS information if there is an identical or similar binary from the Binary DB.   
It can analyze the open source info. in '.jar' file by using [**Dependency-check-py**](https://github.com/jhermann/dependency-check-py).   
   
**Github Repository** : [https://github.com/fosslight/fosslight_binary_scanner](https://github.com/fosslight/fosslight_binary_scanner)    
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_binary_scanner/blob/main/LICENSE)

## Contents
- [Prerequisite](#-prerequisite)
- [How to install](#-how-to-install)
- [How to run](#-how-to-run)
- [Result](#-result)
- [How it works](#-how-it-works)


## 📋 Prerequisite
FOSSLight Binary Scanner needs a Python 3.7+.    
To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).

[**Java**](https://openjdk.java.net/) Installation for jar file analysis. (Install Open Source JDK)     

## 🎉 How to install
### Method 1. Download the executable file.
Download the executable file suitable for the OS. : [https://github.com/fosslight/fosslight_binary_scanner/releases]()
### Method 2. Install fosslight_binary based on Python environment.
It can be installed using pip3. 
0. (Only for windows) Install Microsoft Build Tools from https://visualstudio.microsoft.com/en/vs/older-downloads/ > Redistributables packages and Build Tools.
1. [python 3.7 + virtualenv](etc/guide_virtualenv.md) environment setting.
2. Install the Python package fosslight_binary.
```
$ pip3 install fosslight_binary
```

## 🚀 How to run
### Method 1. If you run it as an executable on windows.
After placing the fosslight_bin_windows.exe file in the path to be analyzed binary, double-click to run it.
### Method 2. When executing with command.
````
$ fosslight_binary [option] <arg>
````    
### Options
```` 
    Options:
        -p <binary_path>                    Path to analyze binaries (Default: current directory)
        -h                                  Print help message
        -v                                  Print FOSSLight Binary Scanner version
        -s                                  Extract only the binary list in simple mode
        -o <output_path>                    Output path
                                            (If you want to generate the specific file name, add the output path with file name.)
        -f <format>                         Output file format (excel, csv, opossum, yaml)
        -d <db_url>                         DB Connection(format :'postgresql://username:password@host:port/database_name')
        --notice                            Print the open source license notice text.
        --no_correction                     Enter if you don't want to correct OSS information with sbom-info.yaml
        --correct_fpath <path>              Path to the sbom-info.yaml file
````    

## 📁 Result

```
$ tree
.
├── fosslight_log_220904_0912.txt
├── fosslight_report_220904_0912.xlsx
└── fosslight_opossum_220904_0912.json

```
- fosslight_log_[datetime].txt : The execution log.
- fosslight_report_[datetime].xlsx : FOSSLight binary result in FOSSLight Report format.    
   - If analyzing jar files, 'Vernerability Link' Column is added to FOSSLight-Report_[datetime].xlsx file.
   - The checksum and tlsh values for each binary are hidden by default and written within FOSSLight Report.   
- fosslight_opossum_[datetime].json : FOSSLight binary Scanner result for [OpossumUI](https://github.com/opossum-tool/OpossumUI)

## 🧐 How it works
1. List up binaries except the following cases.    
    1-0. Symbolic link files and FIFO files.    
    1-1. The file extension is ['png', 'gif', 'jpg', 'bmp', 'jpeg', 'qm', 'xlsx', 'pdf', 'ico', 'pptx', 'jfif', 'docx',
                                'doc', 'whl', 'xls', 'xlsm', 'ppt', 'mp4', 'pyc', 'plist']            
    1-2. The file type is ['data','timezone data', 'apple binary property list']    
    1-3. The directory is ['.git']    
2. Check “Exclude” in FOSSLight Report.         
    - binary is ['fosslight_bin', 'fosslight_bin.exe']     
    - directory is ["test", "tests", "doc", "docs"]     
    - directory is a hidden directory (directory name starts with .)
3. Extract checksum and tlsh for each binary.     
4. Load OSS information from Binary DB.      
5. Create output files.  
