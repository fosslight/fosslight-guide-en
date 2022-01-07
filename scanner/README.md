---
sort: 5
published: true
title: 🔎 FOSSLight Scanner
---
# FOSSLight Scanner

## Introduction

FOSSLight Scanner can perform an analysis for open source compliance at once. It can perform open source analysis of source code, binary and dependency. Also, it can check whether an open source complies with the copyright/license writing rule.

## Features

<div class="flex-container">
  <div class="flex-contents">
    <div>
      <div id="feature_title">
        Inclusive Scanning
      </div>
      <div id="feature_img">
        <img src="https://img.icons8.com/dotty/80/000000/check-all.png"/>
      </div>
      <div id="feature_content">
        It can detect source code, binary as well as dependency.
      </div>
    </div>
  </div>

  <div class="flex-contents">
    <div>
      <div id="feature_title">
        Integrated One
      </div>
      <div id="feature_img">
        <img src="https://img.icons8.com/wired/64/000000/workspace-one.png"/>
      </div>
      <div id="feature_content">
        It can work from one command line through a single integrated package.
      </div>
    </div>
  </div>

  <div class="flex-contents">
    <div>
      <div id="feature_title">
        Independent Module
      </div>
      <div id="feature_img">
        <img src="https://img.icons8.com/dotty/80/000000/module.png"/>
      </div>
      <div id="feature_content">
        The scanner module can be used independently and lightly.
      </div>
    </div>
  </div>
</div>

## Scanner Projects

#### 1. [**FOSSLight Reuse**](1_reuse.md)
- FOSSLight Reuse is a tool that can be used to comply with the copyright/license writing rules in the source code.
- It checks reuse compliance by using the **[reuse-tool](https://github.com/fsfe/reuse-tool)**.

#### 2. [**FOSSLight Source Scanner**](2_source.md)
- FOSSLight Source Scanner uses ScanCode, a source code scanner, to detect the copyright and license phrases contained in the file.
- It can scan the source code by using the **[scancode-toolkit](https://github.com/nexB/scancode-toolkit)** and **[scanoss.py](https://github.com/scanoss/scanoss.py)**.

#### 3. [**FOSSLight Dependency Scanner**](3_dependency.md)
- FOSSLight Dependency Scanner is the tool that supports the analysis of dependencies for multiple package managers.
- It can analyze the dependency using the following open source software.
  - NPM : **[NPM License Checker](https://github.com/davglass/license-checker)**
  - Pypi : **[pip-licenses](https://github.com/raimon49/pip-licenses)**
  - Gradle : **[License Gradle Plugin](https://github.com/hierynomus/license-gradle-plugin)**
  - Maven : **[license-maven-plugin](https://github.com/mojohaus/license-maven-plugin)**
  - Pub : **[flutter_oss_licenses](https://github.com/espresso3389/flutter_oss_licenses)**

#### 4. [**FOSSLight Binary Scanner**](4_binary.md)
- FOSSLight Binary Scanner searches for a binary and outputs OSS information if there is an identical or similar binary from the Binary DB.
- It can analyze the open source info. in '.jar' file by using **[Dependency-check-py](https://github.com/jhermann/dependency-check-py)**.

#### 5. [**FOSSLight Scanner**](https://github.com/fosslight/fosslight_scanner)
- FOSSLight Scanner performs open source analysis after downloading the source or with the local source path. 
- It can analyze the open source to use FOSSLight Source Scanner, FOSSLight Dependency Scanner and FOSSLight Binary Scanner.
  
     
      
<div class="right"><a href="https://icons8.com/icon">&lt;Icons by Icons8&gt;</a></div>
