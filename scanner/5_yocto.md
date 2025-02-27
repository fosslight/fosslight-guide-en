---
published: true
---
# FOSSLight Yocto Scanner

<img src="https://img.shields.io/pypi/l/fosslight_yocto" alt="FOSSLight Yocto is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_yocto_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_yocto_scanner)

[**FOSSLight Yocto Scanner**](https://github.com/fosslight/fosslight_yocto_scanner) is a Python script that outputs OSS information about the package included in the rootfs image in OSS Report format when building based on Yocto Project.
- How to print OSS information: Prints the OSS information (OSS Name, OSS Version, LICENSE, Download location) defined in the recipe.
- ⚠️**For images (ex- kernel, boot loader) mounted on target other than the rootfs image, the script does not print.** Therefore, for this, the user must manually add OSS information to the FOSS Report.  
   
**Github Repository** : [https://github.com/fosslight/fosslight_yocto_scanner](https://github.com/fosslight/fosslight_yocto_scanner)    
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/LICENSE)

## Contents
- [Prerequisite](#-prerequisite)
- [How to install](#-how-to-install)
- [How to run](#-how-to-run)
- [Result](#-result)
- [How it works](#-how-it-works)


## 📋 Prerequisite
FOSSLight Yocto Scanner needs a Python 3.8+.    
To use the function to extract OSS information (OSS Name, OSS Version, License) from Binary DB, see the [database setting guide](etc/binary_db.md).

[**Java**](https://openjdk.java.net/) Installation for jar file analysis. (Install Open Source JDK)     

## 🎉 How to install
It can be installed using pip3. 
0. (Only for windows) Install Microsoft Build Tools from https://visualstudio.microsoft.com/en/vs/older-downloads/ > Redistributables packages and Build Tools.
1. [python 3.8 + virtualenv](etc/guide_virtualenv.md) environment setting.
2. Install the Python package fosslight_yocto.
```
$ pip3 install fosslight_yocto
```

## 🚀 How to run
### Method 1. How to run using bom.bbclass

---
Convert the results extracted with [bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass) to FOSS Report using FOSSLight Yocto.
- Output per sheet:
    - SRC Sheet : Extract installed package list and print OSS information.
    - BIN Sheet : fter extracting the binary from the folder where the rootfs image was extracted, print the OSS information for each binary.

---

#### Build with bom.bbcalss
1. After moving to the build directory (ex-poky / build), inherit buildhistory and bom in conf/local.conf.
    ```
    $ cd poky/build
    poky/build$ vi conf/local.conf
    INHERIT += "buildhistory"
    BUILDHISTORY_COMMIT = "1"
    
    INHERIT += "bom"
    ```
2. Copy a [bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass) file in meta/classes.
    - If meta/classes does not exist, download bom.bbclass to the classes folder of the meta layer included in the build.
        ```
        poky/meta/classes$ wget -O bom.bbclass "https://github.com/fosslight/fosslight_yocto_scanner/raw/main/files_for_preparation/bom.bbclass"
        ```
    - For versions prior to yocto 2.5, the --runall function is not supported, so bom.bbclass should be modified as follows.
        ```
        addtask write_bom_info -> addtask write_bom_info before do_build
        ```
3. After building the image, run write_bom_info task.
    - yocto 2.5 or Later 
        ```
        poky/build $ bitbake <image>
        poky/build $ bitbake --runall=write_bom_info <image> (eg. bitbake --runall=write_bom_info  core-image-minimal)
        ```
    - Earlier than yocto 2.5
        ```
        poky/build $ bitbake <image>
        ```
4. In the ${TOPDIR}/, bom.json file and buildhistory folder are created.

#### Run the fosslight_yocto
```
$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis]
```

- Options
    ```
    Mandatory
        -p <path>                      Path of buildhistory/package
        -b <file_with_path>            bom.json
        -i <file_with_path>            installed-package-names.txt
        -ip <file_with_path>           installed-packages.txt

    Optional
        -h                             Print help message
        -v                             Print FOSSLight yocto version
        -y <file_with_path>            oss-pkg-info.yaml
        -a <path>                      Path to analyze the binaries
        -n                             Print result in BIN(Android) format        
        -s                             Analyze source code for unconfirmed Open Source
        -c                             Analyze all the source code
        -e <path>                      Top build output path with bom.json to compress all the source code (ex. /data001/projectA/build)
        -o <path>                      Output Path
        -f <format>                    Output file format (excel, csv, opossum)
        -pr                            Print all data of bom.json
    ``` 
After placing the fosslight_bin_windows.exe file in the path to be analyzed binary, double-click to run it.

### Method 2. How to run using meta-doubleopen

---
When building based on Yocto Project, OSS information about the package included in the rootfs image is extracted as spdx.json using [meta-doubleopen](https://github.com/doubleopen-project/meta-doubleopen) and converted into OSS Report format using FOSSLight Yocto.
- Output per sheet:
    - SRC_distributed: Packages included in rootfs image.
    - SRC_recipe: Recipes included in build.
    - SRC_not_distributed: Packages not included in rootfs image.

- OSS information output method for each package: Prints OSS information (OSS Name, OSS Version, LICENSE, Download Location, Homepage) defined in recipe.

---

#### Build with meta-doubleopen
Create a spdx.json file for the image using[meta-doubleopen](https://github.com/doubleopen-project/meta-doubleopen)


#### Run the fosslight_doubleopen
```
$ source venv/bin/activate
(.venv) $  fosslight_doubleopen -f core-image-minimal.spdx.json
```

- Option f {[image].spdx.json} : spdx.json file generated as a result of executing meta-doubleopen

## 📁 Result

```
$ tree
.
├── fosslight_log_220904_0912.txt
├── fosslight_report_220904_0912.xlsx
└── fosslight_opossum_220904_0912.json

```
- fosslight_log_[datetime].txt : The execution log.
- fosslight_report_[datetime].xlsx : FOSSLight Yocto result in FOSSLight Report format.    
   - The Checksum and TLSH values for each binary are hidden by default and written within FOSSLight Report.    
- fosslight_opossum_[datetime].json : FOSSLight Yocto Scanner result for [OpossumUI](https://github.com/opossum-tool/OpossumUI)
