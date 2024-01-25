---
layout: page
title: DispatchPi
description: Share and queue eink images through Gmail
img: /assets/img/dispatchpi/pink_frame2_thumbnail.jpg
importance: 1
category: work
related_publications: false
---
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dispatchpi/white_frame_lowres.jpg" title="The final result." class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dispatchpi/pink_frame_lowres.jpg" title="The final result." class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Send pictures from your phone to a frame via Gmail.
</div>


### Send pictures from your phone to a Pi-powered screen         

<div class="row">

<div class="col-sm-8 mt-3 mt-md-0">
I built DispatchPi to share pictures with my fiancée, who was working in Québec's remote Nunavik region, where internet is unreliable. The project consists of a pair of e-ink frames that fetch images from a Gmail inbox.
<br> <br>
Each frame is powered by a Raspberry Pi Zero and hooked up to a 7.5 inch Waveshare e-ink screen. Their job is to pull an image from a fixed URL at regular intervals. To exchange images, each sender uses a custom shortcut on iPhones. 
<br><br>
DispatchPi is built for a pair of picture frames, which we’ll call <strong>Satellite frame</strong> and <strong>Earth frame</strong>. The satellite frame displays images received from any sender, while the earth frame only shows from a specific sender. This filtering happens in the web app which serves each audience on a different URL endpoint. 
</div>

<div class="col-sm-4 mt-3 mt-md-0" >
{% include figure.liquid path="assets/img/dispatchpi/frame1.gif" title="Two frames." class="img-fluid rounded z-depth-1" caption = "DispatchPi"%}
</div>
</div>

### Full Schema
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="/assets/img/dispatchpi/frame_diagram.png" title="System diagram"  caption="System diagram" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
 

### Sharing images, step by step

1. You open an iOS shortcut to send a recent picture to a dedicated shared Gmail inbox. 
2. Every day, each frame pings a fixed URL (server.com/satellite or server.com/earth) This kicks off a series of events:
3. The Flask app on Google Cloud run spins up:
    * It authenticates into the Gmail inbox
    * It filters by sender, to only fetch the image intended for it. 
    * Brings up the latest email received that contains an attachment       
    * Converts the image to bitmap and grayscale, overlays the email body text on top
    * Publishes the transformed image at the fixed URL
4. The Pi attached to the frame downloads the image from the URL and pushes it to the e-ink screen. 
5. Done!
  
> ### What you need 
>* A Google Cloud Platform account with billing enabled    
>* A pair of Raspberry Pi Wero W with empty microSD cards     
    * You can find the [Pi Zero board on Amazon here](https://www.amazon.ca/Raspberry-Pi-Zero-W/dp/B06XFZC3BX/ref=sr_1_3?crid=3ALT2S14BD8S2&keywords=raspberry+pi+zero+w&qid=1706153826&sprefix=raspberry+pi+zero+w%252Caps%252C85&sr=8-3&_encoding=UTF8&tag=57a5dad807324-20&linkCode=ur2&linkId=8d8b26c8cf3feaa32f56eca526592423&camp=15121&creative=330641), although the price might be better at smaller local retailers, such as [here on PiShop.ca](https://www.pishop.ca/product/raspberry-pi-zero-w/?ref=272efi4f)
    * I bought the board without headers and added GPIO headers with a [solderless hammer header kit](https://www.pishop.ca/product/gpio-hammer-header-solderless-male-female-installation-jig/?ref=272efi4f), as I didn't feel like fussing over soldering
    * This [pair of Sandisk 32GB microSD cards](https://www.amazon.ca/SanDisk-2-Pack-microSDHC-Memory-2x32GB/dp/B08J4HJ98L/ref=sr_1_8?crid=1UUURHEV2I772&keywords=sandisk+microsd&qid=1706153988&sprefix=sandisk+microsd%252Caps%252C82&sr=8-8&_encoding=UTF8&tag=57a5dad807324-20&linkCode=ur2&linkId=c4ad64dabb854c625a68ad48a238521b&camp=15121&creative=330641) should work fine for the project
>* Two [Waveshare 7.5” e-ink screens for Raspberry pi ](https://www.amazon.ca/Waveshare-7-5inch-HAT-Raspberry-Consumption/dp/B075R4QY3L/ref=sr_1_3?crid=2V6TDL93DY3AX&keywords=waveshare+7.5&qid=1706153772&sprefix=waveshare+7+5%252Caps%252C81&sr=8-3&_encoding=UTF8&tag=57a5dad807324-20&linkCode=ur2&linkId=17b52fc2f91a0ffcbfcdd86762cb4328&camp=15121&creative=330641)
>* A computer to configure the Raspberry Pi microSD cards and run terminal commands (I'm using a Macbook)

### A note on cost    
  
Using Google services like Gmail API and Google Cloud Run is not free. But pinging our lightweight Cloud Run server once a day entails minimal costs. With daily calls to the web app from both frames, I pay about 7 Canadian cents per month. There are usually free trial credits available for new GCP users. 

### The tutorial   
  
This tutorial covers the whole installation process, which is divided in four parts. For any direct questions, write me on [Github](https://github.com/malcolmosh) or on [Reddit](http://reddit.com/u/malcolm_osh). 

- Part 1 : [Build a local app](https://malcolmosh.github.io/pages/DispatchPi/dispatchpi_part1/)
- Part 2 : [Host your app online](https://malcolmosh.github.io/pages/DispatchPi/dispatchpi_part2/)
- Part 3 : [Setup the e-ink frame](https://malcolmosh.github.io/pages/DispatchPi/dispatchpi_part3/)
- Part 4 : [Create an iOS shortcut](https://malcolmosh.github.io/pages/DispatchPi/dispatchpi_part4/)