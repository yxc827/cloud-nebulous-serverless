# OUCS Team 2's Guide
## Cloud Functions (Done by Yiting Cao)
### How to modify the translation website into fractals website
On a high level, this is done in three stages: 1. Get familiar with how to run the given translation website 2. Find out which file to modify 3. Create bucket to store images to display, modify the website to display the images, deploy the website again. Detailed steps are below:
#### First stage
- Fork the cloud-nebulous-severless github.
- Open GCP console and create a new project.
- Start cloud shell, type 'git clone https://github.com/googlecodelabs/cloud-nebulous-serverless.git'.
- Go to the target folder by typing 'cd ~/cloud-nebulous-serverless/cloud/python'.
- Follow the [code lab](https://codelabs.developers.google.com/codelabs/cloud-nebulous-serverless-python-gcf?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_wes_aap-serverless_nebservgcf_sms_201020&utm_content=-#0) to deploy the original translate website.
#### Second stage
- Find where to modify by typing 'grep -r "My Google Translate" *'.
- This shows the target html file is in the 'template' folder, type 'cd template'.
#### Third stage
- Go to the 'Navigation Menu' on the left hand side of the console, click on 'Cloud Storage>Browser'. Click on 'CREATE BUCKET', type in a name and click 'CREATE'.
-  Once the bucket is created, click on the three dots on the right and 'Edit Access'. Click 'Add Principles'. Type 'allUsers' as New Principles. In the second box, search 'Storage Object Viewer' and click on it in the drop down. Click 'SAVE' and click 'ALLOW PUBLIC ACCESS'.
-  Click on the created bucket and upload local images to the bucket by clicking 'UPLOAD FILES'.
-  Once images are in the bucket, use the 'Copy URL' button under 'Public Access' to get the url for each image.
-  Type 'vim index.html' then 'i' to edit the html file, include images through setting 'src' as the urls obtained in the previous step. Use 'Esc' then type ':wq' to save and exit.
-  Go back to the previous directory by typing 'cd ..' at command line, then type 'gcloud functions deploy translate --runtime python37 --trigger-http --allow-unauthenticated' to deploy the website again. This may take a few minutes. In the generated output, click on the url with the form 'https://ZONE-PROJECT_NAME.cloudfunctions.net/translate', this should take you to the new website.
### How to use GCP to display the fractals website
- Go to [Google Cloud Console](https://console.cloud.google.com/home/dashboard?project=graphic-abbey-326020), click on the cloud shell button on the upper right to start up a terminal.
- In the cloud shell command line, type 'gcloud init' at the command line, then go to the right directory by typing 'cd ~/cloud-nebulous-serverless/cloud/python'.
- Then deploy by typing 'gcloud functions deploy translate --runtime python37 --trigger-http --allow-unauthenticated', this might take a few minutes
- In the generate output, find the segment similar to 
  >httpsTrigger:
  >securityLevel: SECURE_OPTIONAL
  >url: https://us-central1-graphic-abbey-326020.cloudfunctions.net/translate
- Click on the url, or type the url in your browser.
- You should now be able to see the content on the fractals website.

## Cloud Run
Continued working on same project in which Cloud Function is done
- Fork the cloud-nebulous-serverless project on github
- Activated cloudshell and cloned our project
- Changed the directory to the target folder
- Made sure that Cloud Translation API, Cloud Run and Cloud Artifact Registry APIs are enabled
- In the dockerfile, changed the python version from 2 to 3
- Deployed the translation service to Cloud Run by running the command 'gcloud run deploy translate --source . --allow-unauthenticated --platform managed'
- On deploying the service, we got a service URL
  >Service URL : https://translate-vr34iishva-uc.a.run.app/
- On clicking the URL or typing the URL in the browser, our application opens
- Now our application is available globally and it shows fractals of dragon, tree and triangle in high and low resolution
## Marp for VS Code
 - Install VS Code Latest Version
 - In the Extensions Type Marp for VS Code
 - Click Install
 - Add a new File -> File name as Slideshow.md
 - Include all the documentation from the readme file in the Marp file

| :boom: ALERT!!             |
|:---------------------------|
| This repo will soon be relocating to [GoogleCloudPlatform](https://github.com/GoogleCloudPlatform) as we better organize these code samples! Stay tuned as more info is coming soon. |


# Nebulous Google Cloud serverless &amp; API sample applications
### Run the same apps locally, on App Engine, Cloud Functions, or Cloud Run

## Description

This is the repo for a set of sample apps and corresponding codelabs (self-paced, hands-on tutorials) demonstrating how to call Google APIs from [Google Cloud serverless compute platforms](https://cloud.google.com/serverless) (App Engine, Cloud Functions, Cloud Run). More on each platform below. Working wth [Cloud APIs](cloud) differs from [non-Cloud Google APIs](noncloud), so that is how the samples are organized. The common aspect of all of sample apps is that they can be run locally or deployed to any of the 3 platforms without code changes (all done in configuration).


## Hosting options

- **[App Engine](https://cloud.google.com/appengine)** (standard environment) — source-based application deployments (app-hosting in the cloud; "PaaS")
    - _App Engine_ is for users who wish to deploy a traditional (but not containerized) web stack (LAMP, MEAN, etc.) application direct from source code.
- **[Cloud Functions](https://cloud.google.com/functions)** — cloud-hosted functions or microservices ("FaaS"), possibly event-driven
    - If your app is relatively simple, is a single function, or perhaps some event-driven microservice, _Cloud Functions_ may be the right platform for you.
- **[Cloud Run](https://cloud.run)** — fully-managed serverless container-hosting in the cloud ("CaaS") service
    - If your apps are containerized or you have containerization as part of your software development workflow, use _Cloud Run_. Containers free developers from any language, library, or binary restrictions with App Engine or Cloud Functions.

A "fourth" product, [App Engine flexible environment](https://cloud.google.com/appengine/docs/flexible), which sits somewhere between App Engine standard environment and Cloud Run, is out-of-scope for these sample apps at this time.

When running on App Engine or Cloud Functions, the Python runtime supplies a default web server (`gunicorn`), but for Node.js, [Express.js](http://expressjs.com) was selected. No default servers are available at all for Cloud Run, so Python developers can either run the [Flask](https://flask.palletsprojects.com) development server (default) or self-bundle `gunicorn` (per your configuration). All Node.js deployments specify Express.js.


## Inspiration and implementation

These samples were inspired by a [user's suboptimal experience](https://www.mail-archive.com/google-appengine@googlegroups.com/msg94549.html) trying to create a simple App Engine app using a Cloud API. This was followed-up with the realization that there aren't enough examples showing users how to access non-Cloud Google APIs from serverless, hence *those* samples.

The table below outlines the development languages, supported versions, deployments tested, and selected web frameworks (whose bundled development servers are used for running locally):

Language | Versions | Deployment | Framework
--- | --- | --- | ---
Python|2.7|local, cloud|Flask
Python|3.6+|local, cloud|Flask
Node.js|10, 17|local|Express.js
Node.js|10, 12, 14, 16|cloud|Express.js


## Cost

While many Google APIs can be used without fees, use of GCP products &amp; APIs is _not_ free. Certain products do offer an ["Always Free" tier](https://cloud.google.com/free/docs/gcp-free-tier#free-tier-usage-limits) which you have to exceed in order to be billed. Reference any relevant pricing information linked below before doing so.

- [App Engine](https://cloud.google.com/appengine/pricing)
- [Cloud Functions](https://cloud.google.com/functions/pricing)
- [Cloud Run](https://cloud.google.com/run/pricing)
- [GCP general pricing](https://cloud.google.com/pricing)
- [GCP pricing calculator](https://cloud.google.com/products/calculator)

When enabling services, you may be asked for an active billing account which requires a financial instrument such as a credit card. Reference relevant pricing information before doing so. While Cloud Functions and Cloud Run share a similar Always Free tier and pricing model, App Engine is slightly different.

Furthermore, deploying to GCP serverless platforms incur [minor build and storage costs](https://cloud.google.com/appengine/pricing#pricing-for-related-google-cloud-products). [Cloud Build](https://cloud.google.com/build/pricing) has its own free quota as does [Cloud Storage](https://cloud.google.com/storage/pricing#cloud-storage-always-free). For greater transparency, Cloud Build builds your application image which is then sent to the [Cloud Container Registry](https://cloud.google.com/container-registry/pricing), or [Artifact Registry](https://cloud.google.com/artifact-registry/pricing), its successor; storage of that image uses up some of that (Cloud Storage) quota as does network egress when transferring that image to the service you're deploying to. However you may live in region that does not have such a free tier, so be aware of your storage usage to minimize potential costs. (You may look at what storage you're using and how much, including deleting build artifacts via [your Cloud Storage browser](https://console.cloud.google.com/storage/browser).)

More specific cost information for each sample is available in their respective README files.


## Testing

Each app has its own testing battery; refer to each sample's folder to learn about implemented tests.
