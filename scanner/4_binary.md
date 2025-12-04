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
FOSSLight Binary Scanner needs a Python 3.10+.    
To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).

[**Java 11+**](https://openjdk.java.net/) Installation for jar file analysis. (Install Open Source JDK)     

## 🎉 How to install
### Method 1. Download the executable file.
Download the executable file suitable for the OS. : [Releases](https://github.com/fosslight/fosslight_binary_scanner/releases)
### Method 2. Install fosslight_binary based on Python environment.
It can be installed using pip3. 
0. (Only for windows) Install Microsoft Build Tools from https://visualstudio.microsoft.com/en/vs/older-downloads/ > Redistributables packages and Build Tools.
1. [python 3.10 + virtualenv](etc/guide_virtualenv.md) environment setting.
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
        -e <path>                           Path to exclude from analysis (files and directories, pattern matching is available)
                                            * IMPORTANT: Always wrap patterns in quotes("") to avoid shell expansion.
                  				                Example) fosslight_bin -e "test/abc.py" "*.jar" "test/"
        -o <output_path>                    Output path
                                            (If you want to generate the specific file name, add the output path with file name.)        
        -f <format> [<format> ...]          Output file formats (excel, csv, opossum, yaml)
                                            Multiple formats can be specified separated by space. 
        -d <db_url>                         DB Connection(format :'postgresql://username:password@host:port/database_name')
        --notice                            Print the open source license notice text.
        --no_correction                     Enter if you don't want to correct OSS information with sbom-info.yaml
        --correct_fpath <path>              Path to the sbom-info.yaml file
````    
- Pattern Matching [Pattern matching guide](https://scancode-toolkit.readthedocs.io/en/stable/cli-reference/scan-options-pre.html?highlight=ignore#glob-pattern-matching) Guide for the -e Option
   - ⚠️ Make sure to use double quotes ("") when entering values.
      - Example) fosslight_binary -e "*.png" "tests/"
   - ⚠️ File names and extensions are **case-sensitive**, so please enter them exactly as intended.



## ⚙️ Environment Variables
You can control the behavior of FOSSLight Binary Scanner by setting the following environment variables.

### 1. FOSSLIGHT_SKIP_AUTO_INSTALL
Disables automatic download/installation of dependency-check in the execution environment (especially for deployed executables).
Set to '1' or 'true' to disable auto-install.
```
Usage:
   - Linux/macOS:
     export FOSSLIGHT_SKIP_AUTO_INSTALL=1
   - Windows (cmd):
     set FOSSLIGHT_SKIP_AUTO_INSTALL=1
   - Windows (PowerShell):
     $env:FOSSLIGHT_SKIP_AUTO_INSTALL='1'
```

### 2. DEPENDENCY_CHECK_HOME
Specifies the installation path of dependency-check.
- If already installed: path to the dependency-check directory (e.g., .../dependency-check)
- If newly installed: path is determined as base + 'dependency-check'
```
   $ export DEPENDENCY_CHECK_HOME=/path/to/dependency-check
```

### 3. DEPENDENCY_CHECK_VERSION
Stores the version of the currently installed dependency-check.    
Set after successful installation or version check:
- Automatically set by code (e.g., for version 12.1.7):    
  os.environ['DEPENDENCY_CHECK_VERSION'] = '12.1.7'
- Referenced in external scripts:
     - Linux/macOS:
       bash echo $DEPENDENCY_CHECK_VERSION
     - Windows (cmd):
       cmd echo %DEPENDENCY_CHECK_VERSION%
  
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
   - If analyzing jar files, 'Vulnerability Link' Column is added to FOSSLight-Report_[datetime].xlsx file.
   - The checksum and tlsh values for each binary are hidden by default.   
- fosslight_opossum_[datetime].json : FOSSLight binary Scanner result for [OpossumUI](https://github.com/opossum-tool/OpossumUI)

## 🧐 How it works
1. List up binaries except the following cases.    

    |Excepted case         | Description                                                                                                                       |    
   |------------------------|-----------------------------------------------------------------------------------------------------------------------------------|    
   |symbolic link, FIFO file| Unable to read as file open.   It causes the FOSSLight Binary Scanner to stop when checking for file type or binary.              |    
   |Non-Binary file extensions | 'qm', 'xlsx', 'pdf', 'pptx', 'jfif', 'docx', 'doc', 'whl', 'xls', 'xlsm', 'ppt', 'mp4', 'pyc', 'plist', 'dat', 'json', 'js' etc|    
   |Specific file type         | Files that begin with 'data', 'timezone data', and 'applebinary property list'                                                 |    
   |Specific path             | '.git'                                                                                                                          |

2. Check “Exclude” in FOSSLight Report.
     
   |Exclude case                                                          |Description                                         |
   |----------------------------------------------------------------------|-----------------------------------------------------|
   |Binary included in ['fosslight_bin', 'fosslight_bin.exe']             | -                                                  |
   |Path included in ["test", "test", "doc", "doc", "docs", "intermediates"] folder | Output only the results of the source code or binary included in the actual distribution  |
   |Hidden folder (folders that begin with '.')                           | -                                                   |               
   |Specific file extensions                                              | Not final build output (ex, '.class')               |

3. Extract checksum and tlsh for each binary.     
4. Load OSS information from Binary DB.      
5. Create output files.  
