---
published: true
title: "  ㄴ FOSSLight Binary Scanner"
---
# FOSSLight Binary Scanner

<img src="https://img.shields.io/pypi/l/fosslight_binary" alt="FOSSLight Binary is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_binary" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_binary" /><a href="https://github.com/fosslight/fosslight_binary_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_binary_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_binary_scanner)

 [**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner) identifies binaries and outputs the results. When identical or similar binaries exist in the Binary DB, it provides OSS information (OSS Name, OSS Version, License). In addition, it provides security vulnerability information after JAR file analysis.  

## Prerequisite
{: .left-bar-title} 
- [**FOSSLight Binary Scanner**](https://github.com/fosslight/fosslight_binary_scanner) runs in the Python 3.10+ environment.  
- In environments where the Binary DB is configured, OSS information (OSS Name, OSS Version, License) can be extracted from that DB. If the Binary DB has not been pre-configured, you must set up the environment by referring to the [Binary DB setting guide](etc/binary_db.md).   
- For jar file analysis, you must install [**Java 11+**](https://openjdk.java.net) based on an Open Source JDK.    

<br><br> 

## How to install
{: .left-bar-title} 

### Method 1. Install using the executable file  
{: .specific-title}  
Download and use the FOSSLight Binary Scanner executable suitable for your OS. If your OS is not supported, install using 'Method 2'.    
    - [FOSSLight Binary Scanner - Release](https://github.com/fosslight/fosslight_binary_scanner/releases)  

> 🛠️ Troubleshooting based on error messages  
>  1. \[PYI-385915:ERROR\] Failed to load Python shared library '/tmp/_MEIpgjz34/libpython3.12.so.1.0': /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by /tmp/_MEIpgjz34/libpython3.12.so.1.0).  
>     If this error occurs, the executable is not supported for your OS or OS version. In this case, install using 'Method 2'.   

### Method 2. Install the fosslight_binary package based on the Python environment  
{: .specific-title} 
On Windows, to build the Python package, install Microsoft Build Tools from [Visual Studio official site](https://visualstudio.microsoft.com/en/vs/older-downloads/) > 'Other tools, frameworks and redistributable packages'.     
1. Set up the [python 3.10 + virtualenv](etc/guide_virtualenv.md) environment
2. Install the Python package `fosslight_binary`
```
$ pip3 install fosslight_binary  
```

<br><br> 

## How to run
{: .left-bar-title} 

### Method 1. Run as an executable on Windows  
{: .specific-title}  
1. Run by double-clicking the executable
   - Place the `fosslight_bin_windows.exe` file in the path where you want to analyze binaries, then double-click to run.     
2. Run the executable from the command prompt (cmd)
   - fosslight_bin_windows.exe -p D:\Download\Dir_to_analyze (Path to analyze)

### Method 2. For other cases, run via command (when installed as a Python package)  
{: .specific-title} 
````
$ fosslight_binary [option] <arguments>  
````    

### Options
````
   📖 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_binary [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Binary Scanner extracts binaries and retrieves open source
    and license information by comparing similarity with binaries stored
    in the Binary DB using TLSH (Trend Micro Locality Sensitive Hash).

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/3_binary.html

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Binary path to analyze (default: current directory)
    -o <path>              Output file path or directory
    -f <format>            Output formats: excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml
                           (multiple formats can be specified, separated by space)
    -e <pattern>           Exclude paths from analysis (files and directories)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight_binary -e "test/" "*.jar"
    -h                     Show this help message
    -v                     Show version information

    🔍 Scanner-Specific Options
    ────────────────────────────────────────────────────────────────────
    -d <db_url>            DB Connection (format: 'postgresql://user:pass@host:port/db')
    --notice               Print the open source license notice text
    --no_correction        Skip OSS information correction with sbom-info.yaml
    --correct_fpath <path> Path to custom sbom-info.yaml file

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Scan current directory
    fosslight_binary

    # Scan specific path with exclusions
    fosslight_binary -p /path/to/binaries -e "test/" "*.so"

    # Generate output in specific format
    fosslight_binary -f excel -o results/

    # Connect to Binary DB for OSS information
    fosslight_binary -d "postgresql://user:pass@localhost:5432/exampledb"
````
- -e option: [Pattern matching guide](https://scancode-toolkit.readthedocs.io/en/stable/reference/scancode-cli/cli-pre-scan-options.html#glob-pattern-matching)
   - ⚠️ When using it, please enter values using double quotes ("").
       - Example) fosslight_binary -e "*.png" "tests/"
   - ⚠️ File names and extensions are case-sensitive, so please enter them exactly as intended.

<br><br> 

## Result
{: .left-bar-title}

```
$ tree
.
├── fosslight_log_bin_260401_1156.txt
└── fosslight_report_bin_260401_1156.xlsx

```
- fosslight_log_bin_[datetime].txt : Execution log
- fosslight_report_bin_[datetime].xlsx : Binary analysis result in the FOSSLight Report format        
   - For jar file analysis, security vulnerability information is added to the `Vulnerability Link` column.  
   - For each binary, the checksum and `tlsh` columns are hidden by default.     
