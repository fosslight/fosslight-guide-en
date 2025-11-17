---
sort: 5
published: true
title: 🚩FOSSLight Scanner
---
# FOSSLight Scanner

<a href="https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/fosslight_scanner" alt="FOSSLight Scanner is released under the Apache-2.0." /></a> <a href="https://pypi.org/project/fosslight-scanner/"><img src="https://img.shields.io/pypi/v/fosslight_scanner" alt="Current python package version." /></a> <img src="https://img.shields.io/pypi/pyversions/fosslight_scanner" />

FOSSLight Scanner can perform an analysis for open source compliance at once. It can perform open source analysis of source code, binary and dependency. <br/>

It works using the following scanners:

1. [FOSSLight Source Scanner](2_source.md) : Analyze the source code and generate open source analysis result.
2. [FOSSLight Dependency Scanner](3_dependency.md) : Analyze the dependencies used through package manager or build system and generate open source analysis result. 
3. [FOSSLight Binary Scanner](4_binary.md) : Analyze the binaries and generate open source analysis result.
<br />

**Github Repository** : [https://github.com/fosslight/fosslight_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE)


## 📋 Prerequisite
1. FOSSLight Scanner needs a Python 3.10+.    
2. To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).
3. (Only for windows) Install Microsoft Build Tools (Microsoft Visual C++ 14.0+) from https://visualstudio.microsoft.com/ko/visual-cpp-build-tools/

## 🎉 How to install
It can be installed using pip3. It is recommended to install it in the [python virtualenv](etc/guide_virtualenv.md) environment.
```
$ pip3 install fosslight_scanner
```

### ⚠️ Installation error case
If the error "'Cargo, the Rust package manager, is not installed or is not on PATH.' occurs, try to install cargo and rust as described below, and reinstall FOSSLight Scanner.
#### Linux, MacOS
```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$ export PATH="$HOME/.cargo/bin:$PATH"
```
#### Windows
Download the rust-init.exe file from [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install) and install it.
 

## 🚀 How to run
### How to run by mode & Parameters
```
$ fosslight [Mode] [option1] <arg1> [option2] <arg2>...
```
```
     Parameters:
        Mode: Multiple modes can be entered by separating them with , (ex. source,binary)
            all                     Run all scanners(Default)
            source                  Run FOSSLight Source Scanner
            dependency              Run FOSSLight Dependency Scanner
            binary                  Run FOSSLight Binary Scanner
            compare                 Compare two FOSSLight reports

        Options:
            -h                      Print help message
            -p <path>               Path to analyze (ex, -p {input_path})
                                     * Compare mode input file: Two FOSSLight reports (supports excel, yaml)
                                       (ex, -p {before_name}.xlsx {after_name}.xlsx)
            -w <link>               Link to be analyzed can be downloaded by wget or git clone            
            -f <formats> [<format> ...]     FOSSLight Report file format (excel, csv, opossum, yaml, spdx-tag, spdx-yaml, spdx-json, spdx-xml)
                                     * Compare mode result file: supports excel, json, yaml, html
                                     * Multiple formats can be specified separated by space.
            -e <path>               Path to exclude from analysis (ex, -e {dir} {file})
            -o <output>             Output directory or file
            -c <number>             Number of processes to analyze source
            -r                      Keep raw data
            -t                      Hide the progress bar
            -v                      Print FOSSLight Scanner version
            -s <path>               Path to apply setting from file (check format with 'setting.json' in this repository)
                                     * Direct cli flags have higher priority than setting file
                                       (ex, '-f yaml -s setting.json' - result file extension is .yaml)
            --no_correction         Enter if you don't want to correct OSS information with sbom-info.yaml
                                     * Correction mode only supported xlsx format.
            --correct_fpath <path>  Path to the sbom-info.yaml file
            --ui                          Generate UI mode result file

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

### How to call execution parameters as json
1. Write and save the values ​​for each execution parameter as a json file in [setting.json](https://github.com/fosslight/fosslight_scanner/blob/main/tests/setting.json) format.
2. When running, call setting.json with -s.
```
fosslight -s setting.json
```
🛈 The value called during execution takes precedence over the parameters written in the json file.
ex. When called with '-f yaml -s setting.json', the output file is in yaml format.


## 📁 Result
### Result for the mode that analyze the open source (all, source, dependency, binary)
```
test_result/
├── fosslight_log
│   └── fosslight_log_220214_1824.txt
├── fosslight_report_all_220214_1824.xlsx
└── fosslight_raw_data (with -r option)
    ├── fosslight_src_220214_1824.xlsx
    ├── fosslight_bin_220214_1824.xlsx
    └── fosslight_dep_220214_1824.xlsx
```
- fosslight_report_all_(datetime).xlsx : FOSSLight Report format file in which source code analysis, binary analysis, and dependency analysis results are written.
- fosslight_raw_data directory: The folder where the analysis result raw data file is created (with -r option)
  - fosslight_src_(datetime).xlsx : FOSSLight Report of FOSSLight Source Scanner
  - fosslight_dep_(datetime).xlsx : FOSSLight Report of FOSSLight Dependency Scanner
  - fosslight_bin_(datetime).xlsx : FOSSLight Report of FOSSLight Binary Scanner

#### fosslight_report_(datetime).xlsx 
##### Scanner Info sheet
This sheet displays information about the executed scanner and its execution environment.
1. Tool Information: Shows the name and version of the executed scanner.
2. Start Time: Displays the start time of the scanner execution.
3. Python Version: Indicates the Python version used to run the scanner.
4. Analyzed Path: Displays the analyzed path (either the path entered using the -p option or the default path where the scanner was executed).
5. Excluded Path: Shows the paths excluded during analysis (entered using the -e option).
6. Comment: Displays the analysis results for each scanner.
    - fosslight_source
       - Display the total number of analyzed files (Total number of files) and the number of files removed from the analysis results because they belong to excluded paths (Removed files).
       - If no files are detected in the analysis path: add (No file detected.)
       - If files exist in the analysis path but no open source analysis results are detected: add (No OSS detected.)
    - fosslight_binary
        - Display the total number of analyzed binaries and the total number of analyzed files.
        - If no binaries are detected in the analysis path: add (No binary detected.)
    - fosslight_dependency:
       - If no package manager manifest file is detected: No Package manager detected.
       - If package manager analysis is successful: Success to anlalyze: -{package manager}: {path of manifest file}: {manifest file}
           - Ex)
             ```
             [fosslight_dependency v4.1.23] Success to analyze:
               -npm:
                /home/worker/local/codebase: package.json
               -nuget:
                /home/worker/local/codebase: packages.config
             ```
       - If package manager analysis fails: Analysis failed Package manager: Fail to analyze: -{package manager}: {path of manifest file}: {manifest file} / Check {prerequisite 가이드 url}
          - Ex)
            ```
            [fosslight_dependency v4.1.23] Fail to analyze:
              -gradle:
                /home/worker/local/codebase: build.gradle
              -android:
                /home/worker/local/codebase: build.gradle / Check log file(fosslight_log*.txt) and https://fosslight.org/fosslight-guide-en/scanner/3_dependency.html#-prerequisite.
            ```

##### {SRC/BIN/DEP}\_FL\_{Source/Binary/Dependency} sheet
You can check which scanner was executed from the sheet name and review the results for each scanner within that sheet.
1. Exclude: Checked Row
    test(s), doc(s), hidden files or folders are checked as Exclude.
2. When sbom-info.yaml is loaded, the loaded data is appended and the analysis results for duplicate files checked as Exclude.
3. Comment :      
   Add/Loaded by ** : Row loaded from **      
   Excluded by ** : Row excluded due to **            
   
### Result for compare mode
```
test_result/
├── fosslight_log
│   └── fosslight_log_20220817_114259.txt
└── fosslight_compare_20220817_114259.xlsx
```
- fosslight_compare_(datetime).xlsx : Two BOM comparison results in the form of (add/delete/change) table.

## 🐳 How to install and run using Docker
> [!NOTE]  
> When running with Docker, only the FOSSLight Source/Binary Scanner works. The FOSSLight Dependency Scanner does not work.

1. Download the FOSSLight Scanner Docker image
   
    Option 1. Download fosslight scanner from DockerHub.
    ```
    $ docker pull fosslight/fosslight_scanner
    ```
    Option 2. Build image using [Dockerfile](https://github.com/fosslight/fosslight_scanner/blob/main/Dockerfile) (If your OS is not supported in option 1)
    ```
    $ docker build -t fosslight_scanner .
    ```

2. Run with the image you built.      
ex. Output: /Users/git/temp/output, Path to be analyzed: /Users/git/temp/dir_to_analyze
```
$ docker run -it -v /Users/git/temp/dir_to_analyze:/app/dir_to_analyze -v /Users/git/temp/output:/app/output fosslight_scanner -p dir_to_analyze -o output
```
