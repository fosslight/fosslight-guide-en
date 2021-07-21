# FOSSLight Report

```note
This is a template used by FOSSLight and FOSSLight Scanner, and is a document written to understand the open source status included in each project.
```

## Form

v1.0 : [FOSSLight-Report.xlsx](./files/FOSSLight-Report_v1.0.xlsx)

## Description by Sheet

### Project Info Sheet
![info](./files/project_info.png)
- About the report  
   Write the creator, division and date of creation.

- About the project  
   Provide information about the development project.

- About OSC Process  
   Provide information about the OSC process.

### 3rd Party Sheet
If your project includes software from 3rd party, you should obtain OSS checklist from 3rd party and check what open sources are used. (Refer to [3rd Party OSS Checklist](https://github.com/fosslight/fosslight/raw/main/src/main/resources/static/sample/FOSSLight-OSS-Checklist-for-3rdParty_Eng_1.0.xlsx))         
The OSS Checklist obtained should be registered in the [3rd Party](../started/2_try/5_third-party.md) menu of the FOSSLight and collected in the 3rd Party tab.     
If you export the FOSSLight report from the FOSSLight, the contents registered in the 3rd Party tab will be filled in the "3rd party" sheet. Therefore, do not fill in the "3rd party" sheet manually.       

![info](./files/3rd_party.png)


### SRC Sheet
Write open source information included by source code.   

![info](./files/src.png)
- The License Text in the Open Source file used is present, but if the Open Source name or the origin is unclear, put a hyphen ("-") in the OSS Name field.
- If multiple licenses are applied to a single open source, write them by comma-separated by license.
- Note. If you use [FOSSLight Source Scanner](https://github.com/fosslight/fosslight_source_scanner), you can automatically generate "SRC" sheet.


### BIN Sheet
Write open source information included by binary.     

![info](./files/bin.png)
- The license text in the open source file used to create the binary exists, but if the open source name or origin is unclear, put a hyphen ("-") in the OSS Name field.
- If multiple licenses are applied to a single open source, write them by comma-separated by license.



### BOM Sheet
The "BOM" (Bill of Materials) sheet is a sheet to show the contents of the open sources listed in the FOSSLight Report. Do not write this sheet.     
      
The "BOM" sheet is automatically included in the FOSSLight Report downloaded from the FOSSLight.

![info](./files/bom.png)






