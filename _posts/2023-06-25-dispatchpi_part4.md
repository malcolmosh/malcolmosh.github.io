---
layout: post
title: 'DispatchPi part 4 : an iOS shortcut 📱'
date: 2023-06-25 12:00:00-0500
last_modified_at: 2023-06-25 12:00:00-0500
description: #an example of a blog post with table of contents on a sidebar
tags: #formatting toc sidebar
categories: DispatchPi
giscus_comments: false
related_posts: false
toc:
  beginning: true
---


### Installing the shortcut


<div class="row">

<div class="col-sm-8 mt-3 mt-md-0">
  The last tutorial step streamlines picture sharing with an iOS shortcut that allows you to select a picture from your camera roll, add a message to superimpose on top, and share it to another frame.
  <br><br>
  Email is a reliable way to share an image, as it will sit in the iOS Mail app's outbox in case you have limited connectivity. 
  <br><br>
  You can <a href="https://www.icloud.com/shortcuts/1f72f553c4314d92acdd975478d0d340"> download and customize the DispatchPi iOS shortcut here. </a><br><br>
  <strong>Notes</strong> : 
  <ul>
    <li>The “FROM” email field should be edited to display the email account your are using with the iOS Mail app.</li>
    <li>The earth frame owner can send images from any email address, whereas the satellite frame must use the address added to line 69 of the `/server/main.py` file. This is the web app distinguishes between the two frames.</li>
  </ul>
</div>

<div class="col-sm-4 mt-3 mt-md-0" >
{% include figure.liquid path="assets/img/dispatchpi/frame2.gif" title="GIF of the sharing process height=400px" class="img-fluid rounded z-depth-1" %}
</div>
</div>
  
And just like that, we’ve wrapped everything together into a single action, abstracting away all of our complicated pipeline! 
  
I hope you enjoyed this tutorial. Let me know on [Github](https://github.com/malcolmosh) or on [Reddit](http://reddit.com/u/malcolm_osh) if you managed to implement DispatchPi ! Cheers.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/pink_frame_lowres.jpg" alt="The final result" caption="The assembled frame." height="400px" %}


### Further reading

- [simplegmail](https://github.com/jeremyephron/simplegmail)  would be an alternative way of connecting to our Gmail inbox
- [A good explanation of the Gmail Python API](https://www.thepythoncode.com/article/use-gmail-api-in-python)
- You could try replicating the whole server script in [N8N, an automation tool that ressembles Zapier](https://n8n.io/) 
