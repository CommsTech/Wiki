---
title: Python_Regex_list
description: # Python Regex blocking list
dateCreated: 2022-09-09T04:44:01.149Z
published: true
editor: markdown
tags: pfblockerNG, regex, blocklist
dateModified: 
---
# Python_Regex_list
# Python Regex blocking list
# 1/23/2022

######### ADS and Trackers ###########
(.*\.|^)((think)?with)?google($|((adservices|static|syndication|tagmanager|tagservices|-analytics)($|\..+)))
([A-Za-z0-9.-]*\.)?.double-click(\.\w{2}\.\w{2}|\.\w{2,4})
([0-9]{6})\.(ortb|artb|xmlfeed(-eu|))\.(adtelligent)(\.net|\.com)
(10er-tagesticket\.(de|us|ca)\.)([A-Za-z0-9.-]*)(\.data\.adobedc\.net)
([A-Za-z0-9.-]*\.)?adnxs(\.\w{2}\.\w{2}|\.\w{2,4})
([A-Za-z0-9.-]*\.)?advertising\.com
([A-Za-z0-9.-]*\.)?appnexus(\.\w{2}\.\w{2}|\.\w{2,4})
([A-Za-z0-9.-]*).([A-Za-z0-9.-]*)(filter||.)?clickbank(\.net|\.com) #clickbank
([A-Za-z0-9.-]*\.)?clicks\.beap(/|$|)\.bc\.yahoo(\.\w{2}\.\w{2}|\.\w{2 ,4}) #Yahoo
([A-Za-z0-9.-]*\.|)(www|)(deeplink|api|audience|cdn|engine|info|mail|measure|media|platform|status|)(\.|)(mobileapptracking)(\.com|\.net) #deeplink dirty
([A-Za-z0-9.-]*)\.(metrics)(\.convertexperiments)(\.com)
([A-Za-z0-9.-]*)\.(fls\.|)\.?doubleclick(\.\w{2}\.\w{2}|\.\w{2,4})#doubleclick
([0-9]{6})(\.gsspcln\.jp) #gsspcln
([A-Za-z0-9.-]*\.)?evidon(\.\w{2}\.\w{2}|\.\w{2,4}|\.com)
([A-Za-z0-9.-]*\.)?flashtalking(\.\w{2}\.\w{2}|\.\w{2,4}|\.com)
([A-Za-z0-9.-]*\.)?googlesyndication(\.\w{2}\.\w{2}|\.\w{2,4})
([0-9]*)\.(collect|recs)\.(igodigital)(\.net|\.com) #igodigital
(av[0-9]{3})(\.infusionsoft)(\.com|\.app)# infusionsoft
([A-Za-z0-9.-]{32})(\.redinuid)(\.imrworldwide)(\.net|\.com) #imrworldwide
([0-9]{9})\.(keywordblocks)(\.net|\.com) #keywordblocks
(koi-[A-Za-z0-9.-]*)(\.marketingautomation\.services)
(avi-[A-Za-z0-9.-]*)(\.pubmatic)(\.com) #pubmatic
(a[0-9]*\.)(actonservice|casalemedia)\.com
([A-Za-z0-9.-]*\.)?match\.com
([A-Za-z0-9.-]*\.)?mathtag(\.\w{2}\.\w{2}|\.\w{2,4}|\.com)
([A-Za-z0-9.-]*\.)?mediamath(\.\w{2}\.\w{2}|\.\w{2,4})
([0-9]{3})-([a-z]{3})-([0-9]{3})\.(mkto(web|resp))(\.net|\.com)
(onfiei-[A-Za-z0-9.-]*)(\.wcohus|\.uscluster)(\.teradatadmc|)(\.com|\.net)
(or[A-Za-z0-9.-]*)(\.insight\.omniture\.com)
([A-Za-z0-9.-]*\.)?s\.yimg\.com/cv/ae/us/audience
([A-Za-z0-9.-]*\.)?scorecardresearch(\.\w{2}\.\w{2}|\.\w{2,4}|\.com)
([A-Za-z0-9.-]*\.)?secure\.footprint\.net
([A-Za-z0-9.-]*\.)?sitescout(\.\w{2}\.\w{2}|\.\w{2,4})
(o[0-9]*)\.(ingest)\.(sentry)\.(io)$ #Sentry.io
(node-[A-Za-z0-9.-]*-[A-Za-z0-9.-]*\.|)(sitescout)(\.com|\.\w{2}\.\w{2}|\.\w{2,4}) #sitescout
(zn[A-Za-z0-9.-]*)(-|)([A-Za-z0-9.-]*|)(\.siteintercept\.qualtrics\.com)
(u[0-9]*)\.(ct\.|[0-9]{2}\.|)(spylog|sendgrid)(\.net|\.com)
([A-Za-z0-9]*|)([A-Za-z0-9]*|)(-(reserve|news|par|survey|tra)-[0-9]*|\.|)(spartoo)\.(widget\.criteo\.com|com|co\.uk|[A-Za-z]{2}) #spartoo dirty
([A-Za-z0-9.-]*\.)?surveylink($|/)
([A-Za-z0-9.-]*\.)?turn(\.\w{2}\.\w{2}|\.\w{2,4}|\.com)
([A-Za-z0-9.-]*\.)?w55c(\.\w{2}\.\w{2}|\.\w{2,4})
([A-Za-z0-9.-]*\.)?yieldmanager(\.\w{2}\.\w{2}|\.\w{2,4})
(ads|captive|logs)\.roku\.com #Roku Ads
(google|partner|pub)-{0,}ads{0,}-{0,}(apis{0,})\.[a-z]{2,7}
([A-Za-z0-9.-]*)(000webhostapp)(\.com)#webhostapp
^(.+[-_.])??adse?rv(er?|ice)?s?[0-9]*[-.]
^(.+[-_.])??m?ad[sxv]?[0-9]*[-_.]
^(.+[-_.])??xn--
^(.+[_.-])?adse?rv(er?|ice)?s?[0-9]*[_.-]
^(.+[_.-])?telemetry[_.-]
^ad([sxv]?[0-9]*|system)[_.-]([^.[:space:]]+\.){1,}|[_.-]ad([sxv]?[0-9]*|system)[_.-]
^adim(age|g)s?[0-9]*[_.-]
^adtrack(er|ing)?[0-9]*[_.-]
^advert(s|is(ing|ements?))?[0-9]*[-_.]
^aff(iliat(es?|ion))?[-.]
^analytics?[_.-]
^banners?[_.-]
^beacons?[0-9]*[_.-]
^count(ers?)?[0-9]*[_.-]
^mads\.
^pixels?[-.]
^stat(s|istics)?[0-9]*[_.-]
^track(ers?|ing)?[0-9]*([-.]|[_.-])
^traff(ic)?[-.]
double-{0,}clic(k|k[.]*by-{0,}google)\.[a-z]{2,7}$
google-{0,}(analytics{0,}|(ad|tag)manager)\.[a-z]{2,7}$
google-{0,}(analytic|syndication|(ad[a-z0-9]*|tag)-{0,}service)[s]\.[a-z]{2,7}$
^whatsapp-cdn-shv-[0-9]{2}-[a-z]{3}[0-9]\.fbcdn\.net$ #Whatsapp1
^((www|(w[0-9]\.)?web|media((-[a-z]{3}|\.[a-z]{4})[0-9]{1,2}-[0-9](\.|-)(cdn|fna))?)\.)?whatsapp\.(com|net)$ #Whatsapp2

########### SMART TVs ##########
# LG
(^|\.)ibs\.lgappstv\.com
(^|\.)lgsmartad\.com
(^|\.)smartshare\.lgtvsdp\.com
(^|\.)rdx2\.lgtvsdp\.com

# Samsung
(^|\.)giraffic\.com$
(^|\.)internetat\.tv$
(^|\.)pavv\.co\.kr$
# (^|\.)samsungcloudcdn\.com$ # prevents updates
# (^|\.)samsungcloudsolution\.com$ # prevents internet connection
(^|\.)samsungcloudsolution\.net$
(^|\.)samsungelectronics\.com$
# (^|\.)samsungotn\.net$ # prevents updates
(^|\.)samsungrm\.net$

# Vizio
(\.|^)tvinteractive\.tv$

# Other
# (^|\.)myhomescreen\.tv$ # can produce false positives, rather block individual domains
^api\..*\.hismarttv\.com$
(^|\.)buffpanel\.com$
(^|\.)bugsnag\.com$
(^|\.)redshell\.io$
(^|\.)treasuredata\.com$

# Country Blocks
(\.|^)cn$	
(\.|^)ru$
