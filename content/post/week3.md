---
title: "Displaying the template calls in a Spreadsheet Interface"
thumbnailImagePosition: left
thumbnailImage: //res.cloudinary.com/dmytyofam/image/upload/v1526577476/Wikimedia_Foundation_Logo.png
metaAlignment: center
coverMeta: out
date: 2018-06-11
categories:
- GSoC
tags:
- GSoC
- Wikimedia
- MediaWiki
- Page Forms
disqusIdentifier: gsoc3
---

In the second week of the coding period I moved on to the central task of my project
ie. displaying the spreadsheet in the special page previously created.
<!--more-->
### Fetching data and creating the Spreadsheet Interface
At first I was clueless about how I should get all the pages which contain the
selected template, but then I came across the templatelinks table which is part
of the Mediawiki core MySQl database. The templatelinks table stores the page_id
of each page that is made using a particular template. Thus using natural join, I
fetched the page names of all the pages containing that template. Using natural join
also ensures that we do not get any pages that have been deleted.
The query used is:
{{< tabbed-codeblock >}}
    <!-- tab sql -->
SELECT page_title from templatelinks INNER JOIN page ON tl_from = page_id;<!-- endtab -->
{{< /tabbed-codeblock >}}

After getting all the pages, I fetched the contents of all the pages and seperated the template calls from them and put the template calls in an array of strings. Then a
function was used to fetch all the values from the template calls and put them in
the global variable **$wgPageFormsGridValues**. The existing code of the Page Forms
extension that uses the **jsGrid** library was used to display the values in a spreadsheet:

{{< image classes="fancybox right clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1528732302/spreadsheet1.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1528732302/spreadsheet1.png" title="Display template calls in a Spreadsheet" >}}


### Getting the ideal input type for each field
The global variable **$wgPageFormsGridParams** was used to set the properties for each
column (according to each parameter of the template). A **PFTemplate** object was
created for the template and the **getTemplateFields** function was used to get
the template parameters. For each template field the field name, field label(title)
and input type were set using methods from the **PFTemplateField** class. Furthermore,
to determine the ideal input type to display the field values, we first check whether
Cargo or Semantic MediaWiki was used to store the data. We use the specialized
functions **getFieldType** and **getPropertyType** for Cargo and SMW respectivley to get
the input type and set the grid param values. It currently supports text, textarea,
checkbox and date. If the input type is not suited for any of those mentioned above,
we set it to text by default.


{{< image classes="fancybox right clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1528732302/idealinput.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1528732302/idealinput.png" title="Display template calls in a Spreadsheet" >}}

### Saving the edits to the different pages
This task was a bit tough for me and I had to put in a lot of thought as to how
I would implement it. There were two feasible options:

1. Get the values from the submitted form and the contents of the pages to be edited.
Then use a custom function to edit the page contents according to the submitted values.
The new page contents are then saved using the methods already defined in the
pfautoedit API.
2. Another approach would be to fetch a form that can be paired with each template
such that the form contains the template it is paired with. Then the pfautoedit API
is called after setting the form and the target page.

I have successfully tested the first approach and am currently trying the second
one. Even though the first approach can be used for more cases (pages not created
using forms), it currently does not include the implementation for adding a new
page and autocomplation (this is where the second approach might be useful). Thus,
it is to be seen which approach will be more suitable or if it will be possible
to somehow include the features of both the approaches.

### Patches on Gerrit for the above mentioned tasks:
+ [Display template calls in a Spreadsheet](https://gerrit.wikimedia.org/r/#/c/434549/)
+ [Ideal Input Type](https://gerrit.wikimedia.org/r/#/c/436030/)
+ [Fetch Form for each template](https://gerrit.wikimedia.org/r/#/c/437217/)
+ [Save Functionality using approach - 1](https://gerrit.wikimedia.org/r/#/c/438065/)
