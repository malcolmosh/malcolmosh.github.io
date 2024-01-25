---
layout: post
title: "DashboardPi tutorial: display tasks, events and weather 🗓️"
date: 2024-01-29 00:00:00-0500
last_modified_at: 2024-01-29 00:00:00-0500
description: #an example of a blog post with table of contents on a sidebar
tags: raspberrypir
categories: DashboardPi
published: false
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

## What it is

Dashboard that displays events, grocery list and weather
Runs on a Raspberry Pi Zero W. Some adaptation may be necessary to convert to other boards. Waveshare 7.5 " e-ink screen (black and white).
Through API calls to Google Calendar & Google Tasks, as well as through a python library to pull environment canada weather conditions and forecasts
Pretty modular since the template is a SVG file.
The SVG file serves as the graphical canvas. Since a SVG file can be explored in code, specific elements are swapped out and updated.  
Connected to a PiSugar battery. With three daily refreshes of 3 minutes, lasts about 2 weeks. You could also just plug it permanently. Uses little power.
Weather icons from X library matched to Environment Canada weather icons (low res)

Follow the steps described in DispatchPi to test the app locally and then deploy to CloudRun. Feel free to open an issue on the GH repo if you are encountering any difficulties.

## An SVG template

Use boxysvg to preserve attribute names.

parameters to tune :
build token files
select coordinates
tinker with language

editing the svg template was fussy. use boxysvg to preserve class names
the layer names in the svg are used in the svg updater script

## Storing env variables

secrets management : everything works through the .env file

## External battery

PiSugar2

Works with PiSugar 2 battery (https://www.pisugar.com/)

Script to wake up the raspberry Pi at specific moments and run a script on startup
Try to pull dashboard from URL
Otherwise display local image

Disable bluetooth : https://di-marco.net/blog/it/2020-04-18-tips-disabling_bluetooth_on_raspberry_pi/

Custom scripts to wake 3 times a day.

Could get better battery life with a Pico W or even an ESP32, but that's a project for another day...

## Deploy easily with Cloud Run and Github

To simplify the app's web development, I'm using continuous deployment from the Github repo to Google Cloud Platform's Cloud Run service. This means each time I push a code change to the repo, Github notifies Cloud Run of the update, and Cloud Run then rebuilds the docker image before hosting it anew. This is a huge timesaver, since it avoids fiddling with GCP's command line interface in search of the exact command to build and tag a new image.

See tutorial here: https://cloud.google.com/run/docs/quickstarts/deploy-continuously

I'm not following best practice for secrets management since I'm simply storing environment variables directly in Cloud Run, without using Google's secret manager. Nevertheless, the app works well and no tokens are exposed on the endpoint.
