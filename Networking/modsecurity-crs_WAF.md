---
title: ModSecurity-crs Web Application Firewall (WAF)
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# modsecurity-crs_WAF

 I used the modsecurity WAF which I setup in a [[docker]] node running behind the firewall based on https://github.com/jcmoraisjr/modsecurity-spoa.

The tricky part was using the pfSense GUI to configure the [[HAProxy]] frontend.

[![ae0367c9-73e7-499e-949b-a28002ccff4b-image.png](https://forum.netgate.com/assets/uploads/files/1623680173334-ae0367c9-73e7-499e-949b-a28002ccff4b-image.png)](https://forum.netgate.com/assets/uploads/files/1623680173334-ae0367c9-73e7-499e-949b-a28002ccff4b-image.png)  
Important to note here is the "dummy" frontend entry that is there only to ensure that the modsecurity spoe backend is included. The port 33780 is just an arbitrary choice which is never intended to be actually used.

Here are non-default parts of the dummy frontend entry:  
[![42e12876-164a-4b18-b1b0-c3c7ab1a65de-image.png](https://forum.netgate.com/assets/uploads/files/1623680454501-42e12876-164a-4b18-b1b0-c3c7ab1a65de-image.png)](https://forum.netgate.com/assets/uploads/files/1623680454501-42e12876-164a-4b18-b1b0-c3c7ab1a65de-image.png)  
[![e52d5ff0-d52c-40f9-b7de-1ef6ae00f5ff-image.png](https://forum.netgate.com/assets/uploads/files/1623680512522-e52d5ff0-d52c-40f9-b7de-1ef6ae00f5ff-image.png)](https://forum.netgate.com/assets/uploads/files/1623680512522-e52d5ff0-d52c-40f9-b7de-1ef6ae00f5ff-image.png)

The backend's IP address in my setup is 192.168.90.1, and below is all that is needed:  
[![88dc5353-bebb-4d1a-b9be-faceed7e10a9-image.png](https://forum.netgate.com/assets/uploads/files/1623680665503-88dc5353-bebb-4d1a-b9be-faceed7e10a9-image.png)](https://forum.netgate.com/assets/uploads/files/1623680665503-88dc5353-bebb-4d1a-b9be-faceed7e10a9-image.png)

Finally, the tricky part is the frontend configuration. I've a bunch of web apps on the backend all of which are protected by the modsecurity WAF. Here are screenshots of the "working" frontend's non-default parts:  
[![d4b4bd53-a701-4952-8e35-dc4156f2a7e4-image.png](https://forum.netgate.com/assets/uploads/files/1623681168496-d4b4bd53-a701-4952-8e35-dc4156f2a7e4-image.png)](https://forum.netgate.com/assets/uploads/files/1623681168496-d4b4bd53-a701-4952-8e35-dc4156f2a7e4-image.png)  
[![3d96d0dc-34b3-4c8c-b05f-9c257dc00733-image.png](https://forum.netgate.com/assets/uploads/files/1623681213509-3d96d0dc-34b3-4c8c-b05f-9c257dc00733-image.png)](https://forum.netgate.com/assets/uploads/files/1623681213509-3d96d0dc-34b3-4c8c-b05f-9c257dc00733-image.png)  
[![3443761a-8c1f-46ee-84dc-d727394784fd-image.png](https://forum.netgate.com/assets/uploads/files/1623681257245-3443761a-8c1f-46ee-84dc-d727394784fd-image.png)](https://forum.netgate.com/assets/uploads/files/1623681257245-3443761a-8c1f-46ee-84dc-d727394784fd-image.png)

The last screenshot refers to a configuration file needed by haproxy. I used the filer package because I have a HA setup that needs several additional files sync'd to the backup node. You can do a direct edit if you only have one node but filer gives you the benefit of the files being part of the config.xml backups. Here is a screenshot of the filer entry needed:  
[![cd720e36-1603-43ba-b75e-84068649589f-image.png](https://forum.netgate.com/assets/uploads/files/1623681596307-cd720e36-1603-43ba-b75e-84068649589f-image.png)](https://forum.netgate.com/assets/uploads/files/1623681596307-cd720e36-1603-43ba-b75e-84068649589f-image.png)  
Please take note of the Script/Command entry, and IIRC the indentation used is critical. Also, at least in 2.5.1, you will find a Warning message in your general system log about the last line, which is harmless with present version of haproxy.

The setup works but I should add deploying a WAF this way may not be the best of ideas which probably why a WAF is not part of pfSense package lineup.

I tested this using this: https://github.com/wallarm/gotestwaf. The default rules of the modsecurity setup mentioned earlier doesn't score perfectly but shows pfSense interface to the WAF works and that its time to tune :)

## Kill Switch

If you would like to implement a kill switch the following changes will be required

The following changes were made which results in a 403 error protecting the backend application if the WAF container is not running.

[![963b85c5-b8a9-48af-b0be-e37a02dbf526-image.png](https://forum.netgate.com/assets/uploads/files/1631615854530-963b85c5-b8a9-48af-b0be-e37a02dbf526-image.png)](https://forum.netgate.com/assets/uploads/files/1631615854530-963b85c5-b8a9-48af-b0be-e37a02dbf526-image.png)

Additional Resources 

https://www.netnea.com/cms/apache-tutorial-6_embedding-modsecurity/

https://hub.docker.com/r/owasp/modsecurity-crs/

https://coreruleset.org/videos/

https://forum.netgate.com/topic/164410/haproxy-with-an-external-modsecurity-filter/8

https://owasp.org/www-project-modsecurity-core-rule-set/
