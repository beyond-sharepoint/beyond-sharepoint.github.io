---
layout: post
title: SharePoint Framework - What It Is - Part 2
categories: [SharePoint, O365, SPFx]
tags: [SharePoint Framework, sharepoint]
fullview: true
comments: true
---

There were a few questions that came in, and I'll do my best to answer:

**What does SPFx provide to SPO that is currently not present in SP16? Is there a way I can use SPFx on-prem now?**

At this point, the only thing that appears to be new on SPO is the new ClientSideWebPart, ClientSitePage, and a new xml element in elements.xml

This web part provides the following behavior:
1) Provides a client-side web-part property framework
2) Embeds SystemJS for module loading (the "System" object becomes available at the global scope when a CSWP is added)

They've mentioned some new things (new SharePoint pages experience, SharePoint specific web hooks) but those don't appear to be available in the preview.

Really the approach should be to build your HTML/JS based native web app using any client-side technology you wish, possibly using the add-in web part (or not). When the CSWP becomes available in on-prem (via a future update) the effort to host your add-in in the webpart (if you so desire) will be minimal.
 
**What is in the new Javascript libraries that SPFx provides and are there OSS versions of these?**

The SPFx Yeoman template has your webpart requiring a base module named "@microsoft/sp-client-base"

 
MS doesn't appear to have opened access to these components on Github yet, but they can be found on NPM here:
<https://www.npmjs.com/package/@microsoft/sp-client-base>

 
Here's some things that caught my eye -- all of these are discoverable if you look at the node_modules/@microsoft folder after installing the SPFx yeoman template:

1) Fetch API polyfill used as the base HttpClient to making request to SP web services. See <https://github.com/github/fetch>
2) Digest caching (for X-RequestDigest). See <https://github.com/beyond-sharepoint/spo-remote-auth>
3) Class to generate GUIDs. See <https://github.com/broofa/node-uuid>
4) A custom bundle of lodash. Pull in full lodash (or specific components if needed), see the @microsoft/sp-lodash-subset package on NPM too.
5) New (but incomplete) JS-classes around SP-prefixed objects (SPUser, SPList, SPSite, etc). See <https://github.com/Kaldeera/ng-SharePoint>
6) A new SPModule loader which is a wrapper around SystemJS. Use SystemJS full yourself or your preferred module loader...

I'm sure this will expand over time.

**You mentioned the new SharePoint pages experience, what will this bring?**

Unfortunately I don't know much more about SharePoint Pages -- There's a small stub in the sp-client-preview module, but it's currently empty.

The workbench.aspx might be the start of the SharePoint Pages experience but it seems in it's infancy (or I have high standards :))

One could speculate that there could be a new type of webpart page that uses react components to handle the things that traditionally have been done via the (server-side) webpart manager, and facilitate inter-webpart messaging and so forth. 

Speculating further, things like layout could be handled by client-side rendering through page configuration, rather than static layout .aspx pages.

ClientWebPart discovery will still need to be handled, however, whether this is done by a new system, or leverages the existing add-in catalog is TBD.

Any other info anyone has would be highly interesting.

**What is the purpose of node? Can I use node in my SPFx solutions?**

NodeJS is used just for the tooling. E.g. Yeoman, Webpack, Gulp, etc are all NodeJS based toolkits and the SPFx Yeoman template provides a standard configuration of these tools to build SPFx client web parts with.

Node is a server-side tech so it's not something you can use directly for your client-side web parts. But, you can create services in node that your client side scripts call, if you want to, as well as import client-side JS files from node and require them in your SPFx web part.

**What about .Net Core? Why doesn't SPFx use MS's own stuff?**

Good question -- I don't have the precise answer to this but one can extrapolate:

1. Timing;
.Net Core is still very new as an RTM release, but .Net Core's tooling is still in preview, and the ecosystem is still developing. It might have been pre-mature to build a solution based on yet-to-be-fully-proven .Net Core.

2. Community;
Modern Web Development is aggressively using NodeJS and the modules that SPFx uses. It makes sense that SPFx wanted to bring SharePoint development to be in-line with what the rest of the Web Development community is using.

3. Expand audience;
SPFx doesn't replace existing ways to build SharePoint solutions. It provides another way - so by using this specific toolchain, and the client-side components in place, we might be able to see the types of developers that the SPFx is aiming to attract.

4. Embrace OSS;
The modules that SPFx uses are very clever tested components being maintained by large communities in themselves. Rather than re-create the wheel, it makes sense to embrace these open technologies and use them.


Will we see a reboot of SPFx to utilize .Net Core for the tooling? One can only surmise what goes on in terms of management and politics of blue-badge 'softies...