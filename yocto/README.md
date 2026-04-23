---
sort: 4
published: true
title: 🚩FOSSLight Yocto Scanner

---
# FOSSLight Yocto Scanner

<img src="https://img.shields.io/pypi/l/fosslight_yocto" alt="FOSSLight Yocto is released under the Apache-2.0." /> <img src="https://img.shields.io/pypi/v/fosslight_yocto" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_yocto" /> <a href="https://github.com/fosslight/fosslight_yocto_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_yocto_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_yocto_scanner)

[**FOSSLight Yocto Scanner**](https://github.com/fosslight/fosslight_yocto_scanner) is a Python script that uses the results extracted via [bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass) during a Yocto Project-based build process to output OSS information for packages included in the rootfs image in FOSSLight Report format.

- **Output**
    - SRC Sheet : Extracts the installed package list and outputs OSS information.
    - BIN Sheet : Extracts binaries from the folder where the rootfs image was decompressed, then outputs OSS information per binary.

- **OSS Information Collection Criteria**
    - OSS information per package outputs OSS Name (Recipe name), OSS Version, LICENSE, and Download Location based on the metadata defined in the Recipe.

- **⚠️ Note**
    - For images mounted on the target other than the rootfs image (e.g., kernel, bootloader), the script does not output OSS information. Users must manually add OSS information to the FOSSLight Report for these.

<br><br>

## Prerequisite
{: .left-bar-title}
- [**FOSSLight Yocto Scanner**](https://github.com/fosslight/fosslight_yocto_scanner) runs on Python 3.10+.
- To use the feature that extracts OSS information (OSS Name, OSS Version, License) from Binary DB, refer to the [DB Setting Guide](../scanner/etc/binary_db.md).

<br><br>

## How to Install
{: .left-bar-title}
0. (Windows only) Install Microsoft Build Tools from https://visualstudio.microsoft.com/en/vs/older-downloads/ > Redistributables and Build Tools.
1. Set up a [python virtualenv](../scanner/etc/guide_virtualenv.md) environment.
2. Install the Python package fosslight_yocto.
    ```
    $ pip3 install fosslight_yocto
    ```
<br><br>

## How to Run
{: .left-bar-title}


### Preparation
{: .specific-title}
1. Move to the build directory (e.g., poky/build), then inherit buildhistory and bom in conf/local.conf.
    ```
    $ cd poky/build
    poky/build$ vi conf/local.conf
    INHERIT += "buildhistory"
    BUILDHISTORY_COMMIT = "1"
    
    INHERIT += "bom"
    ```
2. Download the [bom.bbclass](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/files_for_preparation/bom.bbclass) file to the meta/classes directory under the top-level directory.
    - If meta/classes does not exist, download bom.bbclass to the classes folder of the meta layer included in the build.
        ```
        poky/meta/classes$ wget -O bom.bbclass "https://github.com/fosslight/fosslight_yocto_scanner/raw/main/files_for_preparation/bom.bbclass"
        ```
    - For versions prior to Yocto 2.5, the --runall feature is not supported. To output bom.bbclass during the build, modify bom.bbclass as follows.
        ```
        addtask write_bom_info -> addtask write_bom_info before do_build
        ```
3. After building the image, run write_bom_info.
    - Yocto 2.5 or later
        ```
        poky/build $ bitbake <image>
        poky/build $ bitbake --runall=write_bom_info <image> (eg. bitbake --runall=write_bom_info  core-image-minimal)
        ```
    - Earlier than Yocto 2.5
        ```
        poky/build $ bitbake <image>
        ```
4. The bom.json file and buildhistory folder are created under ${TOPDIR}/.

### Run
{: .specific-title}
Run the fosslight_yocto command.
```
$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis]
```

- Options
    ```
     Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_yocto [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Yocto Scanner parses bom.json to extract open source
    information for packages installed on a Yocto-based model.

    📚 Guide: https://fosslight.org/fosslight-guide-en/scanner/7_yocto.html

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -h                     Show this help message
    -v                     Show version information
    -o <path>              Output directory path (default: current directory)
    -f <format>            Output file format (excel, csv, opossum)

    🔍 Scanner-Specific Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Path of buildhistory/package directory
    -b <file>              bom.json file path
    -i <file>              installed-package-names.txt file path
    -ip <file>             installed-packages.txt file path
    -y <file>              sbom-info.yaml or oss-pkg-info.yaml file path
    -a <path>              Path to analyze the binaries
    -n                     Print result in BIN(Yocto) format
    -s                     Analyze source code for New Open Source
    -c                     Analyze all the source code
    -e <path>              Top build output path with bom.json to compress
                           all the source code
    -pr                    Print all data of bom.json

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Basic scan with required inputs
    fosslight_yocto -p buildhistory/packages -b bom.json \
                    -i installed-package-names.txt -ip installed-packages.txt

    # Scan with sbom-info.yaml and output path
    fosslight_yocto -p buildhistory/packages -b bom.json \
                    -i installed-package-names.txt -ip installed-packages.txt \
                    -y sbom-info.yaml -o results/

    # Scan with binary analysis and source code analysis
    fosslight_yocto -p buildhistory/packages -b bom.json \
                    -i installed-package-names.txt -ip installed-packages.txt \
                    -a /path/to/binaries -s
    ```
<br><br>

## Result
{: .left-bar-title}
```
$ tree
.
├── fosslight_log_yocto_260413_1443.txt  
└── ffosslight_report_yocto_260413_1443.xlsx  

```
- fosslight_log_yocto_[datetime].txt : Execution log.  
- fosslight_report_yocto_[datetime].xlsx : FOSSLight Yocto result in FOSSLight Report format.  
   - The checksum and TLSH values per binary are hidden by default in the report.  

<br><br>

## Additional Features
{: .left-bar-title}

### -y Option : Load OSS Information per Recipe/Package from SBOM Info File
{: .specific-title}
- When the script is run with the -y option, OSS information defined in the Recipe is not used; instead, the FOSSLight Report is generated based on the OSS information contained in the SBOM file.
  However, even if Recipe/Package information exists in the SBOM file, it will not be included in the FOSSLight Report if the Recipe/Package is not actually installed.

    | OSS Information                          | Exclude                              | Comment                                |
    |------------------------------------------|--------------------------------------|----------------------------------------|
    | Information extracted from Recipe        | O                                    | Excluded by sbom-info.yaml          |
    | Information added from sbom-info.yaml | Value written in sbom-info.yaml   | Info loaded from sbom-info.yaml     |

#### Writing the SBOM Info File
{: .under-bar-title}
Prepare a YAML file to write the SBOM info file, choosing one of the two formats below.
1. Create a sbom-info.yaml file (YAML format) and enter the corresponding yocto_recipe or yocto_package for each OSS. ( e.g., [sbom-info.yaml](https://github.com/fosslight/fosslight_yocto_scanner/blob/main/test_files/sbom-info.yaml) )
    - Fields that cannot be written as list type: version, download location, homepage, exclude
    <br>

#### How to Run
{: .under-bar-title}
- Specify the SBOM file with the -y parameter when running the script.
    - For a single SBOM file:
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis] -y [sbom-info.yaml]
    ```
    - To load two or more SBOM files (separated by comma):
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -a [path_to_binary_analysis] -y [oss-pkg-info.yaml,sbom-info.yaml]
    ```
<br>

### -s, -c Options : Source Code Analysis
{: .specific-title}
Runs source code analysis.

#### How to Run
{: .under-bar-title}
- <span style="color:red">(Recommended)</span> Parameter -s : Analyzes only Recipes whose OSS is not stored in FOSSLight Hub.
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -s
    ```
- Parameter -c : Analyzes source code for all installed Recipes.
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -c
    ```

#### Result Files
{: .under-bar-title}
- source_analysis_report.xlsx : Source code analysis result file.
- scancode_result folder : Result files per Recipe.

<br>

### -e Option : Copy and Compress Source Code per Recipe
{: .specific-title}
- This feature compresses the source code per installed Recipe into [recipe_name].zip, saves it to the package_zips folder, and then re-compresses them into a single archive. Note that socket, FIFO, and binary files are excluded.

#### Preparation
{: .under-bar-title}
- Add the following item to the jsondata dictionary inside the do_write_bom_info() function of bom.bbclass, then generate the bom.json file.
    > jsondata["file_path"] = d.getVar("FILESPATH", True)

#### How to Run
{: .under-bar-title}
- Add the -e parameter.
    ```
    (.venv)$ fosslight_yocto -i [installed-package-names.txt] -b [bom.json] -p [buildhistory/packages] -e
    ```

#### Result Files
{: .under-bar-title}
- package_zips : Folder containing the source code per Recipe copied and archived in [recipe_name].zip format.
- packages.tar.gz : Compressed archive of the package_zips folder.
- oss_source_path.txt : File that outputs the paths where source code was collected, limited to Recipes with SRC_URI starting with file://.
- failed_to_compress_list.txt : File that outputs the list of Recipes for which compression failed in whole or in part.
