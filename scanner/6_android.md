---
published: true
---
# FOSSLight Android Scanner

<img src="https://img.shields.io/pypi/l/fosslight_android" alt="FOSSLight Android is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_android_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_android_scanner)

[**FOSSLight Android Scanner**](https://github.com/fosslight/fosslight_android_scanner) List all the binaries loaded on the Android-based model to check which open source is used for each binary, and to check whether the notices are included in the OSS notice.

**Github Repository** : [https://github.com/fosslight/fosslight_android_scanner](https://github.com/fosslight/fosslight_android_scanner)    
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_android_scanner/blob/main/LICENSE)

## Contents
- [Prerequisite](#-prerequisite)
- [How to install](#-how-to-install)
- [How to run](#-how-to-run)
- [Result](#-result)
- [Add-ons](#-add-ons)


## 📋 Prerequisite
FOSSLight Android Scanner needs a Python 3.8+.    
To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).

[**Java**](https://openjdk.java.net/) Installation for jar file analysis. (Install Open Source JDK)     

## 🎉 How to install
It can be installed using pip3. 
1. [python 3.8 + virtualenv](etc/guide_virtualenv.md) environment setting.
2. Install the Python package fosslight_android.
```
$ pip3 install fosslight_android
```

## 🚀 How to run
#### Prerequisite
Android Build
Build in user mode to get the output (out directory) and build log (android.log). You must first run make clean to get the full build log.

```
(Android native source build 예)
$ source ./build/envsetup.sh
$ make clean
$ lunch aosp_hammerhead-user
$ make -j4 2>&1 | tee android.log
```

{::options parse_block_html="true" /}
<details>
<summary markdown="span">**In case of earlier than Android 7.0 based model**</summary>

However, for models based on Android 7.0 and earlier, build after placing the module-info.mk file in build/core/tasks/ first. (To create module-info.json file during build time)

```
$ wget https://raw.githubusercontent.com/aosp-mirror/platform_build/android-cts-7.0_r33/core/tasks/module-info.mk
$ mv ./module-info.mk ./build/core/tasks
```

</details>   

#### Run the fosslight_android

Run the FOSSLight Android. (At this time, the build output (/out directory) and build log file (android.log) must be present in the android source path.)

```
(venv)$ fosslight_android  -s [android source path] -a [build log file name]
```

- Options
    ```
        Mandatory
            -s <android_source_path>       Path to analyze
            -a <build_log_file_name>       The file must be located in the android source path.

        Optional
            -h                             Print help message
            -m                             Analyze the source code for the path where the license could not be found.
            -p                             Check files that should not be included in the Packaging file.
            -f                             Print result of Find Command for binary that can not find Source Code Path.
            -t                             Collect NOTICE for binaries that are not added to NOTICE.html.
            -d                             Divide needtoadd-notice.html by binary.
            -i                             Disable the function to automatically convert OSS names based on AOSP.
            -r <result.txt>                result.txt file with a list of binaries to remove.
    ``` 

## 📁 Result
- fosslight_report_[datetime].xlsx : This is a report outputting OSS information for each binary detected in the output.
    - The checksum and tlsh values for each binary are hidden by default and written within FOSSLight-Report.    
- fosslight_log_[datetime].txt : Execution log is output.      
- REMOVED_BIN_BY_DUPLICATION_[datetime].txt : This is a list deduplicated from FOSSLight Report because there are two or more files with the same binary name and checksum in the output path.

| Column           | Description                                                                                   |
|:-----------------|:----------------------------------------------------------------------------------------------|
| Binary Name      | Binary, library, APK, and font files in the root file system.                                |  
| Source Code Path | Path information of source code that creates the binary (LOCAL_PATH)                                                |  
| NOTICE.html      | Indicates whether binary information is displayed in NOTICE.html. If binary using open source, NOTICE.html value must be ok.                  |
| OSS Name         | Print the information of matching binary from Binary DB or Android repositories.    |
| OSS Version      | Print the information of matching binary from Binary DB.                                |
| License          | Print license from 1. Binary DB 2. "MODULE_LICENSE_xxxxxx" in Source Path 3. {MODULE_NAME}.meta_lic.|
| Need Check       | If O, review is required                                                                        |
| Comment          | Shows what needs to be reviewed.                                                                 |
| (TLSH)           | Print TLSH value of the binary.                                                            |
| (SHA1)           | Print Cheucksum value of the binary.                                                           |

## 🚗 Add-ons
---
Additional functions are available through the options.
- Option: -p : Check files that should not be included in the Packaging file.
- Option: -f : Print result of Find Command for binary that can not find Source Code Path.
- Option: -i : Turn off OSS Name auto-completion
- Option: -r : Deduplicate the binary.
- Option: -m : Extract the license by analyzing the source code.

---

### -p: Check files that should not be included in the Packaging file.
Check the file names, file extensions, and paths that should not be included when packaging source code to be disclosed.               

Prerequisite     
- Packaging Config File : Create a pkgConfig.json file as json format for checking files.

Example : pkgConfig.json

```
    {
       "Prohibited_File_Names":[
          "key_file",
          "confidential_key"
       ],
       "Prohibited_File_Extensions":[
          "exe",
          "jar"
       ],
       "Prohibited_Path":[
          "confidential",
          ".git"
       ]
    }
```

- Prohibited_File_Names : File name to be detected. 
- Prohibited_File_Extensions : File extension to be detected. 
- Prohibited_Path :File path to be detected.
- A path or compressed file containing the source code to be disclosed. 
  - If there is a compressed file in the path or compressed file that contains the source code to be released, decompress it and search it.
  - File extensions that support decompression : tar, tar.gz, zip

**How to run**
1. Place the Packaging config file (pkgConfig.json).
2. Run the fosslight_android with -p option. (-p : A path or compressed file containing the source code to be disclosed)
    ```
    (venv)$ fosslight_android -p [A path or compressed file containing the source code to be disclosed]
     
    ex
    (venv)$ fosslight_android -p /home/test/sourceCodeToBeDisclosed.tar.gz
    ```

3. Result 
The extracted list is displayed for each detected item.        
       
Example :       

```
    (venv)$ fosslight_android  -p /home/test/sourceCodeToBeDisclosed.tar.gz
    1. Prohibited file names : 1
    sourceCode/executable/LgeOscClient/confidential_key
    2. Prohibited file extension : 4
    sourceCode/executable/Report_Jenkins_ubuntu.exe
    sourceCode/executable/ReportTool_v3.03_181128U.jar
    sourceCode/executable/Protex_Create_Upload_Analyze_v3.03_181128U.jar
    sourceCode/executable/ReportTool_CLI_v3.03_181128U.jar
    3. Prohibited Path : 2
    sourceCode/.git
    sourceCode/executable/LgeOscClient/confidential
    4. Fail to read : 0
```
   
- Prohibited file names : If the file name of the source code to be disclosed contains the value of Prohibited_File_Names in pkgConfig.json.
- Prohibited file extension :  If the file extension of the source code to be disclosed contains the value of Prohibited_File_Extensions in pkgConfig.json.
- Prohibited Path :If the file path of the source code to be disclosed contains the value of Prohibited_Path in pkgConfig.json.
- Fail to read : Prints a list of files that failed to decompress.

### -f: Print result of Find Command for binary that can not find Source Code Path.
Print the result of executing Find Command for each folder (excluding out directory, hidden directories starting with '.') in Android's Source Path for Binary that can not find Source Code Path.     

1. Run the fosslight_android with -f.
    ```commandline
    (venv)$ fosslight_android  -s [android source path] -a [build log file name] -f
     
    ex
    (venv)$ fosslight_android  -s /home/soim/android/source -a android.log -f
    ```

2. Result           
The result of executing the Find command for each binary that can not find the Source Code Path is generated as a 'FIND_RESULT_OF_BINARIES.txt' file.
However, if there is no binary that can not find the source code path, 'FIND_RESULT_OF_BINARIES.txt' will not be created.

### -i: Turn off OSS Name auto-completion
If OSS information is not found in Binary DB or OSS Name is [Android Native](https://android.googlesource.com/platform), OSS Name is automatically printed if it is a repository in Android Reference based on Source Code Path.       
To turn off this automatic OSS name output, add the i option.     

### -r: Deduplicate the binary.
It is used only when Android native and vendor mounted on one model are created as separate outputs.
When running FOSSLight Android for vendor, use the -r option to deduplicate the binary in Android native.        
- Conditions to remove duplicates: Binary name is the same and checksum is the same OR Binary name is the same and TLSH value difference is less than 120
- Deduplicated binaries are output to REMOVED_BIN_BY_DUPLICATION.txt.

1. Run the fosslight_android with -r. 
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -r [android_native_result.txt]
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -r android_native_result.txt
    ```

2. Result         
Binaries duplicated with android_native_result.txt are removed from FOSSLight-Report.xlsx and output to REMOVED_BIN_BY_DUPLICATION.txt.


### -m: Extract the license by analyzing the source code.
Only for binary that could not extract license, license by source code is extracted using FOSSLight Source, and the result is output in license column of FOSSLight Report.        

1. Run the fosslight_android with -m.
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -m
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -m
    ```

2. Result            
The analysis results are printed in the license column of the FOSSLight Report.        
Additionally, analysis results by source code are created in the source_analyzed_[datetime] folder.      
