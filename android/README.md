---
sort: 3
published: true
title: 🚩FOSSLight Android Scanner
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
FOSSLight Android Scanner needs a Python 3.10+.    
To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).

[**Java**](https://openjdk.java.net/) Installation for jar file analysis. (Install Open Source JDK)     

## 🎉 How to install
It can be installed using pip3. 
1. [python 3.10 + virtualenv](etc/guide_virtualenv.md) environment setting.
2. Install the Python package fosslight_android.
```
$ pip3 install fosslight_android
```

## 🚀 How to run
#### Prerequisite
Android Build
Build in user mode to get the output (out directory) and build log (android.log). You must first run make clean to get the full build log.

```
(Android native source build example)
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
            -e <path1> <path2..>           Path to exclude from source analysis.
            -p                             Check files that should not be included in the Packaging file.
            -f                             Print result of Find Command for binary that can not find Source Code Path.
            -i                             Disable the function to automatically convert OSS names based on AOSP.
            -r <result.txt>                result.txt file with a list of binaries to remove.
    ```
    

## 📁 Result
- fosslight_report_[datetime].xlsx : This is a report outputting OSS information for each binary detected in the output.
    - The Checksum and TLSH values for each binary are hidden by default and written within FOSSLight Report.    
- fosslight_log_[datetime].txt : Execution log is output.
- notice file
   - notice_to_fosslight_hub_{datetime}.{extension} : If there is only one notice file, move it directly to the output folder.
   - notice_to_fosslight_hub_{datetime}.zip : If there are multiple notice files, compress them into a ZIP file and move it to the output folder.
- REMOVED_BIN_BY_DUPLICATION_[datetime].txt : This is a list deduplicated from FOSSLight Report because there are two or more files with the same binary name and checksum in the output path.


| Column           | Description                                                                                            |
|:-----------------|:----------------------------------------------------------------------------------------------|
| Binary Name      | Binary, library, APK, and font files in the root file system.                                |  
| Source Code Path | Path information of source code that creates the binary (LOCAL_PATH)                                                |  
| Notice           | Displays whether Binary information is shown in the NOTICE file. If it is a Binary where Open Source has been used, it should be "ok."<br>&ensp;&ensp;- ok: The NOTICE file exists in the Source Path, and the final output NOTICE (e.g., NOTICE.html) includes the Binary. <br>&ensp;&ensp;- ok(NA): The NOTICE file does not exist in the Source Path, but the final output NOTICE (e.g., NOTICE.html) includes the Binary. <br>&ensp;&ensp;- nok: The NOTICE file does not exist in the Source Path, and the final output NOTICE (e.g., NOTICE.html) does not include the Binary.<br>&ensp;&ensp;- nok(NA): Even though the NOTICE file exists in the Source Path, the final output NOTICE (e.g., NOTICE.html) does not include the Binary.<br>&ensp;&ensp;- CANNOT_FIND_NOTICE_HTML: The NOTICE.html file cannot be found. (In this case, when running the script, you must provide the NOTICE.html file path as a parameter using -n [NOTICE.html_path].) |
| OSS Name         | Retrieves and displays the information of the Binary matched from the LGE Binary DB.     |
| OSS Version      | Retrieves and displays the information of the Binary matched from the LGE Binary DB.                               |
| License          | Displays the Open Source License extracted from the following information:<br>&ensp;&ensp;- Information about the Binary matched from the LGE Binary DB."<br>&ensp;&ensp;- Reads and displays licenses specified in files like "MODULE_LICENSE_xxxxxx" within the Source Code Path."<br>&ensp;&ensp;- Information found in {MODULE_NAME}.meta_lic from the output. |
| Need Check       | If O, review is required                                                                           |
| Comment          | Outputs items that require review:<br>&ensp;&ensp;- Fill in [Column Name]: Displays columns that need to be filled in.<br>Example: Fill in OSS Name: The 'OSS Name' column requires the name of the OSS used.<br>&ensp;&ensp;- Add NOTICE to path: Since there is no NOTICE file in the Source Code Path, a NOTICE file must be added to the Source Code Path of the corresponding binary.<br>However, if it is difficult to add a NOTICE file to the Source Code Path or if adding the NOTICE file does not include it in the final NOTICE for the target, the project should be reviewed via FOSSLight Hub. Afterward, download the NOTICE that needs to be added using the Supplement NOTICE.html feature, and supplement it using the method under Android Model OSS Notice > "Add separately created NOTICE to OSS Notice."|
| (TLSH)           | Print TLSH value of the binary.                                                            |
| (SHA1)           | Print Cheucksum value of the binary.    |



## 🚗 Add-ons
---
The following options enable the use of additional features.
- Option: -p : Checks for files that should not be included in the packaging file.
- Option: -f : Outputs the results of the find command for binaries whose Source Code Path could not be located.
- Option: -i : Disables the automatic output of OSS Name based on the Android reference repository.
- Option: -r : Removes duplicates of specific binaries in the FOSSLight Report. This option is used in scenarios where Android native and vendor are built separately, to eliminate duplicated binaries. When running FOSSLight Android for the vendor, use the -r option and provide the result_*.txt file generated from the Android native as a parameter.
- Option: -m : Automatically analyzes Source Code within the Source Path to detect Licenses (based on License texts found in source files) and populates empty License fields. (Note: This process can be time-consuming. For Android native with 44 Paths, the analysis takes approximately 35 minutes.)

---

<br>
<br>






### -p: Check files that should not be included in the Packaging file.
Check the file names, file extensions, and paths that should not be included when packaging source code to be disclosed.               

#### Prerequisite     
1. Packaging Config File : Create a pkgConfig.json file as json format for checking files.

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

2. Itemized Description: If there is more than one item to write for each category, separate them with a comma (',').
    - Prohibited_File_Names : File name to be detected. 
    - Prohibited_File_Extensions : File extension to be detected. 
    - Prohibited_Path :File path to be detected.
3. A path or compressed file containing the source code to be disclosed. 
    - If there is a compressed file in the path or compressed file that contains the source code to be released, decompress it and search it.
    - File extensions that support decompression : tar, tar.gz, zip

#### How to run
1. Place the Packaging config file (pkgConfig.json).
2. Run the fosslight_android with -p option. (-p : A path or compressed file containing the source code to be disclosed)
    ```
    (venv)$ fosslight_android -p [A path or compressed file containing the source code to be disclosed]
     
    ex
    (venv)$ fosslight_android -p /home/test/sourceCodeToBeDisclosed.tar.gz
    ```

#### Result 
1. The extracted list is displayed for each detected item.               
2. Example :       

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
<br>
<br>



### -f: Print result of Find Command for binary that can not find Source Code Path.
Print the result of executing Find Command for each folder (excluding out directory, hidden directories starting with '.') in Android's Source Path for Binary that can not find Source Code Path.     

#### How to run
1. Run the fosslight_android with -f.
    ```commandline
    (venv)$ fosslight_android  -s [android source path] -a [build log file name] -f
     
    ex
    (venv)$ fosslight_android  -s /home/soim/android/source -a android.log -f
    ```

#### Result           
1. The result of executing the Find command for each binary that can not find the Source Code Path is generated as a 'FIND_RESULT_OF_BINARIES.txt' file.
2. However, if there is no binary that can not find the source code path, 'FIND_RESULT_OF_BINARIES.txt' will not be created.
<br>
<br>


### -i: Turn off OSS Name auto-completion
If OSS information is not found in Binary DB or OSS Name is [Android Native](https://android.googlesource.com/platform), OSS Name is automatically printed if it is a repository in Android Reference based on Source Code Path.       
To turn off this automatic OSS name output, add the i option.     


#### How to run
1. Run the command with the -i option:
    ```commandline
    (venv)$ fosslight_android -s [android source path] -a [build log file name] -i
 
    ex
    (venv)$ fosslight_android -s /home/soim/android/source -a android.log -i
    ```

#### Review the results        
1. Generated File Name: RESULT_COMPARE_I_OPTION.xlsx
2. Contents of Result File:
   - Sheet "Ref_with_i_option": Analysis results from FOSSLight Android with the -i option enabled, using the Android reference source.
   - Sheet "Ref_without_i_option": Analysis results from FOSSLight Android without the -i option, using the Android reference source.

<br>
<br>



### -r: Deduplicate the binary.
It is used only when Android native and vendor mounted on one model are created as separate outputs.
When running FOSSLight Android for vendor, use the -r option to deduplicate the binary in Android native.        
    - Conditions to remove duplicates: Binary name is the same and checksum is the same OR Binary name is the same and TLSH value difference is less than 120
    - Deduplicated binaries are output to REMOVED_BIN_BY_DUPLICATION.txt.


#### How to run
1. Run the fosslight_android with -r. 
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -r [android_native_result.txt]
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -r android_native_result.txt
    ```

#### Result         
1. Binaries duplicated with android_native_result.txt are removed from FOSSLight-Report.xlsx and output to REMOVED_BIN_BY_DUPLICATION.txt.
<br>
<br>

### -m: Extract the license by analyzing the source code.
Only for binary that could not extract license, license by source code is extracted using FOSSLight Source, and the result is output in license column of FOSSLight Report.        


#### How to run
1. Run the fosslight_android with -m.
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -m
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -m
    ```

#### Result            
1. The analysis results are printed in the license column of the FOSSLight Report.        
2. Additionally, analysis results by source code are created in the source_analyzed_[datetime] folder.      
