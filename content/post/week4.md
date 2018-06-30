---
title: "Pagination for the spreadsheet interface using Ajax"
thumbnailImagePosition: left
thumbnailImage: //res.cloudinary.com/dmytyofam/image/upload/v1526577476/Wikimedia_Foundation_Logo.png
metaAlignment: center
coverMeta: out
date: 2018-06-30
categories:
- GSoC
tags:
- GSoC
- Wikimedia
- MediaWiki
- Page Forms
disqusIdentifier: gsoc4
---

I spent my third week of coding implementing pagination for the spreadsheet interface.
 This was a crucial task which helped in reducing the time taken to load the data.
<!--more-->
### Fetching data using Ajax
Pagination required the dynamic loading of data. Thus, the previously used method
of using the database slave to retrieve data would not work. This is where the
Mediawiki API comes into picture. The API provides a query action that enables
the retrieval of data dynamically using Ajax.
The first query is used to retieve the names of all the pages in which the template is embedded:
{{< tabbed-codeblock >}}
    <!-- tab -->
baseUrl + '/api.php?action=query&format=json&list=embeddedin&eilimit=500&eititle=Template:' + templateName<!-- endtab -->
{{< /tabbed-codeblock >}}

Now using another query, the page contents are retreived for each page:
{{< tabbed-codeblock >}}
    <!-- tab -->
baseUrl + '/api.php?action=query&format=json&prop=revisions&rvprop=content&formatversion=2&titles=' + pageName<!-- endtab -->
{{< /tabbed-codeblock >}}
After the page contents are fetched, all the template calls are seperated and the
values are extracted to be stored in an array of objects using javascript functions.

### Pagination using jsGrid methods
The jsGrid library provides inbuilt methods to support pagination. The configuration
required to enable pagination is as follows:
{{< tabbed-codeblock >}}
    <!-- tab js -->
    autoload: true,
    paging: true,
    pageLoading: true,
    pageSize: 25,
    pageIndex: 1,<!-- endtab -->
{{< /tabbed-codeblock >}}

The jsGrid library also provides the **loadData(filter)** method which is called when data loading for each
page. I overrided this method to get the page contents using Ajax and convert the results
into required format to display them in the spreadsheet.
An inbuilt pager is provided by the jsGrid library that is automatically rendered
below the grid.


{{< image classes="fancybox center clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1530374724/pager.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1530374724/pager.png" title="Pager provided by jsGrid" >}}

The number of pages are calculated using the **_pagesCount()**. I overrided the
function as follows:
{{< tabbed-codeblock >}}
    <!-- tab js -->
    pagesCount: function() {
	  pageSize = this.pageSize;
	  return Math.ceil( pages.length / pageSize );
    }
    <!-- endtab -->
{{< /tabbed-codeblock >}}

### Selector for Number of results to show
To create the the selector, I used the widgets from the Object Oriented User Interface
(OOUI) library, namely PopupButtonWidget, ButtonSelectWidget and the ButtonOptionWidget.


{{< image classes="fancybox center clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1530376000/pageSizeSelector.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1530376000/pageSizeSelector.png" title="Selector for Number of results to show using OOUI" >}}

When a different **pageSize** is selected the loadData() function is called again
and the grid is refreshed according to the new pageSize.


My next task will be to enable adding of new pages using the interface and to improve
the autoedit API for saving pages. Stay tuned for updates!


### Patches on Gerrit:
+ [Add pagination feature to EditUSingSpreadsheet special page](https://gerrit.wikimedia.org/r/#/c/mediawiki/extensions/PageForms/+/441261/)
