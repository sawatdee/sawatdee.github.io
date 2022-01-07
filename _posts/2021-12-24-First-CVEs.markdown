---
layout: post
title:  "CVE-2021-38694 - CVE-2021-38697"
date:   2021-12-24 14:24:55 +0700
categories: post
---
### Vulnerabilities found in SoftVibe SARABAN for INFOMA
#### Summary
##### Vulnerability List
1. SQL Injection -- CVE-2021-38694
2. Stored Cross Site Scripting (XSS) -- CVE-2021-38695
3. Incorrect Access Control -- CVE-2021-38696
4. Unauthenticated unrestricted File Upload -- CVE-2021-38697

---

#### Details

##### **1. SQL Injection**
###### **Description**
The application allows performing a union-based injection query on the uid parameter in main.aspx.

###### **Impact**
This vulnerability allows attackers to retrieve all databases data.

###### **Known Affected Component**
SARABAN for INFOMA 1.1

###### **Steps to Reproduce**
1. Use cURL to send the SQL payload.

```shell
curl -H "Content-Type: application/json" -d '{uid: "a UNION ALL SELECT [payload]-- aa", pwd: "pass", dbname:"[dbname]""}'' https://[target]/main.aspx/signin
```


##### **2. Stored Cross Site Scripting (XSS)**
###### **Description**
The application is vulnerable to a Stored Cross-Site Scripting (XSS) vulnerability that allows users to store scripts in some fields (e.g., subject, description) of the document form.

###### **Impact**
This vulnerability allows attackers to inject malicious scripts.

###### **Known Affected Component**
SARABAN for INFOMA 1.1

###### **Steps to Reproduce**
1.	Login to the application
2.	Navigate to the create document menu in https://[target]/main.aspx
3.	Inject the payload `"><script>console.log(1)</script>"` in the subject and description fields.
4.	Click on create button.
5.	The script get executed. It also runs when any user views this document.


##### **3. Incorrect Access Control**
###### **Description**
The application does not perform access control to signature files properly.

###### **Impact**
Attacker can access signature files on the application without any authentication.

###### **Known Affected Component**
SARABAN for INFOMA 1.1

###### **Steps to Reproduce**
Browse to https://[target]/flowdata/Signatures/[file_name].


##### **4. Unauthenticated unrestricted File Upload**
###### **Description**
The file upload function of the application does not perform access control and file type restriction properly.

###### **Impact**
Unauthenticated attackers can upload files with any file extension, which leads to arbitrary code execution.

###### **Known Affected Component**
SARABAN for INFOMA 1.1

###### **Steps to Reproduce**
1. Use cURL to upload an ASP webshell to the application.

```shell
curl -F name=shell.asp -F “filename=@/PATH/TO/shell.asp” https://[target]/[application_path]/signimgHandler.ashx?uid=[file_name]&cab=[dbname]
```
* To execute the webshell, combind this vulnerability with the Incorrect Access Control vulnerability (CVE-2021-38696) to access the uploaded file at https://[target]/flowdata/Signatures/[file_name].asp.


##### **Credits**
Incognito Lab
