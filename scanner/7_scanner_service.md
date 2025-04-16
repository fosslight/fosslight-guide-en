---
published: true
---

# (Enterprise Only) FOSSLight Scanner Service 

## Overview
{: .left-bar-title}
Source, binary and dependency analysis is performed using [FOSSLight Scanner](https://fosslight.org/fosslight-guide-en/scanner/)as a web service. Analysis results are generated in the form of a [FOSSLight Report](https://fosslight.org/hub-guide-en/learn/2_fosslight_report.html).    
- URL : [http://fs.lge.com](http://fs.lge.com)
- Supported items
    - [FOSSLight Source Scanner](https://fosslight.org/fosslight-guide-en/scanner/2_source.html)
    - [FOSSLight Binary Scanner](https://fosslight.org/fosslight-guide-en/scanner/4_binary.html)
    - [FOSSLight Dependency Scanner](https://fosslight.org/fosslight-guide-en/scanner/3_dependency.html)
        - Supported : npm, pypi, maven, pub, go, nuget, cargo 
          (Other package managers are currently undergoing verification)  
- Not supported items
    - [FOSSLight Android Scanner](https://fosslight.org/fosslight-guide-en/scanner/6_android.html)
    - [FOSSLight Yocto Scanner](https://fosslight.org/fosslight-guide-en/scanner/5_yocto.html)


## How to use
{: .left-bar-title}

### Login
{: .specific-title}
- You can access it by entering your AD account ID and password at [http://fs.lge.com](http://fs.lge.com)<br>
![log-in](images/7_fl_ss_login.png){: .styled-image width="30%"}  

### Create a Project 
{: .specific-title} 
1. Click the "New Project" button in the upper right corner to create a project. 
![New Project](images/7_fl_ss_newproject.png){: .styled-image width="80%"}  

2. Enter the contents in"Create a Porject".
![Creat a Project](images/7_fl_ss_create_project.png){: .styled-image width="80%"}
    - **Name** : Enter the Project name.
    - **Inputs** : Select sources to analyze.
        - **Upload files** : Compress and upload files to be analyzed. (Please upload only 1 file.)  
        - **Download URLs** :  Enter the source link to be analyzed (link that can be obtained through "wget" or "git clone")    
            - **Public** 
                - The example of input value
                    - wget : github.com/LGE-OSS/example/archive/refs/tags/v1.0.0.zip
                    - git clone : github.com/LGE-OSS/example
            - **Private Git** 
                - **http://** or **https://** : You must enter the user name and PAT value. (The PAT value must not include /.)  
                - **ssh://** : Copy the provided ssh key value and register it in your private git repository. ⚠️ Please use PAT instead of ssh for github.    
                ![ssh](images/7_fl_ss_ssh.png){: .styled-image}  
    - **Pipeline**
        - scan_all : Analyze source, binary, dependency.
        - source : Analyze only the source code.
        - binary : Analyze only binary. 
    - **Permission **
        - Private : Only the creator can view.
        - Public : Other people can view the project and download analysis results through the link.


### Result
{: .specific-title} 
![analysis_result](images/7_fl_ss_analysis_result.png){: .styled-image}
1. **Download results** : You can download the analysis result file.
    - FOSSLight Scanner Result :  This is the file to upload during the FOSSLight Hub [Identification](https://fosslight.org/hub-guide/tutorial/1_project/2_Identification/) process.
2. **Files** :  This is the file to upload during the FOSSLight Hub
3. **Detected Open Source** : You can check the analysis results as a list. (FOSSLight Dependency results not included)  
![detected_opensource](images/7_fl_ss_detected_opensource.png){: .styled-image width="80%"}  
