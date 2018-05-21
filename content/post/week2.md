---
title: "Community Bonding and the start of the Coding Period"
thumbnailImagePosition: left
thumbnailImage: //res.cloudinary.com/dmytyofam/image/upload/v1526577476/Wikimedia_Foundation_Logo.png
metaAlignment: center
coverMeta: out
date: 2018-05-17
categories:
- GSoC
tags:
- GSoC
- Wikimedia
- MediaWiki
- Page Forms
disqusIdentifier: gsoc1
---


<!--more-->
### The Community Bonding Period
My end semester exams took up most of my time during the community bonding period.
However, I managed to take out time to setup my own blog. Setting up the blog using
Hugo was fun and convenient. Also, I was lucky to find a great theme - tranquilpeak
which has lots of features and integrated services like disqus. Once I was statisfied
and had finished customizing the blog, I wrote my first blog post. Apart from this,
I interacted with my mentors, discussed a bit about the project and created a MediaWiki
user page for myself and updated my profile.

### Coding Period: Added a Special page
I made an initial commit to add an extra special page to the Page Forms Extension.
The special page tentatively named Edit using spreadsheet appears in the list of special
pages on Special:SpecialPages as follows:

{{< image classes="fancybox right clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1526908324/blog-images/week2/pic2.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1526908324/blog-images/week2/pic2.png" title="Special:SpecialPAges" >}}


A template must be passed as a GET variable along with Special:EditUsingSpreadsheet.
I no template is specified, the page displays a list of all the available templates in
the wiki similar to Special:Templates as shown below:

{{< image classes="fancybox right clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1526908324/blog-images/week2/pic1.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1526908324/blog-images/week2/pic1.png" title="Special:EditUsingSpreadsheet without a template(default)" >}}


If a template is specified, then the page currently displays a blank. I am currently working
on adding a spreadsheet interface using jsGrid to the page, which will have all the
template call to that template.
{{< image classes="fancybox right clear" src="https://res.cloudinary.com/dmytyofam/image/upload/v1526908324/blog-images/week2/pic3.png" thumbnail="https://res.cloudinary.com/dmytyofam/image/upload/v1526908324/blog-images/week2/pic3.png" title="Special:EditUsingSpreadsheet for template: Jsgrid-test" >}}
I am currently figuring out the best approach to make the spreadsheet and trying
to understand the functions and classes used to query and display data.

The above mentioned updates can be viewed in the following commit: **[https://gerrit.wikimedia.org/r/#/c/433565/](https://gerrit.wikimedia.org/r/#/c/433565/)**

Stay tuned for updates!

### Related Links
+ [https://gohugo.io/](https://gohugo.io/)
+ [https://themes.gohugo.io/hugo-tranquilpeak-theme/](https://themes.gohugo.io/hugo-tranquilpeak-theme/)
+ [https://www.mediawiki.org/wiki/User:Yashdeep97](https://www.mediawiki.org/wiki/User:Yashdeep97)
