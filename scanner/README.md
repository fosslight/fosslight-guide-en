---
sort: 1
published: true
title: 📦FOSSLight Scanner (All-in-One)
---
# FOSSLight Scanner

<a href="https://github.com/fosslight/fosslight_scanner/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/fosslight_scanner" alt="FOSSLight Scanner is released under the Apache-2.0." /></a> <a href="https://pypi.org/project/fosslight-scanner/"><img src="https://img.shields.io/pypi/v/fosslight_scanner" alt="Current python package version." /></a> <img src="https://img.shields.io/pypi/pyversions/fosslight_scanner" /><a href="https://github.com/fosslight/fosslight_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a>

FOSSLight Scanner is an integrated scanning tool that automatically analyzes open source information contained in dependencies, source code, and binaries. In addition to analyzing sources downloadable via Git or wget, it can also analyze sources provided through a local path, and the results are generated in the FOSSLight Report format, which follows the SBOM standard.  
<br />
FOSSLight Scanner consists of the following 3 scanners, each responsible for different analysis areas.  

1. [FOSSLight Dependency Scanner](1_dependency.md)   
    A scanner that analyzes dependencies used through package managers or build systems to extract open source information.   
    It supports various package managers such as npm, pypi, maven, gradle, etc., and analyzes not only direct dependencies but also transitive dependencies that are included as a result.     
2. [FOSSLight Source Scanner](2_source.md)  
    Detects open source-related information such as license phrases, copyright strings, and code snippets by analyzing source code.   
3. [FOSSLight Binary Scanner](3_binary.md)  
    Performs open source analysis on binary files.   
    Instead of reverse engineering the binary itself, it collects binary lists and matches them with OSS information in the internal database to identify open source included in the binary.  

<br><br>

## Table of Contents
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [How to Run](#how-to-run)
  - [Results](#results)
  - [Installation and Execution using Docker](#installation-and-execution-using-docker)

<br><br>


## Prerequisites
{: .left-bar-title}

1. [**FOSSLight Scanner**](https://github.com/fosslight/fosslight_scanner) runs on Python 3.10 or higher (officially supported versions: 3.10~3.12) and can be installed via pip3 command.    
2. To analyze Jar files, [**Open Source JDK (Java)**](https://openjdk.java.net) must be installed.  
3. (For Windows) [Microsoft Build Tools (Microsoft Visual C++ 14.0+)](https://visualstudio.microsoft.com/visual-cpp-build-tools/) must be installed.  
<br><br>


## Installation
{: .left-bar-title} 
### Install from Bee (LGE Only)
{: .specific-title}
You can install and use FOSSLight Scanner from [Bee](https://docs.bee0.lge.com/docs/dev-tools/fosslight/).  

### Standard Installation Method
{: .specific-title}   
FOSSLight Scanner can be installed using pip3.      
It is recommended to install it in a [python3 virtualenv](etc/guide_virtualenv.md) environment.  

```
$ pip3 install fosslight_scanner
```

### Installation Error Cases
{: .specific-title}
If the error 'Cargo, the Rust package manager, is not installed or is not on PATH.' occurs, install cargo and rust as described below, then reinstall FOSSLight Scanner.
- Linux, macOS
  ```
  $ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  $ export PATH="$HOME/.cargo/bin:$PATH"
  ```
- Windows :
Download the rust-init.exe file from [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install) and install it.
<br><br> 

## How to Run
{: .left-bar-title} 
### How to run by mode & Parameters
{: .specific-title}
```
    📖 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight [mode] [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Scanner performs comprehensive open source analysis by running
    multiple modes (Source, Dependency, Binary) together. It can download
    source code from URLs (git/wget) or analyze local paths, and generates
    results in OSS Report format.

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/

    🔧 Modes
    ────────────────────────────────────────────────────────────────────
    all (default)          Run all modes (Source, Dependency, Binary)
    source                 Run FOSSLight Source analysis only
    dependency             Run FOSSLight Dependency analysis only
    binary                 Run FOSSLight Binary analysis only
    compare                Compare two FOSSLight reports

    Note: Multiple modes can be specified separated by comma
          Example: fosslight source,binary -p /path/to/analyze

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Path to analyze
                           • Compare mode: path to two FOSSLight reports (excel/yaml)
    -w <url>               URL to download and analyze (git clone or wget)
    -f <format>            Output format (excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml)
                           • Compare mode: excel, json, yaml, html
                           • Multiple formats: ex) -f excel yaml json (separated by space)
    -e <pattern>           Exclude paths from analysis (files and directories)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight -e "test/" "*.jar"
    -o <path>              Output directory or file name
    -c <number>            Number of processes for source analysis
    -r                     Keep raw data from scanners
    -t                     Hide progress bar
    -h                     Show this help message
    -v                     Show version information
    -s <path>              Apply settings from JSON file(check format with 'setting.json' in this repository)
                           Note: CLI flags override settings file
                           Example: -f yaml -s setting.json → output is .yaml
    --no_correction        Skip OSS information correction with sbom-info.yaml
                           (Correction only supports excel format)
    --correct_fpath <path> Path to sbom-info.yaml file for correction
    --ui                   Generate UI mode result file
    --recursive_dep        Recursively analyze dependencies

    🔍 Mode-Specific Options
    ────────────────────────────────────────────────────────────────────
    For 'all' or 'binary' mode:
      -u <db_url>          Database connection string
                           Format: postgresql://username:password@host:port/database

    For 'all' or 'dependency' mode:
      -d <args>            Additional arguments for dependency analysis

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Scan current directory with all scanners
    fosslight

    # Scan specific path with exclusions
    fosslight -p /path/to/source -e "test/" "node_modules/" "*.pyc"

    # Generate output in specific format
    fosslight -p /path/to/source -f yaml

    # Run specific modes only
    fosslight source,dependency -p /path/to/source

    # Download and analyze from git repository
    fosslight -w https://github.com/user/repo.git -o result_dir

    # Compare two FOSSLight reports
    fosslight compare -p report_v1.xlsx report_v2.xlsx -f excel

    # Run with database connection for binary analysis
    fosslight binary -p /path/to/binary -u "postgresql://user:pass@localhost:5432/sample"

```  
- Ex.1 How to analyze a local path  
```
fosslight -p /home/source_path
```

- Ex.2 How to download a link and analyze it    
```
fosslight -o test_result_wget -w "https://github.com/LGE-OSS/example.git"
```

- Ex.3 How to compare FOSSLight Report SBOM results to check changes/additions/deletions  
```
fosslight compare -p FOSSLight_before_proj.yaml FOSSLight_after_proj.yaml -o test_result
```

### How to call execution parameters as json
{: .specific-title}
1. Write and save execution parameter values as a JSON file in [setting.json](https://github.com/fosslight/fosslight_scanner/blob/main/tests/setting.json) format.
2. When executing, call the created setting.json with -s.
```
fosslight -s setting.json
```
🛈 The value called during execution takes precedence over the parameters written in the json file.      
ex. When called with '-f yaml -s setting.json', the output is in yaml format.
<br><br>

## Results  
{: .left-bar-title} 
### Open Source Analysis Results (all mode)  
{: .specific-title}
<pre>
test_result/
├── fosslight_log
│   └── fosslight_log_all_260204_0925.txt
├── fosslight_report_all_260204_0925.xlsx
└── fosslight_raw_data (with -r option)
    ├── fosslight_report_dep_260204_0925.xlsx
    ├── fosslight_report_src_260204_0925.xlsx
    └── fosslight_report_bin_260204_0925.xlsx
</pre>
- fosslight_report_all_(datetime).xlsx : FOSSLight Report format file containing Source analysis, Binary analysis, and Dependency analysis results
- fosslight_raw_data directory: Folder where analysis result Raw Data files are created (with -r option)
  - fosslight_report_dep_(datetime).xlsx : Dependency analysis result file
  - fosslight_report_src_(datetime).xlsx : Source analysis result file
  - fosslight_report_bin_(datetime).xlsx : Binary analysis result file
 
### fosslight_report_all_(datetime).xlsx  
{: .specific-title}

- **Scanner Info sheet**  
  This sheet displays information about the executed scanners and execution environment.  
  -  **Tool information** : Shows the name and version of the executed scanner.  
  -  **Start time** : Displays the start time of scanner execution.  
  -  **Python version** : Shows the Python version used to run the scanner.   
  -  **Analyzed path** : Displays the analyzed path (analysis path entered via '-p' option or default path where the scanner was executed)  
  -  **Excluded path** : Shows paths excluded during analysis (paths entered via '-e' option)  
  -  **Comment** : Displays analysis results for each scanner.   
      - **fosslight_dependency**
          - When package manager manifest file does not exist  
            <pre>
            Ex) [fosslight_dependency v4.1.31] No Package manager detected.
            </pre>
                
          - When package manager analysis succeeds  
            -[Success]  {package manager}:
              {project path}: {manifest file}
              <pre>
              Ex) [fosslight_dependency v4.1.31] Dependency Analysis Summary
                  - [Success]  pypi:
                  /home/worker/sample_code/example: requirements.txt
              </pre>  

          - When package manager analysis fails   
            -[Fail]  {package manager}:
              {project path}: {manifest file}
              If analysis fails, see fosslight_log*.txt and the prerequisite guide: https://fosslight.org/fosslight-guide-en/scanner/1_dependency.html#-prerequisite
              <pre>
                Ex) [fosslight_dependency v4.1.31] Dependency Analysis Summary
                    - [Fail]  yarn:
                    /home/worker/sample_code/example: package.json
                    If analysis fails, see fosslight_log*.txt and the prerequisite guide: https://fosslight.org/fosslight-guide-en/scanner/1_dependency.html#-prerequisite
              </pre>
      - **fosslight_source**  
          - Scanned files : Total number of analyzed files  
          - Detected source : Number of files where open source was detected.  
            - If no open source is detected, it is displayed as ‘Detected source: 0’.     
          - KB Enable/KB Unreachable : Indicates whether KB DB is enabled.    
          - Mode : Mode used for Source analysis.    
          
      - **fosslight_binary**  
          - Detected binaries: Number of binaries where open source was found.  
          - Scanned Files : Total number of analyzed files.  

- **DEP_FL_Dependency, SRC_FL_Source, BIN_FL_Binary sheet**   
  You can identify which scanner was executed from the sheet name and review the results for each scanner in that sheet.
  - Rows with Exclude column checked  
    - test(s), doc(s), hidden files or folders are checked as Exclude.    
    - When sbom-info.yaml is loaded, the loaded data is appended and analysis results for duplicate files are checked as Exclude.
  - Comment column  
    - Add/Loaded by ** : Row loaded from **  
    - Excluded by ** : Row excluded due to **         

### compare mode results
{: .specific-title}  
<pre>
test_result/
├── fosslight_log
│   └── fosslight_log_all_260205_1101.txt
└── fosslight_compare_260205_1101.xlsx
</pre>
- fosslight_compare_(datetime).xlsx : File containing comparison results of two SBOMs in add/delete/change table format  
<br><br>  

## Installation and Execution using Docker  
{: .left-bar-title} 
> ⚠️ In Docker environment, only FOSSLight Source/Binary Scanner runs, and FOSSLight Dependency Scanner is not supported.

1. Download FOSSLight Scanner Docker image
   
    Option 1. Download fosslight_scanner from Dockerhub 
    ```
    $ docker pull fosslight/fosslight_scanner
    ```
    Option 2. Build image using [Dockerfile](https://github.com/fosslight/fosslight_scanner/blob/main/Dockerfile) (If your OS is not supported in option 1)
    ```
    $ docker build -t fosslight_scanner .
    ```
    
2. Run with the built image.     
ex. Output path: /Users/git/temp/output, Analysis path: /Users/git/temp/dir_to_analyze
```
$ docker run -it -v /Users/git/temp/dir_to_analyze:/app/dir_to_analyze -v /Users/git/temp/output:/app/output fosslight_scanner -p dir_to_analyze -o output
```
