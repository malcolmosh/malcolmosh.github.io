---
layout: post
title: "DispatchPi part 1: build a local app 📟"
date: 2023-06-25 00:00:00-0500
last_modified_at: 2023-06-25 00:00:00-0500
description: #an example of a blog post with table of contents on a sidebar
tags: raspberrypi pizero
categories: DispatchPi
giscus_comments: false
related_posts: false
toc:
  beginning: true
---

> **Required for this section**
>
> - A [Google Cloud Platform](https://cloud.google.com/) (GCP) account, with billing activated.
> - An IDE for Python. I use [Visual Studio Code](https://code.visualstudio.com/).
> - A Gmail inbox created specifically for the project. Create one here: [https://mail.google.com/](https://mail.google.com/)

### Pull the code

Start by pulling the code in the `/server` folder of the [Github repo](https://github.com/malcolmosh/dispatchPi/tree/main/server) to your local machine. Here's an overview of all files:

- `main.py` contains the UI and the routing framework for our webapp built with Flask.
- `gmail_connector.py` is a python class that connects to a Gmail inbox using the Gmail API python client and extracts the relevant images for both the satellite and earth frames.
- `eink_image.py` is a python class that converts any image (even HEIC!) into a low resolution B&W image suitable for the e-ink display.
- The `fonts` folder contains a font file used to draw text onto the image.
- The `secrets` subfolder will house three files you must create:
  - `client_secret.json`, which logs you in to your GCP project
  - `flask_key.json`, a unique identifier for your flask app
  - `token.json`, which will be created by the app to authenticate you for a specific Gmail inbox. The `main.py` file will automatically refresh the authentication token if it is expired.

### Create a GCP project

Then, in your browser, **create a new GCP project** and connect it to an existing billing account.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/1%20Create%20project.png" alt="GCP project creation" caption="GCP project creation" %}

Once it's created,select it, click on the sidebar and then on “Enabled APIs and services”.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/2_API_sidebar.png" alt="API sidebar" caption="API sidebar" %}

Since this project uses the Gmail API, you will need to activate it in this project. Search for the Gmail API in the search bar at the top of the page, click on it and then click on the enable button.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/3%20Gmail%20api%20screen.png" alt="Activating the Gmail API" caption="Activating the Gmail API" %}

### Create OAuth Credentials

Next, on the left sidebar, click on “Credentials” and then click on “Create credentials” at the top of the screen.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/4.%20Create%20credentials.png" alt="Creating oauth credentials" caption="Creating oauth credentials" %}

Select “OAuth client ID” and advance to the next screen. GCP asks you to configure a consent screen that will be shown to users who wish to use your app. Click on “configure consent screen”. Select “External” under the User Type header, and then click on create. Fill in the following fields:

- App name: name it as you see fit
- User support email: select your own Google email from the dropdown
- Developer contact information: type out your own Google email again

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/5.%20Oauth%20consent%20screen%20external.png" alt="Create app consent screen" caption="Create app consent screen" %}

Click on “Save and continue”. Skip the step asking you to add app scopes by clicking “save and continue again”. Also skip the step asking you to add test users. Confirm everything is correct and then click on “Back to dashboard at the bottom of the page”.

On the summary screen of the OAuth consent screen, click on “Publish App”. Accept the warning and click confirm.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/6.%20Publish%20app.png" alt="Publish the app" caption="Publish the app" %}

Your Google OAuth consent screen is now finalized. It means we can now create the credentials we needed to access the Gmail API. Head over to the credential menus on the left, and click on create credentials > OAuth client ID.

Select “Web application” as the application type. Edit the name if you’d like to.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/7.%20OAuth%20client%20id.png" alt="Web application type" caption="Web application type" %}

### Add authorized redirects

> ##### WARNING
>
> This step is important. Doing it right will spare you many a headache.

In the authorized redirect URIs, add the two following URLs, by making sure you are not omitting any character (include the backslashes). If you encounter any problems with authentication, review these URLs carefully.

- `http://localhost:8080/oauth2callback`
- `http://localhost:8080/`

The app will only accept authentication calls originating from these URLs. The localhost address is the local flask server you will be running during tests on your local machine.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/8.%20OAuth%20client%20id%202.png" alt="Add local URLs" caption="Add local URLs" %}

Click on create. A popup will come up with your client ID and secrets. Click on “Download JSON” and store the file in your app folder, in the `secrets` subfolder. Rename it to `client_secret.json`

In the `secrets` folder, open the `flask_key.json` file. This is a custom key used to identify your flask app. It’s not strictly needed, but it’s good practice to include one. Replace the empty string with a random string with the code below:

```python
import os
os.urandom(12)
```

See this [Stack Overflow](https://stackoverflow.com/questions/34902378/where-do-i-get-secret-key-for-flask) thread for more details.

### Create a new python environment

It’s now time to generate an access token to a specific Gmail inbox by running the app code locally.

It’s good practice to create a new python environment before you start developing. I use Anaconda to manage my Python installation. You can see instructions on creating a new environment [here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

In your new python environment, start by installing the required libraries, by running the commands below. Open the `main.py` file.

Install these libraries via pip install if you’re not an Anaconda user.

```
conda install -c conda-forge google-auth
conda install -c conda-forge flask
conda install -c conda-forge google-api-core
```

Also install the required libraries via the requirements.txt file, by running the following command in your terminal:

```
pip install -r requirements.txt
```

### Generate an Access Token

Now, run the app by clicking on the run button in the top right of the Vscode interface. You can also run your app with the following command :

```
flask --APP_LOCATION main run
```

You should now be able to view the app in your browser at the following address : `localhost:8080`. The homepage of the app looks like this.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/10.%20Localhost%20view.png" alt="View app local homepage" caption="View app local homepage" %}

The first link displays the latest image received in the Gmail inbox intended for the satellite frame, while the second image displays the image intended for the earth frame.

Click on **“Test the auth flow directly”** to connect your GCP app with a specific inbox. Select your new Gmail address in the list of accounts that comes up.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/11.%20choose%20account.png" alt="Select your Gmail address" caption="Select your Gmail address" %}

A warning screen comes up, explaining the app has not been verified by Google. Continue undeterred by clicking on “Access (APPLICATION_NAME)” in the bottom of the screen.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/12.%20Unverified%20app.png" alt="Warning screen" caption="Warning screen" %}

On the consent screen, accept the app’s terms and click confirm.

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/13.%20consent%20screen.png" alt="Accept app terms" caption="Accept app terms" %}

You will be redirected to the homepage of the app. You will notice a new `token.json` file has been created in your secrets folder. This token will allow the app to read images from your Gmail inbox, while the `client_secret.json` file we downloaded earlier instructs the app to use your GCP project credentials to call the Gmail API. Now that this authorization step is done, it will not be needed anymore. You may notice the token contains an expiry date : the app is able to refresh this token dynamically without requiring user intervention.

Your app should now work properly. Test this by clicking again on “Test the auth flow directly”. If all goes well, the index page should simply reload.

**Before continuing further**, you need to send some emails to the shared inbox. The app filters images shown to each frame. The satellite frame sees images from all senders except its own email address, while the earth frame will only display images from the satellite owner's email. You need the app which email address(es) the satellite frame owner uses. This is specified in **line 66** of the `server/main.py` file (`satellite_emails`).

### Queueing images (new !!)

The app allows you to display images in the order they are received, at the rate of one image per calendar day (24 hours). This is accomplished through a "First-in, first-out" queue (or FiFo). The Gmail connector first lists all emails relevant for a specific frame, trims the list to the length of the queue, and then iterates over the list in reverse. The oldest image will be assigned the first place in the queue, whereas the last image will be given the last spot in the queue. As images are appended to the queue, they are given a display date. The frame will always display the last image received, even if the display date is in the past. The queues are generated in json files stored in a `queues` subfolder that is created dynamically when you execute the app.

If you would rather not use the queue feature, you can use the `gmail_connector_no_queue.py` script instead. Just be sure to call the class properly, as it takes slightly different options on initialization.

### Test the app

Send two emails with image attachments to the shared Gmail inbox. One from any email address whatsoever, and the other from the email you just added on line 69 of the file. Try adding text in the body: the frame will draw your text onto the image! Wait a minute for all emails to be received in the inbox.

You can now test the app’s ability to pickup images for both the satellite frame and the earth frame. With the app still running, click on the first two links to check everything works properly. If everything works, you should be a rasterized image like this. Note that on dark backgrounds, the black superimposed email body text could be somewhat hidden. A future app version may include white text!

{% include scripts/image_with_caption.liquid src="/assets/img/dispatchpi/tutorial_image/14.%20View%20earth%20frame.png" alt="Image preview rasterized, black and white" caption="Image preview rasterized, black and white" %}

Your app now works locally. It’s time to upload it to the cloud!
