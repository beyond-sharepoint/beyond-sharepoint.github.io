---
layout: post
title: SharePoint Framework - ClientSideWebPart vs AppPart
categories: [SharePoint, O365, SPFx]
tags: [SharePoint Framework, sharepoint]
fullview: true
comments: true
---

One of the interesting things about building a SharePoint Add-in model AppPart was the issue of where the client side script lived. App script parts surface their content within a containing IFrame on the page which points to the app part's content (script files etc) on the app web. This means that the SP cross-domain library needed to be used any time the app part needed to access content on the host web through services, as that was a CORs call.

Back in 2014, Vesa (current MS PM leading SPFx) introduced us to the pattern of using the Add-in model to simply deploy a Script Editor web part that is configured to load the necessary JS associated with HTML+JS in a web part.

<https://blogs.msdn.microsoft.com/vesku/2014/07/08/introducing-app-script-part-pattern-for-office365-app-model/>

By doing so we didn't need to use the cross-domain library as the reference to our JS is just a script embed on the page. Cool.


Fast-forward to today and this pattern is now pretty much what the new SPFx ClientSideWebPart gives us, except that it fixes the two downsides that Vesa mentions in the above blog:

1. With a script editor web part, Vesa mentions that an end-user could inadvertently change the embed script tag. The CSWP uses the manifest file to determine what JS references to load, so this potential is now gone.
2. The current "mode" (view/edit) that the web part page is in is now passed to the CSWP code so a CSWP can easily determine what mode it is in and change the rendering accordingly.
3. Module loading was a pain when you got into App Script development, the CSWP uses SystemJS to facilitate module loading and in doing so ensures that the required modules are loaded before your CSWP's render method.


Unfortunately one big downside still remains in SPFx -- since the packaged .spapp does reference a .webpart file, SPFx solutions are still not able to be hosted in the SharePoint app/addin store. Whether this changes by the time the SPFx RTMs or a new "SPFx Add-in" directory or a seperate permission level will spring up is yet to be seen.


So we've seen how the CSWP in SPFx could be seen as an evolution of Vesa's app script part pattern, and some of the benefits that the CSWP gives you in developing HTML+JS based web parts over using app script parts.

Thanks,