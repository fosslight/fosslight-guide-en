---
published: true
title: "  ㄴ FOSSLight Source Scanner"
---
# FOSSLight Source Scanner

<img src="https://img.shields.io/pypi/l/fosslight_source" alt="FOSSLight Source is released under the Apache-2.0 License." /> <img src="https://img.shields.io/pypi/v/fosslight_source" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_source" /><a href="https://github.com/fosslight/fosslight_source_scanner"><img src="https://img.shields.io/badge/GitHub-Repository-purple?logo=github" alt="GitHub Repository" /></a> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_source_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_source_scanner)

[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner) runs in [ScanCode][sc], [SCANOSS][scanoss], and KB (LGE Only) modes.  
  - [ScanCode][sc]: Detects copyright and license phrases included in files.  
  - [SCANOSS][scanoss]: Searches [OSSKB][osskb] for OSS Name, OSS Version, Download Location, Copyright, and License information.
  - KB (LGE Only): Queries file provenance from LG Electronics' internal Knowledge Database server and outputs OSS Name, OSS Version, and Download Location.

> Build scripts, binary files, directories, specific directories (for example, `test`), and files in hidden folders are excluded.

[sc]: https://github.com/nexB/scancode-toolkit
[scanoss]: https://github.com/scanoss/scanoss.py
[osskb]: https://osskb.org/


## Prerequisite
{: .left-bar-title}
[**FOSSLight Source Scanner**](https://github.com/fosslight/fosslight_source_scanner) runs on Python 3.10+.     
<br><br>

## How to Install
{: .left-bar-title}
FOSSLight Source Scanner can be installed with `pip3`.     
Installing in a [python 3.10 + virtualenv](etc/guide_virtualenv.md) environment is recommended.

```
$ pip3 install fosslight_source
```
<br><br>

## How to Run
{: .left-bar-title}

After scanning source code, results are output in FOSSLight Report format.
````
$ fosslight_source [option] <arg>
````
### Options
{: .specific-title}

```
📖 Usage
    ────────────────────────────────────────────────────────────────────
    fosslight_source [options] <arguments>

    📝 Description
    ────────────────────────────────────────────────────────────────────
    FOSSLight Source Scanner analyzes source code to detect copyright and
    license information using several modes.

    Note: Build scripts, binary files, and test directories are automatically
          excluded from analysis.

    📚 Guide: https://fosslight.org/fosslight-guide/scanner/2_source.html

    ⚙️  General Options
    ────────────────────────────────────────────────────────────────────
    -p <path>              Source path to analyze (default: current directory)
    -o <path>              Output file path or directory
    -f <format>            Output formats: excel, csv, opossum, yaml, spdx-yaml, spdx-json, spdx-xml, spdx-tag, cyclonedx-json, cyclonedx-xml
                           (multiple formats can be specified, separated by space)
    -e <pattern>           Exclude paths from analysis (files and directories)
                           ⚠️  IMPORTANT: Always wrap in quotes to avoid shell expansion
                           Example: fosslight_source -e "dev/" "tests/" "*.jar"
    -m                     Generate detailed scan results on separate sheets
    -h                     Show this help message
    -v                     Show version information

    🔍 Scanner-Specific Options
    ────────────────────────────────────────────────────────────────────
    -s <mode>              Choose mode: scancode, scanoss, kb, or all(default)
    -c <number>            Number of CPU cores/threads to use for scanning
    -t <seconds>           Timeout in seconds for ScanCode scanning
    -j                     Generate raw scanner results in JSON format
    --no_correction        Skip OSS information correction with sbom-info.yaml
    --correct_fpath <path> Path to custom sbom-info.yaml file

    💡 Examples
    ────────────────────────────────────────────────────────────────────
    # Scan current directory
    fosslight_source

    # Scan specific path with exclusions
    fosslight_source -p /path/to/source -e "test/" "node_modules/"

    # Generate output in specific format
    fosslight_source -f excel -o results/

    # Generate raw scanner results in JSON format
    fosslight_source -p /path/to/source -j
```
- If the `-s` option is not provided, results from all modes (ScanCode, SCANOSS, KB) are aggregated.
- [Pattern Matching Guide](https://scancode-toolkit.readthedocs.io/en/stable/reference/scancode-cli/cli-pre-scan-options.html#glob-pattern-matching) for the `-e` option
   - ⚠️ Always enter patterns using double quotes (`""`).
      - Example: `fosslight_source -e "dev/" "tests/"`
   - ⚠️ File names and extensions are case-sensitive and must match exactly.

### Example
{: .specific-title}
Source code scan
```
$ fosslight_source -p /home/source_path
```

## Result
{: .left-bar-title}
```
$ tree
.
├── fosslight_log_src_260311_1503.txt
└── fosslight_report_src_260311_1544.xlsx
```

  - fosslight_log_src_[datetime].txt: File that stores execution logs
  - fosslight_report_src_[datetime].xlsx: Source code analysis result in FOSSLight Report format
  - fosslight_opossum_src_[datetime].json: Source code analysis result that can be used in [OpossumUI](https://github.com/opossum-tool/OpossumUI) (`-f opossum` option)
  - fosslight_report_src_[datetime].csv: FOSSLight Report exported as CSV (`-f csv` option)
  - scancode_raw_result.json: ScanCode execution result (`-j` option)
  - scanoss_raw_result.json: SCANOSS execution result (`-j` option)
  - scanner_output.wfp: Fingerprint generated during SCANOSS execution (`-j` option)
