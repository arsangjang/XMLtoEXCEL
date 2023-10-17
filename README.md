# XML to EXCEL
Read and Convert XML file into Excel
---
title: "Read & Convert XML files"
output: html_notebook
---

Required libraries

```{r}
library(XML)
library(xml2)
library(xmlconvert)
library(plyr)
library(methods)
library(tidyverse)
library(writexl)
```

Set directory and copy your xml files into your directory

```{r}
setwd("C:/Users/XML folder")
```

print all available XML files in your directory

```{r}
all_xml_files <- list.files(pattern="\\.xml", full.names=TRUE)
all_xml_files
```

Read xml file, this file includes a information of 1 sample

```{r}
File_a167 <- read_xml("a_167.xml")
```

Extract variables of interest from XML file (a_167):

Variables (id) from Measurement node and Value from ReportableValue node

![XML file example; 3 variables is defined in this file: Variable_1 ;Variable_2, and Variable_3]([images/XML%20example.PNG](https://drive.google.com/file/d/1xipplHTjWZHFSox1XghUPFMULhhhHoLK/view?usp=sharing)

\* Measurements indicates the node for variables/ Measurement Id indicates variable name

We want this information from xml file:

| Patient ID | Age | Weight |
|------------|-----|--------|
| 1          | 29  | 84     |

R code to do this:

```{r}
data_xml = xml_find_all(File_a167, ".//Measurement") %>% 
  map_df(function(x) {
    list(
      PatientID=xml_attr(x, "Id"),
      Age=xml_find_first(x, ".//ReportableValue") %>% xml_attr("Value")
    )
  })

```

Print and save results as excel file

```{r}
print (data_xml, n=50)
write_xlsx(data_xml, "values_xml.xlsx")
```
