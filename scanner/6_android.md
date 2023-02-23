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
FOSSLight Android Scanner needs a Python 3.7+.    
To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).

[**Java**](https://openjdk.java.net/) Installation for jar file analysis. (Install Open Source JDK)     

## 🎉 How to install
It can be installed using pip3. 
1. [python 3.7 + virtualenv](etc/guide_virtualenv.md) environment setting.
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
- fosslight_binary_android_[datetime].txt : fosslight_binary.txt is a file that outputs the OSS-Report file in text format and additionally outputs TLSH and checksum values for each binary.    
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

## 🚗 Add-ons
---
하기 옵션을 통해 부가 기능을 활용할 수 있습니다.
- Option: -p : Packaging 파일에 포함되지 않아야 하는 파일 확인
- Option: -f : Source Code Path를 찾지 못하는 binary에 대하여 Find Command 실행 결과 출력
- Option: -i : Android reference 의 repository기준으로 OSS Name 자동 출력 끄기
- Option: -r : 특정 binary를 FOSSLight Report에서 중복 제거. Android native와 vendor가 분리되어 build되는 구조에서 사용하는 옵션으로 중복으로 포함되는 Binary를 제거합니다. vendor에 대한 FOSSLight Android 실행시 -r 옵션으로 android native 결과 생성되는 result_*.txt 파일을 parameter로 추가합니다.
- Option: -m : License가 빈칸인 부분에 대해 자동으로 Source path 내 Source code 분석(소스 파일 내 License text 기반 License 검출)을 실행하여 License 값을 채워줍니다. (그러나 분석에 시간이 오래 걸립니다. Android native에서 44개 Path기준 약 35분 소요)

---

### -p: Packaging 파일에 포함되지 않아야 하는 파일 확인
공개할 Source Code 취합시, 포함되지 말아야 하는 파일 이름, 확장자, 디렉토리를 체크합니다.      

사전 준비      
- Packaging Config File : 체크할 항목을 json 형식의 pkgConfig.json 파일 이름으로 생성합니다.

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

- Prohibited_File_Names : 검출하려는 파일 이름 
- Prohibited_File_Extensions : 검출하려는 파일 확장자 
- Prohibited_Path : 검출할 파일 디렉토리
- 공개할 소스 코드를 취합한 디렉토리 위치 혹은 압축 파일 확인 
  - 공개할 소스 코드 취합한 디렉토리나 압축 파일 내 압축된 파일이 있을 경우, 압축을 해제하여 검색합니다. 
  - 압축 해제 지원 확장자 : tar, tar.gz, zip

**실행 방법**
1. Packaging Config File을 pkgConfig.json 파일명(json 형식)으로 준비합니다.
2. -p 옵션을 추가하여 실행합니다. (-p : 공개할 소스 코드를 취합한 Path 혹은 압축 파일)
    ```
    (venv)$ fosslight_android -p [A path or compressed file containing the source code to be disclosed]
     
    ex
    (venv)$ fosslight_android -p /home/test/sourceCodeToBeDisclosed.tar.gz
    ```

3. 결과 확인 
검출된 항목별로 추출된 목록을 보여줍니다.        
       
결과 example :       

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
   
- Prohibited file names : 공개할 소스 코드 중 파일명에 pkgConfig.json의 Prohibited_File_Names 값을 포함하는 경우 출력합니다.
- Prohibited file extension : 공개할 소스 코드 중 파일 확장자가 pkgConfig.json의 Prohibited_File_Extensions 값인 경우 출력합니다.
- Prohibited Path : 공개할 소스 코드 중 파일 Path 중 pkgConfig.json의 Prohibited_Path 값을 포함하는 경우 출력합니다.
   - Fail to read : 압축 해제에 실패한 파일 목록을 출력합니다.

### -f: Source Code Path를 찾지 못하는 binary에 대하여 Find Command 실행 결과 출력
Source Code Path를 찾지 못하는 Binary에 대하여 Android의 Source Path내 폴더 (out directory, .으로 시작하는 숨김 directory 제외)별로 Find Command 실행 결과를 출력합니다.     

1. -f 옵션을 추가하여 실행합니다.
    ```commandline
    (venv)$ fosslight_android  -s [android source path] -a [build log file name] -f
     
    ex
    (venv)$ fosslight_android  -s /home/soim/android/source -a android.log -f
    ```

2. 결과 확인        
Source Code Path를 찾지 못하는 Binary별 Find command 실행 결과는 'FIND_RESULT_OF_BINARIES.txt' 파일로 생성됩니다.
단, Source Code Path를 찾지 못하는 Binary가 없을 경우 해당 파일은 생성되지 않습니다.

### -i: OSS Name 자동 완성 기능 끄기
FOSSLight Android는 Binary DB에서 OSS 정보를 찾을 수 없는 경우이거나 OSS Name이 "Android Open Source Project"인 경우, Source Code Path를 기준으로 [Android Native](https://android.googlesource.com/platform)에 있는 저장소라면 OSS Name을 자동으로 출력해줍니다.      
OSS Name 자동 완성 기능을 끄고자 할 경우 선택합니다.      

### -r: 특정 binary를 FOSSLight Report에서 중복 제거
하나의 Model에 탑재하는 Android native와 vendor가 분리된 output으로 생성되는 경우에 한하여 활용합니다.        
- vendor에 대한 FOSSLight Android 실행시 -r 옵션을 이용하여 Android native에도 포함되는 binary를 중복 제거합니다.
- 중복 제거 조건 : Binary name이 같고 checksum이 같거나, Binary name이 같고 TLSH 값 차이가 120이하인 경우
- 중복 제거된 binary는 REMOVED_BIN_BY_DUPLICATION.txt에 출력됩니다.

1. -r 옵션을 추가하여 실행합니다. 
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -r [android_native_result.txt]
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -r android_native_result.txt
    ```

2. 결과 확인         
android_native_result.txt와 중복된 binary는 FOSSLight-Report.xlsx에서 제거되고, REMOVED_BIN_BY_DUPLICATION.txt에 출력됩니다.


### -m: 소스 코드 분석하여 License 출력
License 정보를 못 찾은 경우에 한하여 FOSSLight Source를 이용하여 Source code를 분석한 결과를 License란에 출력합니다.        

1. -m 옵션을 추가합니다.
    ```commandline
    (venv)$ fosslight_android -s [vendor_source_path] -a [android_build_log_file] -m
     
    ex
    (venv)$ fosslight_android -s [vendor_source_path] -a android.log -m
    ```

2. 결과 확인         
FOSSLight Report의 License column에 분석한 결과가 채워집니다.              
추가로 source_analyzed_[datetime] 폴더에 소스 코드별 분석한 결과가 생성됩니다.       