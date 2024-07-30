# EmpireCMS 

### Introduction to EmpireCMS 

EmpireCMS is an open-source content management system (CMS).

download link：http://www.phome.net/downcenter/empirecms/ecms75/download/EmpireCMS_7.5_SC_UTF8.zip

### Vulnerability description

EmpireCMS v7.5 version has an SQL injection vulnerability, which arises from the application's lack of validation for external input SQL statements. Remote attackers can exploit this vulnerability to execute arbitrary code and obtain sensitive information through the EditClass function in e/class/classfun. php.

### vulnerability analysis

The vulnerability exists in line 2095 of classfun, where the bname parameter is passed to the frontend without effective filtering, and then passed to the Update statement for execution

![image-20240729180518750](EmpireCMS-SQL-Inject (Classfun Error).assets/image-20240729180518750.png)

![image-20240729180513675](EmpireCMS-SQL-Inject (images1/image-20240729180513675.png)

### Vulnerability reproduction

1、Log in to the backend  -> check "栏目" -> "管理栏目(分页)"

![image-20240729180848232](EmpireCMS-SQL-Inject (Classfun Error).assets/image-20240729180848232.png)

2、Check "修改" And submit -> Burpsuite packet capture -> Change bname parameter to inject SQL error into Poc, and successfully inject database name:

```
Poc: bname=test'and+extractvalue(1,concat(char(126),database()))+and'
```

![image-20240729181354631](EmpireCMS-SQL-Inject (Classfun Error).assets/image-20240729181354631.png)
