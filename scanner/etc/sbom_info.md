---
published: true
---

# How to Correct/Exclude Scan Results (sbom-info.yaml)

If you need to correct scan results or exclude specific files/directories from the output, you can create and use a `sbom-info.yaml` file.

> ℹ️ **Supported Scanners:** FOSSLight Scanner, FOSSLight Source Scanner, FOSSLight Binary Scanner
> (Not applicable to FOSSLight Dependency Scanner.)

## How It Works
{: .left-bar-title}
For each item in the scan results, the scanner checks whether the same `source name or path` exists in `sbom-info.yaml`, based on the **source path**.  
If a matching entry is found, the **OSS information written in `sbom-info.yaml` (name, version, license, etc.) takes priority** over the scan result.

## How to Use
{: .left-bar-title}
1. Create a `sbom-info.yaml` file in the top-level directory of the project to be scanned.
2. Fill in the OSS information you want to correct or the files you want to exclude, referring to the format and field descriptions below.
3. By default, the contents of `sbom-info.yaml` are automatically applied to the scan results when the scanner runs.

### Options
{: .specific-title}

- `--no_correction` : When this option is provided, the contents of `sbom-info.yaml` will **not** be applied to the scan results, even if the file exists in the working directory.
- `--correct_fpath [PATH]` : Specifies the path to the `sbom-info.yaml` file (e.g., `[PATH]/sbom-info.yaml`) when the file is located in a directory other than the top-level directory of the scan target.

<br><br>

## sbom-info.yaml Example
{: .left-bar-title}

```yaml
libidn: # Open source package
- version: "1.5"
  source name or path:
  - "src/libidn/*"
  - "b.c"
  license:
  - "GPL-3.0"
  - "LGPL-2.1"
  download location: "http://ftp.gnu.org/gnu/libidn"
  homepage: "https://www.gnu.org/software/libidn"
  copyright text: "Copyright 2002-2007, Simon Josefsson"

rsync: # Multiple versions of the same package
- version: "2.6.9"
  source name or path: "test/tool"
  license: "GPL-2.0"
  download location: "https://download.samba.org/pub/rsync/src"
  homepage: "http://rsync.samba.org"
- version: "3.1.2"
  source name or path: "test/tool"
  license: "GPL-3.0"
  download location: "https://download.samba.org/pub/rsync/src"
  homepage: "http://rsync.samba.org"
  copyright text:
  - "Copyright 1996 Andrew Tridgell"
  - "Copyright 1996 Paul Mackerras"
  - "Copyright 2003-2015 Wayne Davison"

'-': # All files in the directory are developed in-house
- version: ''
  license: LicenseRef-LGE-Proprietary
  copyright text: Copyright 2026 LG Electronics Inc.
  source name or path:
  - "src/lge/*"

'-': # Exclude specific paths from scan results
- version: ''
  exclude: True
  source name or path:
  - "build/*"
  - "test/*"
```

<br><br>

## sbom-info.yaml Field Descriptions
{: .left-bar-title}

The following fields are available when writing `sbom-info.yaml`.

### 1. Package Name (Header paragraph)
{: .specific-title}
- **Required:** Enter the Package Name (OSS Name) as the key.
- If the package has no name (e.g., in-house developed code), enter `'-'`.

### 2. Version paragraph
{: .specific-title}

| Field | Required / Optional | Value Type | Description / Example |
|---|---|---|---|
| **version** | Required | String | Package version. Use an empty string (`''`) if there is no version.<br>ex) `version: "2.8"` |
| **source name or path** | Optional | String \| Array of String | File or path to correct or exclude.<br>ex) `source name or path: "src/*"`<br>ex) `- "main.c"`<br>&nbsp;&nbsp;&nbsp;&nbsp;`- "main.h"` |
| **license** | Optional | String \| Array of String | License to apply.<br>ex) `license: "Apache-2.0"`<br>ex) `- "GPL-2.0"`<br>&nbsp;&nbsp;&nbsp;&nbsp;`- "LGPL-2.1"` |
| **download location** | Optional | String | URL where the package can be downloaded.<br>ex) `download location: "https://ftp.gnu.org/gnu/glibc"` |
| **homepage** | Optional | String | Homepage URL of the open source project.<br>ex) `homepage: "http://google.com"` |
| **copyright text** | Optional | String | Copyright notice associated with the package.<br>ex) `copyright text: "Copyright 2020 Test"`<br>For multi-line entries, use YAML's `\|` syntax. |
| **exclude** | Optional | Boolean | Set to `True` to exclude the path from scan output (e.g., build scripts).<br>ex) `exclude: True` |
| **comment** | Optional | String | Any additional comments related to the entry.<br>ex) `comment: "This is the build tool"` |
