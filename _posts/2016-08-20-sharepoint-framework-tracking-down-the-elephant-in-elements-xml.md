---
layout: post
title: SharePoint Framework - Tracking down the elephant in elements.xml
categories: [SharePoint, O365, SPFx]
tags: [SharePoint Framework, sharepoint]
fullview: true
comments: true
---

If we look at our webpart\sharepoint\solution\debug folder after we've packaged it at least once using the gulp task, we find the following:
![screenshot](/assets/media/2016-08-20-sharepoint-framework-tracking-down-the-elephant-in-elements-xml-01.png "Screenshot")

As mentioned on Chris O'Brien's Blog, the \sharepoint folder contains the .spapp file which is our add-in (just a zip file using the Open Packaging Conventions) that contains the files in the /debug folder.

If you're a long time SharePont guy, you should recognize most of these files. The .webpart file contains the webpart definition (webpart type etc), and the WebPart_&lt;guid&gt;.xml is actually just our good old friend elements.xml:
![screenshot](/assets/media/2016-08-20-sharepoint-framework-tracking-down-the-elephant-in-elements-xml-02.png "Screenshot")


yep, all looks familiar. down at the bottom is our &lt;module&gt; that deploys our .webpart into the web part catalog on the add-in host web. If we wanted to, we could throw additional modules entries to include additional files on the host web with our CSWP on add-in deployment.


However, the elephant in the room is this new "ClientSideComponent" element.

We haven't seen this before in SharePoint on-prem, but we can easily figure out that this is pulling in the manifest.json file that describes the urls to our bundle files, some metadata and so forth.

We also have seen that this information provided here gets rendered by the CSWP to provide information to the SystemJS module loader of our web part. There's the baseurl of our bundle which could be hosted in a file on SP, locally, on github or whereever. 

Now, the $100,000 question is where this data gets stored in SPO to be retrieved by the CSWP. If we were able to modify this metadata we would be able to change the base location of our bundle files and the other metadata without needing to redeploy our addin.

This would be pretty handy in a dev/test scenario, or just upgrading an app hosted in a CSWP from one iteration to the next.


It's not a property bag setting, nor is it a hidden list on the add-in host web. If the we had the bits to the CSWP we could use a .net reflection tool to get an inkling of where it's being stored... but we don't.


Anyone have any thoughts?