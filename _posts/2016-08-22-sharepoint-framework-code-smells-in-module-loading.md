---
layout: post
title: SharePoint Framework - Code Smells in Module Loading
categories: [SharePoint, O365, SPFx]
tags: [SharePoint Framework, sharepoint]
fullview: true
comments: true
---

The SharePoint guys recently sent out this video on developing jQuery based client side web parts with the SharePoint Framework.

<http://dev.office.com/blogs/building-simple-sharepoint-framework-client-web-part>


 
One thing that caught my eye when viewing the video was the module loading code in their render method:
![screenshot](/assets/media/2016-08-22-sharepoint-framework-code-smells-in-module-loading-01.png "Screenshot")


Effectively this is saying "load jQuery... wait until it's done... then load jQuery.simpleWeather... then call renderConents()"

It's even worse since we know that within the webpart's lifecycle, "render()" is called after the web part's dependencies are loaded. So we're actually waiting to pull down our bundle, then waiting for jquery then waiting for jquery.simpleWeather, then finally calling renderContents()

Further, this all is wrapped in a conditional on "this.renderedOnce()" to not load these libraries multiple times...

to sum it up...

![ewwwww](/assets/media/2016-08-22-sharepoint-framework-code-smells-in-module-loading-02.png "Ew!")


What if we needed a few more libraries that depend on one another -- we'd end up with a massive pyramid of doom.

**Don't do this. _please_.**

We could simplify this code with ```Promise.all(...)``` but we've got all these advanced module loading and bundling capabilities right at our fingertips already included with SPFx, let's use them!


I'm going to show one way of mitigating this code smell -- there's other ways, but this is pretty straightforward.

What we're going to do is add jQuery and jQuery.simpleWeather to the project via NPM  so that it will be part of the webparts bundled javascript.

As webpack is also included by default, we'll use its "require" method to pull in jQuery and simpleWeather

Since the required files are contained in the existing bundle, there will be no additional external requests to fetch the additional libraries and we can use really simple module loading to boot.

1. First, we'll add the jquery and simpleweather from npm with the following command at the shell in your project's directory.
``` bash
$ npm i jquery jquery.simpleweather --save
```

2. Let's add the references to jQuery and jQuery.simpleWeather at the top of our webpart's configuration
<br/>
(lines 12 and 13 are new)
<br/>
![screenshot](/assets/media/2016-08-22-sharepoint-framework-code-smells-in-module-loading-03.png "Screenshot")
This is using webpack's require function to load jQuery and adding it to the global object and doing so in a TypeScript friendly manner.
<br/><br/>
We then load simpleWeather.

3. Let's refactor the render method:
<br/>
![screenshot](/assets/media/2016-08-22-sharepoint-framework-code-smells-in-module-loading-04.png "Screenshot")
<br/><br/>
Annnnd that's it.

If we look at our timeline on the page, we see a single request to our simple-weather.bundle.js which now contains jQuery and jQuery.simpleWeather
![screenshot](/assets/media/2016-08-22-sharepoint-framework-code-smells-in-module-loading-05.png "Screenshot")


So now the code performs better and is more maintainable. Fun!

**Note: The jQuery.simpleweather library on the Github repo plays better with module loading, npm install the one from the github repo and you'll be able to use jQuery in local scope rather than global.**