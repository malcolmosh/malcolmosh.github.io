---
layout: page
title: DailyPi
description: Daily tasks, events & weather on a battery-powered e-paper frame
img: /assets/img/dailypi/alternate_splash_image.jpg
importance: 1
category: work
related_publications: false
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/dashboardpi_assembly.jpeg" title="Assembly process." class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/dashboardpi_table.jpeg" title="Assembly process." class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/dashboardpi_back_table.jpeg" title="Assembly process." class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The assembly process.
</div>

DailyPi is a straightforward home dashboard that shows daily tasks, events and weather on a 7.5 inch e-paper screen. I wanted to have a small, unobtrustive frame that would give me the day's info at a glance. 

All of the data preparation is conducted server-side, through a web app hosted on Google Cloud Run, which collects information from multiple APIs and outputs a PNG image at a fixed URL. The local device is composed of a Raspberry Pi zero W computer physically wired to an e-ink screen. The Pi's task is to regularly refresh its PNG image by pulling it from the server.

> The **[code for DailyPi is available here on GitHub](https://github.com/malcolmosh/dailypi).** and you can **[start the tutorial here](/blog/2024/dailypi-tutorial/)**. 

{% include scripts/image_with_caption.liquid src="/assets/img/dailypi/secondary_splash.jpg" alt="Everything at a glance" caption="Everything at a glance (in French!)" %}

To make the device portable, I connected it to an external battery, a [PiSugar2 battery](https://www.tindie.com/products/pisugar/pisugar-2-battery-for-raspberry-pi-zero/) that plugs in directly to the Pi's board and lets you connect other components, such as HATs. With three refreshes a day and a wake time of two minutes, I am able to make the battery last roughly 2.5 weeks. The really cool thing about e-paper is that any image will continue displaying even without power, as the panel is made up of thousands of physical microcapsules of pigment that are physically reshuffled around by electricty. 

This is my second real e-ink project, and I aimed to make everything as simple as possible to maintain. I implemented a few time-saving features, such as a dynamic SVG template and continuous deployment from Github to Cloud Run.

I made the dashboard's interface in French, but the fields could easily be customized through code by swapping variable names.

### What you need


- A Google Cloud Platform account with billing enabled
- A pair of Raspberry Pi zero W with empty microSD cards
    * You can find the [Pi Zero board on Amazon here](https://www.amazon.ca/Raspberry-Pi-Zero-W/dp/B06XFZC3BX/ref=sr_1_3?crid=3ALT2S14BD8S2&keywords=raspberry+pi+zero+w&qid=1706153826&sprefix=raspberry+pi+zero+w%252Caps%252C85&sr=8-3&_encoding=UTF8&tag=57a5dad807324-20&linkCode=ur2&linkId=8d8b26c8cf3feaa32f56eca526592423&camp=15121&creative=330641), although the price might be better at smaller local retailers, such as [here on PiShop.ca](https://www.pishop.ca/product/raspberry-pi-zero-w/?ref=272efi4f)
    * I bought the board without headers and added GPIO headers with a [solderless hammer header kit](https://www.pishop.ca/product/gpio-hammer-header-solderless-male-female-installation-jig/?ref=272efi4f), as I didn't feel like fussing over soldering
    * This [pair of Sandisk 32GB microSD cards](https://www.amazon.ca/SanDisk-2-Pack-microSDHC-Memory-2x32GB/dp/B08J4HJ98L/ref=sr_1_8?crid=1UUURHEV2I772&keywords=sandisk+microsd&qid=1706153988&sprefix=sandisk+microsd%252Caps%252C82&sr=8-8&_encoding=UTF8&tag=57a5dad807324-20&linkCode=ur2&linkId=c4ad64dabb854c625a68ad48a238521b&camp=15121&creative=330641) should work fine for the project
- One [Waveshare 7.5” e-ink screen for Raspberry Pi](https://www.amazon.ca/Waveshare-7-5inch-HAT-Raspberry-Consumption/dp/B075R4QY3L/ref=sr_1_3?crid=2V6TDL93DY3AX&keywords=waveshare+7.5&qid=1706153772&sprefix=waveshare+7+5%252Caps%252C81&sr=8-3&_encoding=UTF8&tag=57a5dad807324-20&linkCode=ur2&linkId=17b52fc2f91a0ffcbfcdd86762cb4328&camp=15121&creative=330641)
- A computer to configure the Raspberry Pi microSD card and run terminal commands (I'm using a Macbook, but powershell on Windows works much the same!)

### How it works

I know how time-consuming tech projects can be - it always takes longer than we imagine to properly build what one has in mind, along with all of the tedious debugging this entails. I've come across lots of smart devices that are true labours of love. Considering this, I aimed to construct a flexible app that would remain simple to develop and maintain. 

For the UI, I didn't want to custom code complex html or CSS classes. Instead, a SVG file forms the backbone of the dashboard. As this file format can both be opened as an image or as code, it has the advantage of allowing dynamic field updates, to drop in new data on the fly. This idea is already present in quite a few e-ink projects, such as [Mendhak's e-paper dashboard](https://code.mendhak.com/raspberrypi-epaper-dashboard/), [Sevesalm's e-ink weather display](https://github.com/sevesalm/eInk-weather-display) and [r-mathieu's train map display](https://github.com/r-mathieu/E-Ink-Trainstation).

I created the SVG template from scratch in Adobe Illustrator, matching the frame's resolution (800 x 480 pixels). I then formatted all of the object fields and names by manipulating the SVG code in Vscode. An online editor such as [BoxySVG](https://boxy-svg.com/) can be especially useful to update the SVG graphics without messing up the order of the code and the custom object names. Alternating between Adobe Illustrator and Vscode is often destructive, since Illustrator will re-order the code and wipe out your custom code changes.

<div class="row">
    <div class="col-sm mt-6 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/blank_template_lowres.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-6 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/filled_out_template_lowres.png" title="example image" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    The blank template on the left, and the filled out image on the right
</div>

### The data

The dashboard's data is pulled from three sources :

- Google Calendar, for daily events
- Google Tasks, for grocery/to-do lists
- [Environment Canada](https://weather.gc.ca/canada_e.html), for weather forecasts, through the [amazing env_canada library](https://github.com/michaeldavie/env_canada) built by MichaelDavie. Basically, this is a wrapper that interprets the XML information published for local weather stations multiple times per day ([here is Châteauguay's page, for example](https://dd.weather.gc.ca/citypage_weather/xml/QC/s0000050_e.xml)).

All Google authentication is handled via Oauth 2.0 tokens, which can be generated during testing and then refreshed automatically server-side. 

### Weather icons

The icons used by Environment Canada are pretty low resolution, and so I decided to pick a new set of icons and then match them one-to-one to the icon descriptiosn used by Environment Canada, which is no small feat! Try finding the difference between 'Light rain shower and flurries' (icon code 37) and 'A few wet flurries' (icon code 38)...

I found the description of [each icon code here](https://dd.weather.gc.ca/citypage_weather/docs/) and painstakingly matched them to erikflowers' great [SVG iconset available here](https://github.com/erikflowers/weather-icons). My indexing file is [in the project repo here](https://github.com/malcolmosh/dailypi/blob/master/Weather_icon_correspondence.xlsx) I then dropped in all SVG files as individual objects into my main SVG template and set their visibility to 'hidden'. The app scripts dynamically reveal the right weather icons depending on the forecast emitted by Environment Canada.

<div class="row">
    <div class="col-sm mt-6 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/33-cloudynight.gif" title="example image" class="img-fluid rounded z-depth-1" height="150px" %}
    </div>
    <div class="col-sm mt-6 mt-md-0">
        {% include figure.liquid path="assets/img/dailypi/night-alt-cloudy.jpg" title="example image" class="img-fluid rounded z-depth-1" height="150px" %}
    </div>
</div>
<div class="caption">
    Left to right... from low res to SVG!
</div>
