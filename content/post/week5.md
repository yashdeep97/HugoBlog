---
title: "Functionality for updating and creating pages using the Spreadsheet interface"
thumbnailImagePosition: left
thumbnailImage: //res.cloudinary.com/dmytyofam/image/upload/v1526577476/Wikimedia_Foundation_Logo.png
metaAlignment: center
coverMeta: out
date: 2018-07-29
categories:
- GSoC
tags:
- GSoC
- Wikimedia
- MediaWiki
- Page Forms
disqusIdentifier: gsoc5
---

<!--more-->
### Why use MediaWiki API to update page contents?
The MediaWiki API is an extremely useful tool for developers. I was suggested to
use the pfautoeditAPI to update the page contents by Yaron. This turned out be
quite simple compared to the previous method of forom submission that I was using.
It also saved a lot time because now, only those rows which were edited, were updated
instead of processing all the rows. I feel, it also made the user interface simpler
and more intuitive. Furthermore, it made the interface dynamic as the updates would
get saved right away and any errors would also be displayed immediately.


### Using the pfautoeditAPI
The pfautoeditAPI makes use of forms to edit the contents of a template call in
a page. The form being used must contain the template.
The query used is:
{{< tabbed-codeblock >}}
    <!-- tab -->
baseUrl + '/api.php?action=pfautoedit&format=json&form=' + formName + '&target=' + pageName + updateString<!-- endtab -->
{{< /tabbed-codeblock >}}

Here, updateString consists all the edits made to the template call, where *key* is
the template parameter and *value* is the updated contents of that parameter.
{{< tabbed-codeblock >}}
    <!-- tab js-->
updateString += '&' + templateName + '[' + key + ']' + '=' + value; <!-- endtab -->
{{< /tabbed-codeblock >}}


### Creating pages using pfautoeditAPI
The pfautoeditAPI can not only edit pages, but can also create them if the pages
do not exist. The pages created, contain only the template which is being used
for the spreadsheet.
The same query which was used for updating is also used for creating a page. However,
this causes a problem because if a user tries to create a page which already exists,
the page might get updated. Thus, there is a check to find out if the the page already
exists, upon which an error is displyed.

### The biggest hurdles I faced:
I ran into lot of problems due to the asynchronous nature of javascript.( I removed
"async: false" because it was causing the page to freeze ). Nischay helped me resolve
this issue by using callbacks.

### Patch(es) on Gerrit:
+ [Add support for updating and creating pages using the EditUsingSpreadsheet special page](https://gerrit.wikimedia.org/r/#/c/mediawiki/extensions/PageForms/+/441261/)
