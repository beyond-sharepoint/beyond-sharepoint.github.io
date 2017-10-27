---
layout: post
title: Creating an email enabled list in Office 365 and Microsoft Flow
categories: [O365, walkthrough]
tags: [O365, flow, mailbox, sharepoint, email enabled list, walkthrough]
fullview: true
comments: true
---

Had some extra time this morning to play around with O365 and Microsoft Flow.

So in this walkthrough, the desired functionality is to have a replacement for an e-mail enabled document library. Since this functionality isn't available directly in O365, we'll use Shared Mailboxes with Microsoft Flow to achieve similar functionality.

This process could also be done with Site Mailboxes, however, for whatever reason after waiting 2 hours after I created a site mailbox I still wasn't able to access the mailbox in Outlook Online to set up a forwarding rule and kept getting a "ConnectionFailedTransientException" ... so I went the shared mailbox route. Fun.


To get started, I'll create a Shared Mailbox in Office 365 by going to the Admin Center Home -> Shared Mailboxes.

I created a shared mailbox creatively called "emailenabledlist"
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-01.png "Screenshot")


Next, I granted permissions to myself on this mailbox. Now, since the Shared Mailbox is just a mailbox that's not associated with a user account, I'll need to set up a forwarding rule to forward emails to my mailbox in order to have it work with Microsoft Flow.
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-02.png "Screenshot")


I'll also create a folder in my personal account that will house the emails, and create a rule to move emails received from emailenabledlist to the folder. Note that there are many ways to skin a cat and this is just one method -- you'll probably want to create and document a more maintainable one.
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-03.png "Screenshot")




Next, I went over to https://flow.microsoft.com/en-us/

and used the "Save my email attachments to a SharePoint document library" template:
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-04.png "Screenshot")

When setting up the template, I went ahead set the incoming folder to be folder we set up in the rule, in my case "Site Mailbox Temp Drop Folder"
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-05.png "Screenshot")

I then set the create file action to the following settings.
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-06.png "Screenshot")


Saved and clicked done. At this point I should be able start testing to see if this is all actually works.
I'll send an e-mail to emailenabledlist with attachments see if the attachments get to where they need to be.


And here it is!
![screenshot](/assets/media/2016-08-01-creating-an-email-enabled-list-in-O365-and-microsoft-flow-07.png "Screenshot")



So there's one way to do it without writing a line of code. As I mentioned, this should work with Site Mailboxes too, but O365 was hiccuping on me.

Now if you customize the template and add conditionals you'll be able to do some advanced functionality. One might be to place attachments in separate folders or document libraries based on the email subject line.


As mentioned, this should be able to be done with code-based solutions (through Azure Web Jobs or an API via a WebHook  by monitoring the shared mailbox directly and creating the file in O365. This could give additional customization options not found in the MS Flow based solution.


Thanks,

**10/26/17 Update: A simpler way to perform these steps has been posted here: [https://beyond-sharepoint.github.io/o365/walkthrough/2016/08/01/creating-an-email-enabled-list-in-O365-and-microsoft-flow.html]**
