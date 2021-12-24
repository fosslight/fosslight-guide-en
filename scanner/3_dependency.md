---
published: true
title: FOSSLight Dependency Scanner
---
# FOSSLight Dependency Scanner

<img src="https://img.shields.io/pypi/l/fosslight_dependency" alt="License" /> <img src="https://img.shields.io/pypi/v/fosslight_dependency" alt="Current python package version." /> <img src="https://img.shields.io/pypi/pyversions/fosslight_dependency" /> [![REUSE status](https://api.reuse.software/badge/github.com/fosslight/fosslight_dependency_scanner)](https://api.reuse.software/info/github.com/fosslight/fosslight_dependency_scanner)
    
[**FOSSLight Dependency Scanner**](https://github.com/fosslight/fosslight_dependency_scanner) is the tool that supports the analysis of dependencies for multiple package managers. It detects the manifest file of package managers automatically and analyzes the dependencies with using open source tools. Then, it generates the report file that contains OSS information of dependencies.

{::options parse_block_html="true" /}
<details>
<summary markdown="span">Supported Package Managers</summary>
- [Gradle](https://gradle.org/) (Java/Android)
- [Maven](http://maven.apache.org/) (Java)
- [NPM](https://www.npmjs.com/) (Node.js)
- [PIP](https://pip.pypa.io/) (Python)
- [Pub](https://pub.dev/) (Dart with flutter)
- [Cocoapods](https://cocoapods.org/) (Swift/Obj-C)
- [Swift](https://swift.org/package-manager/) (Swift)
- [Carthage](https://github.com/Carthage/Carthage) (Carthage)
- [Go](https://pkg.go.dev/) (Go)
</details>
{::options parse_block_html="false" /}

**Github Repository** : [https://github.com/fosslight/fosslight_dependency_scanner]()  
**License** : [Apache-2.0](https://github.com/fosslight/fosslight_dependency_scanner/blob/main/LICENSE)

## Contents
  - [Prerequisite](#-prerequisite)
  - [How to install](#-how-to-install)
  - [How to run](#-how-to-run)
  - [Result](#-result)
  - [How it works](#-how-it-works)


## 📋 Prerequisite
Because we utilize the different open source software to analyze the dependencies of each package manager, you need to set up the below Prerequisite steps according to package manager to analyze.

{::options parse_block_html="true" /}
<details>
<summary markdown="span">**Prerequisite for NPM**</summary>
1. Install the NPM License Checker to analyze the npm dependencies.
```
$ npm install -g license-checker
```
 > To install license-checker globally, '-g' option is required. If you do not have 'sudo' access, then you can change default path to install global modules.
```
$ npm set prefix ~/.npm
$ PATH=~/.npm/bin:$PATH
```

2. Run the command to install the dependencies. (optional)
```
$ npm install
```
 > It can be skipped if the project meets any of the following cases.
 > - If the 'package.json' file exists in the input directory, it will be executed automatically by FOSSLight Dependency Scanner. So you can skip it.
 > - If the 'node_modules' directory already exists, you can run FOSSLight Dependency Scanner by setting the input directory to the path where node_modules is located.
</details>

<details>
<summary markdown="span">**Prerequisite for Gradle**</summary>
1. Add the License Gradle Plugin in build.gradle file.
```
plugins {
    id 'com.github.hierynomus.license' version '0.15.0'
}
downloadLicenses {
    includeProjectDependencies = true
    dependencyConfiguration = 'runtimeClasspath'
}
```
 > If the gradle version is 4.6 or lower, then add the 'runtime' instead of 'runtimeClasspath' in the dependencyConfiguration.

2. Run the 'downloadLicenses' task.
```
$ gradlew downloadLicenses
```
</details>

<details>
<summary markdown="span">**Prerequisite for Android (gradle)**</summary>
1. Add the android-dependency-scanning Plugin in build.gradle file.
```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'org.fosslight:android-dependency-scanning:1.0.0'
    }
}
```

2. Add the below line in build.gradle file in the app(your application name, default : app) directory.
```
apply plugin: 'org.fosslight'
```

3. Run the 'generateLicenseTxt' task.
```
$ gradlew generateLicenseTxt
```
</details>

<details>
<summary markdown="span">**Prerequisite for Pypi**</summary>
```tip
- You can run this tool with virtual environment for separating the project dependencies from system global dependencies.
- If the 'requirements.txt' file is located in the input path, FOSSLight Dependency Scanner can automatically install and analyze the dependencies. So you can skip the prerequisite for Pypi.
```

1. Create and activate the virtual environment
```
// virtualenv example
$ virtualenv -p /usr/bin/python3.6 venv
$ source venv/bin/activate
// conda example
$ conda create --name {venv name}
$ conda activate {venv name}
```

2. Install the dependencies in the virtual environment.
```
// If you install the dependencies with requirements.txt...
$ pip install -r requirements.txt
```
</details>

<details>
<summary markdown="span">**Prerequisite for Maven**</summary>
```tip
If the 'pom.xml' is located in the input directory, FOSSLight Dependency Scanner will automatically add and execute the license-maven-plugin. So you can skip the prerequisites below.
```
<ol>
<li>Add the license-maven-plugin into pom.xml file.</li>
<pre>
&lt;project&gt;
  ...
  &lt;build&gt;
  ...
    &lt;plugins&gt;
    ...
      &lt;plugin&gt;
        &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
        &lt;artifactId&gt;license-maven-plugin&lt;/artifactId&gt;
        &lt;version&gt;2.0.0&lt;/version&gt;
        &lt;executions&gt;
          &lt;execution&gt;
            &lt;id&gt;aggregate-download-licenses&lt;/id&gt;
            &lt;goals&gt;
              &lt;goal&gt;aggregate-download-licenses&lt;/goal&gt;
            &lt;/goals&gt;
          &lt;/execution&gt;
        &lt;/executions&gt;
      &lt;/plugin&gt;
    &lt;/plugins&gt;
    ...
  &lt;/build&gt;
  ...
&lt;/project&gt;
</pre>

<li>Run the license-maven-plugin task.</li>
<pre>
$ mvnw license:aggregate-download-licenses
</pre>
</ol>
</details>

<details>
<summary markdown="span">**Prerequisite for Pub**</summary>
1. Run the flutter_oss_licenses.
```
$ flutter pub get
$ flutter pub global activate flutter_oss_licenses
$ flutter pub global run flutter_oss_licenses:generate.dart
```
</details>

<details>
<summary markdown="span">**Prerequisite for Cocoapods**</summary>
1. Install the pod package through Podfile.
```
$ pod install
```
</details>

<details>
<summary markdown="span">**Prerequisite for Swift**</summary>
1. Create a github personal access token and use it with '-t' option when running the FOSSLight dependency scanner. It needs the Github API to get the license information of the github repository.  
Please refer the [github docs guide to create a token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token).
</details>

<details>
<summary markdown="span">**Prerequisite for Carthage**</summary>
1. Create 'Cartfile.resolved' by running the package installation command.
```
$ carthage update
```
2. Create a github personal access token and use it with '-t' option when running the FOSSLight dependency scanner. It needs the Github API to get the license information of the github repository.  
Please refer the [github docs guide to create a token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token).
</details>

<details>
<summary markdown="span">**Prerequisite for Go**</summary>
```tip
FOSSLight Dependency Scanner only supports for go modules. It automatically executes the 'go list -m all' command to obtain a list of dependencies, and then collects the open source software information such as license and repository. Therefore, you can execute the 'fosslight_dependency' command directly without prerequisite step.
```
</details>
{::options parse_block_html="false" /}


## 🎉 How to install
FOSSLight Dependency Scanner can be installed using pip3.    
It is recommended to install in the [python 3.6 + virtualenv](etc/guide_virtualenv.md) environment.

```
$ pip install fosslight-dependency
```

## 🚀 How to run

You can run the FOSSLight Dependency Scanner with options based on your package manager.
```
$ fosslight_dependency [option] <arg>
```
### Options
```
        Optional
            -h                              Print help message.
            -v                              Print the version of the fosslight_dependency.
            -m <package_manager>            Enter the package manager.
                                             (npm, maven, gradle, pip, pub, cocoapods, android, swift, carthage, go)
            -p <input_path>                 Enter the path where the script will be run.
            -o <output_path>                Output path
                                             (If you want to generate the specific file name, add the output path with file name.)
            -f <format>                     Output file format (excel, csv, opossum)

        Required only for pypi
            -a <activate_cmd>               Virtual environment activate command (ex, 'conda activate (venv name)')
            -d <deactivate_cmd>             Virtual environment deactivate command (ex, 'conda deactivate')

        Required only for swift, carthage
            -t <token>                      Enter the github personal access token.

        Optional only for gradle, maven
            -c <dir_name>                   Enter the customized build output directory name
                                              (default name : 'build' for gradle, 'target' for maven)

        Optional only for android
            -n <app_name>                   Enter the application directory name where the plugin output file is located
                                             (default: app)
```

### Tips to run
When you run the FOSSLight Dependency Scanner, the input path('-p' option) should be designated as the top directory of the project where the package manager's manifest file exists as above.
The manifest file of each package manager is as follows:
```
  - Npm : package.json
  - Pypi : requirements.txt
  - Maven : pom.xml
  - Gradle (Android) : build.gradle
  - Pub : pubspec.yaml
  - Cocoapods : Podfile
  - Swift : Package.resolved
  - Carthage : Cartfile.resolved
  - Go : go.mod
```

- Swift package manager
  - Exceptionally, you can run "fosslight_dependency -m swift -t {token} command in the path where {Projectname}.xcodeproj file is located.
  - Then it can find the 'Package.resolved' file in {Projectname}.xcodeproj/project.xcworkspace/xcshareddata/swiftpm and run automatically.


## 📁 Result
```
$ tree
.
├── FOSSLight-Report_2021-05-03_00-39-49_SRC.csv
├── FOSSLight-Report_2021-05-03_00-39-49.xlsx
├── fosslight_dependency_log_2021-05-03_00-39-49.txt
└── Opossum_input_2021-05-03_00-39-49.json
```
- FOSSLight-Report_[datetime].xlsx : FOSSLight Dependency Scanner result in spreadsheet format.
- FOSSLight-Report_[datetime]_[sheet_name].csv : FOSSLight Dependency Scanner result in csv format.
- fosslight_dependency_log_[datetime].txt: The execution log.
- Opossum_input_[datetime].json : FOSSLight Dependency Scanner result for [OpossumUI](https://github.com/opossum-tool/OpossumUI)

### Result Contents
It prints the OSS information based on manifest file(package.json, pom.xml) of dependencies (including transitive dependencies).
For a unique OSS name, OSS name is printed such as (package_manager):(oss name) or (group id):(artifact id).

| Package manager                | OSS Name                 | Download Location                                                                                  | Homepage                                            |
| ------------------------------ | ------------------------ | -------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Npm                            | npm:(oss name)           | Priority1. repository in package.json <br> Priority2. npmjs.com/package/(oss name)/v/(oss version) | npmjs.com/package/(oss name)                        |
| Pip                            | pypi:(oss name)          | pypi.org/project/(oss name)/(version)                                                              | homepage in (pip show) information                  |
| Maven<br>& Gradle<br>& Android | (group_id):(artifact_id) | mvnrepository.com/artifact/(group id)/(artifact id)/(version)                                      | mvnrepository.com/artifact/(group id)/(artifact id) |
| Pub                            | pub:(oss name)           | pub.dev/packages/(oss name)/versions/(version)                                                     | homepage in (pub information)                       |
| Cocoapods                      | cocoapods:(oss name)     | source in (pod spec information)                                                                   | cocoapods.org/pods/(oss name)                            |
| Swift                      | swift:(oss name)     | repositoryURL in Package.resolved                                                                   | repositoryURL in Package.resolved                            |
| Carthage                      | carthage:(oss name)     | github repository in Cartfile.resolved                                                                   | github repository in Cartfile.resolved                            |
| Go                      | go:(oss name)     | pkg.go.dev/(oss name)@(oss version)                                                                   | repository in pkg.go.dev/(oss name)@(oss version)                        |

## 🧐 How it works
FOSSLight Dependency Scanner utilizes the open source software for analyzing each package manager dependencies. We choose the open source software for each package manager that shows not only the direct dependencies but also the transitive dependencies including the information of dependencies such as oss name, oss version and license name.

Each package manager uses the results of the following software:

- NPM : [NPM License Checker](https://github.com/davglass/license-checker)
- Pypi : [pip-licenses](https://github.com/raimon49/pip-licenses)
- Gradle : [License Gradle Plugin](https://github.com/hierynomus/license-gradle-plugin)
- Maven : [license-maven-plugin](https://github.com/mojohaus/license-maven-plugin)
- Pub : [flutter_oss_licenses](https://github.com/espresso3389/flutter_oss_licenses)
- Android(gradle) : [android-dependency-scanning](https://github.com/fosslight/android-dependency-scanning)

Because we utilizes the different open source software to analyze the dependencies of each package manager, you need to set up the **Prerequisite** steps according to package manager to analyze.