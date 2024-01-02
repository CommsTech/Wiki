---
title: Google Search Operators
description: A list of google search operators for better search results
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Google_Search_Operators
## Google Search Operators List(https://kinsta.com/blog/google-search-operators/#google-search-operators-list)

Here is a complete list of all working Google advanced search operators:

- **“search term”** Use this to do an exact-match search.
- **OR** Search for this OR that. This will return results related to the two terms or both.
- **AND** Search for this AND that. This will only return results related to the two terms
- **–** Exclude a term or search phrase.
- ***** Acts as a wildcard and will match any word or phrase.
- **( )** Groups multiple terms or operators to control how the search is shown.
- **$** Search for prices.
- **define**: Displays the meaning of a word in a card-like result.
- **Cache:** Returns the most recent cached version of a web page (as long as the page is indexed).
- **filetype:** Shows results of a certain filetype (PDF, DOCX, TXT, PPT, etc.)
- **site:** Limit results to a specific website.
- **related:** Find sites related to another site.
- **intitle:** Find pages that contain a specific word in the title.
- **allintitle:** Like “intitle,” this finds web pages containing all of the specific words in the page title.
- **inurl:** Finds pages with a certain word in the URL.
- **allinurl:** Similar to “inurl,” this finds web pages containing all of the URL’s specific words.
- **intext:** Finds pages containing a specific word in the content.
- **allintext:** Finds results containing all of the specific words somewhere on the page.
- **AROUND(X)** This proximity search finds pages containing two words (or phrases) within X words of each other.
- **weather:** Finds the weather for a specific location.
- **stocks:** See stock information
- **map:** View map results for a location search.
- **movie:** Finds information about a specific movie.
- **in** Convert one unit into another (like currencies, weights, temperatures, etc.)
- **source:** Find news results from a certain source within Google News.

**Cache Command**

Acacheisametadatathatspeedsupthepagesearchprocess.Googlestoressome  
data in itscache, such ascurrentandpreviousversionsofthewebsites.Thiscache  
holds much useful information that the developers can use. Some developers use  
cache to store information for their testing purpose that can be changed with new  
changes to the website.

Youcanusethefollowingsyntaxforanyrandomwebsitetocheckthedata.Theresult  
may vary depending on the updates from Google.

cache:website address

Onceyourunthecommand,youmayfindmultipleresultsrelatedtothat.Youcanalso  
use keywords in our search results, such as ‘xyz’, as shown in the below query.

cache:https://xyz.nu.edu.pk/Login XYZ

Once you get the output, you can see that the keyword will be highlighted.

**Intext and Allintext Command**

Tofindaspecifictextfromawebpage,you canusetheintextcommandintwoways.  
First,youcanprovideasinglekeywordintheresults.Second,youcanlookformultiple  
keywords.

You can use the following syntax for a single keyword.

Intext:usernames

Ifyouwanttousemultiplekeywords,thenyoucanuseallintext.Allthekeywordswillbe  
separated using a single space between them.

Googlewill considerall thekeywords andprovide all thepagesin theresult. Thus,  
usersonlygetspecificresults.So,makesureyouusetherightkeywordsorelseyou  
can miss important information.

Supposeyouwanttolookforthepageswithkeywords“username”and“password:”you  
can use the following query.

allintext:”username” “password”

You will get all the pages with the above keywords.

**Filetype Command**

ThisisoneofthemostimportantDorkingoptionsasitfiltersoutthemostimportantfiles  
fromseveralfiles.Forexample,youcan applyafilterjusttoretrievePDFfiles.Ifyou  
are adeveloper, you can go for thelogfiles, allowing themtokeep track easilyby  
applying the right filter.

To access simple log files, use the following syntax:

filetype:log

Youwillgetalltypesoflogfiles,butyoustillneedtofindtherightonefromthousandsof  
logs.So,tonarrowdownyourfilesearch,youbemorespecificwiththetypeoffileyou  
use with this syntax:

allintext:username filetype:log

Youwillgetspecificresultswiththeusernamementionedinit—allyouneedtodois  
provide the right keyword.

**Intitle Command**

SometimesyouwanttofilteroutthedocumentsbasedonHTMLpagetitles.Themain  
keywordsexistwithinthetitleoftheHTMLpage,representingthewholepage.So,we  
can use this command to find the required information.

Suppose you arelookingfordocumentsthathaveinformationaboutIPCamera.You  
have to write a query that will filter out the pages based on your chosen keyword.

You can use the following syntax:

intitle:”ip camera”

You can also use multiple keywords with this query to get more specific results,  
separating each keyword with double-quotes.

First,Googlewillretrieveallthepagesandthenapplythefiltertothatretrievedresult  
set. It will discard the pages that do not have the right keyword.

You can use the following syntax for that:

allintitle:”ip camera” “dvr”

You can see all the pages with both keywords.

**inurl Command**

Thiscommandworkssimilartotheintitlecommand;however,theinurlcommandfilters  
outthedocumentsbasedontheURLtext.ThosekeywordsareavailableontheHTML  
page,withtheURLrepresentingthewholepage.Youcanusethiscommandtofilterout  
the documents.

Supposeyouwant thedocumentswiththeinformation relatedtoIPCamera.Youcan  
simplyusethefollowingquerytotellgoogleandfilteroutallthepagesbasedonthat  
keyword. You can also provide multiple keywords for more precise results.

Syntax:

allinurl:tesla lambo

**Site Command**

Sitecommandwillhelpyoulookforthespecificentity.Atfirst,youcantryforkeywords  
thatwillprovide youwithabroadrangeofinformationthatmayormaynotbeasper  
your need. Then, you can narrow down your searchusing othercommands with a  
specific filter.

Supposeyouwanttobuyacarandarelookingforvariousoptionsavailablefrom2020.  
You’llgetalonglistofoptions.Now,youcanapplysomekeywordstonarrowdownyour

searchandgatherspecificinformation thatwillhelpyoubuyacar.Here,youcanuse  
the site command to search only for specific websites.

For example:

site:https://global.honda/

You can do the same for other cars also.

**ext Command**

If you want to search for a specific type of document, you can use the ext command.

Supposeyouwanttowriteanarticleonaspecifictopic,butyoucannotstartrightaway  
without researching that topic. Mostly the researched articles are available in PDF  
format. You can specify the type of the file within your dork command.

Here, ext stands for an extension. This command works similarly to the filetype  
command. Now using the ext command, you can narrowdown your searchthat is  
limited to the pdf files only.

You can use the following syntax:

site:https://www.ford.com/ ext:pdf

**Inposttitle**

Youcanusethiscommandwhenyouwanttosearchforacertaintermwithintheblog.It  
is useful for blog search.

For example:

inposttitle:weight loss goals

**Allintitle**

Sayyourunablog,andwanttoresearchotherblogsinyourniche.Thiscommandwill  
help you look for other similar, high-quality blogs.

For example:

allintitle:how to write content for seo

**Allinanchor**

Youcanusethiscommandtodo researchonpagesthathaveallthetermsafterthe  
“inanchor” in the anchor text that links back to the page.

For example:

allinanchor:"how to draw anime"

**Inanchor**

Youcanusethiscommandtofind pageswithinboundlinksthatcontainthespecified  
anchor text.

For example:

inanchor:"digital painting"

**Around**

Lookingforsupernarrowresults?Thiscommandwillprovideyouwithresultswithtwo  
or more terms appearing on the page.

For example:

digital drawing AROUND(2) tools

**@command**

Ifyouwantyoursearchtobespecifictosocialmediaonly,usethiscommand.It’llshow  
results for your search only on the specified social media platform.

For example:

mangoes @facebook

**Quotes**

Ifyouusethequotesaroundthephrase,youwillbeabletosearchfortheexactphrase.  
The search engine results will eliminate unnecessary pages.

For example:

“search term 1”

**Related:**

Insomecases,youmightwantspecificdatawithmorethan onewebsitewithsimilar  
content. You can provide the exact domain name with this Google Dorking command:

“Related:domainname.com”

**Info**

Youcanusethiscommandtofindtheinformationrelatedtoaspecificdomainname. It  
letsyoudeterminethings,suchaspageswiththedomaintext,similaron-sitepages,and the  
website’s cache.

For example:

"Info:domainname.com"

**Weather**

Curious about meteorology? Use this command to fetch Weather Wing device  
transmissions.

intitle:"Weather Wing WS-2"

Youwillseeseveraldevicesconnectedworldwidethatshareweatherdetails,suchas  
wind direction, temperature, humidity, and more.

**Zoom Videos**

OnthehuntforaspecificZoommeeting?Youmayfinditwiththiscommand,butkeep  
inmindthatZoomhas sinceplaced somerestrictionstomakeithardertofind/disrupt  
Zoom meetings. However, as long as a URL is shared, you can still find a Zoom  
meeting.

TheonlydrawbacktothisisthespeedatwhichGoogleindexesawebsite.Bythetime  
a site is indexed, the Zoom meeting might already be over.

inurl:zoom.us/j and intext:scheduled for

**SQL Dumps**

Your database ishighly exposed if itismisconfigured. You can alsofindtheseSQL  
dumps on servers that are accessible by domain. Sometimes, such database-related  
dumpsappear onsitesiftherearenoproperbackupmechanismsinplacewhilestoringthe  
backups on web servers. To find a zippedSQLfile,use the following command.

"index of" "database.sql.zip"

**WordPress Admin**

You can easily find the WordPress admin login pages using dork, as shown below.

intitle:"Index of" wp-admin

**Apache2**

You can find Apache2 web pages with the following Google Dorking command:

intitle:"Apache2 Ubuntu Default Page: It works"

**phpMyAdmin**

This tool is another method of compromising data, as phpMyAdmin is used to  
administer MySQL over the web. The Google dork to use is:

"Index of" inurl:phpmyadmin

**JIRA/Kibana**

You canuseGoogleDorkstofindwebapplicationshostingimportantenterprise data  
(via JIRA or Kibana).

inurl:Dashboard.jspa intext:"Atlassian Jira Project Management Software"

● inurl:app/kibana intext:Loading Kibana

**cPanel Password Reset**

You can reset the passwords of the cPanel to control it:

inurl:_cpanel/forgotpwd

**Finding FTP Servers**

Ifyou wanttoaccesstheFTPservers,youmightneedtomixthequeriestogetthe  
desired output. You can use the following syntax:

intitle:”index of” inurl:ftp

Asaresult,youwillgetalltheindexpagesrelatedtotheFTPserveranddisplaythe  
directories.

Onceyougettheresults,youcancheckdifferentavailableURLsformoreinformation,  
as shown below.

### Accessing Online Cameras

Remember,informationaccessissometimeslimitedtocybersecurityteamsdespiteour  
walkthrough of this Google Dorks cheat sheet.You can usethe dork commandsto  
access thecamera's recording. Some people make thatinformation available to the  
public, which can compromise their security.

The following is the syntax for accessing the details of the camera.

Intitle:”webcamXP 5”’

### Operators

Tonarrow downandfilter yourresults,you canuseoperatorsforbettersearch.The  
following are some operators that you might find interesting.

**Search term**

Youcanusethisoperatortomakeyoursearchmorespecificsothekeywordwillnotbe  
confusedwith something else. For example,ifyouarespecificallylookingfor“Italian  
foods,” then you can use the following syntax.

“Italian foods”

**OR**

Usingthisoperator,youcan providemultiplekeywords.Youwillgetresultsiftheweb  
pagecontains anyof those keywords. Youcan separatethekeywordsusing“|.” For  
example.

site:facebook.com | site:twitter.com

**AND**

Thisoperatorwillincludeallthepagescontainingallthekeywords.Thekeywordsare  
separated by the ‘&’ symbol. You can use the following syntax.

site:facebook.com & site:twitter.com

**Operators Combinaison**

Not only this, you can combine both ‘or’ and‘and’ operators torefine thefilter. For  
example-

(site:facebook.com | site:twitter.com) & intext:"login"  
(site:facebook.com | site:twitter.com) (intext:"login")

**Include Results**

Togettheresultsbasedonthenumberofoccurrencesoftheprovidedkeyword.For  
example-

-site:facebook.com +site:facebook.*

**Exclude Results**

You can also exclude the results from your web page. For example-

site:facebook.* -site:facebook.com

**Synonyms**

Ifyouwanttosearchforthesynonymsoftheprovidedkeyword,thenyoucanusethe“~”sign  
beforethatkeyword.Then,Googlewillprovideyouwithsuitableresults.Forexample,ifyou  
want to searchforthe keyword“set”alongwithits synonym,suchasconfigure, collection,  
change, etc., you can use the following:

~set

**Glob Pattern**

Youcanusetheglobpattern(*)whenyouareunsurewhatgoesthereandtellGoogle  
to make the search accordingly. For example”

site:*.com

## Search parameters

Below are various search parameters:

```
● q- Its value is the search term.
● filter - Itsvaluecanbe 1 or0.Ifitsvalueissetto0,itwilldisplayallthepotential
duplicate results.
● as_epq- Itsvaluecanbeasearchphrase.Youcanuseittosearchforanexact
phrase. There is no need to enclose the search phrase within quotes.
● as_ft- Its value can be exclude (e) or include (i).
● as_filetype- Canbe afile extension. Youcan includeorexcludethefile type
indicated by as_ft.
● as_occt- Itsvaluecanbe-any(anywhere),title(pagetitle),body(pagetext),url
(pageurl), andlinks(pagelink). Youcanfindthekeywordwithinthespecified
location.
● as_dt- Itsvaluecanbe exclude(e)orinclude(i).You canuseittoexcludeor
include the site or domain indicated by as_sltesearch.
● as_sltesearch- Itsvaluecanbeasiteordomain.Youcanincludeorexcludethe
file type indicated by as_dt.
● as_qdr- Itsvaluecanbem3(threemonths),m6(sixmonths),andy(pastyear).
You can search for the pages included within the specified period.
```

### How to Prevent Google Dorks

You can use any ofthefollowing approachesto avoidfalling underthecontrol ofa  
Google Dork. The following are themeasures to preventGoogle dork:

● You must encrypt sensitive and personal information such as usernames, passwords,  
payment details, and so forth.  
● Also, check your website by running inquiries to check if you have any exposed sensitive  
data. If you find any exposed information, just remove them from search results with the  
help of the Google Search Console.  
Protectsensitivecontentusingrobots.txtdocumentavailableinyourroot-levelsitecatalog.It  
will prevent Google to index your website.

User-agent: *  
Disallow: /

Youcanalsoblockspecificdirectoriestobeexceptedfromwebcrawling.Ifyouhavean  
/admin area and you need to protect it, just place this code inside:

User-agent: *  
Disallow: /admin/

Restrict access to specific files:

User-agent: *  
Disallow: /privatearea/file.html

Restrict access to dynamic URLs that contain ‘?’ symbol:

User-agent: *  
Disallow: /*?

## How to Combine Google Search Operators(https://kinsta.com/blog/google-search-operators/#how-to-combine-google-search-operators)

One of the most useful things about Google search operators is that you can combine them for particular use cases.

You can quickly find the source of a quote, an original image, or even official documentation for (almost) anything.

**Just a word of warning:** if you start combining operators and running lots of searches, you may be prompted to prove you are not an evil robot trying to spam Google: