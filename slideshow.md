---
marp: true
size: 4.3
theme : gaia
<style>
:root {
--color-background: #1010101;
--color-foreground: #FFFFF;
}
</style>

---
#### How to modify the translation website into fractals website
On a high level, this is done in three stages:
1. Get familiar with how to run the given translation website 
2. Find out which file to modify 
3. Create bucket to store images to display, modify the website to display the images, deploy the website again. Detailed steps are below:

---
#### First stage
- Fork the cloud-nebulous-severless github.
- Open GCP console and create a new project.
- Start cloud shell, type 'git clone https://github.com/googlecodelabs/cloud-nebulous-serverless.git'.
- Go to the target folder by typing 'cd ~/cloud-nebulous-serverless/cloud/python'.
- Follow the code lab to deploy the original translate website.

----
#### Second stage
- Find where to modify by typing 'grep -r "My Google Translate" *'.
- This shows the target html file is in the 'template' folder, type 'cd template'.
---
#### Third stage
- Go to the 'Navigation Menu' on the left hand side of the console, click on 'Cloud Storage>Browser'. Click on 'CREATE BUCKET', type in a name and click 'CREATE'.
-  Once the bucket is created, click on the three dots on the right and 'Edit Access'. Click 'Add Principles'. Type 'allUsers' as New Principles. In the second box, search 'Storage Object Viewer' and click on it in the drop down. Click 'SAVE' and click 'ALLOW PUBLIC ACCESS'.
---
-  Click on the created bucket and upload local images to the bucket by clicking 'UPLOAD FILES'.
-  Once images are in the bucket, use the 'Copy URL' button under 'Public Access' to get the url for each image.
-  Type 'vim index.html' then 'i' to edit the html file, include images through setting 'src' as the urls obtained in the previous step. Use 'Esc' then type ':wq' to save and exit.
---
-  Go back to the previous directory by typing 'cd ..' at command line, then type 'gcloud functions deploy translate --runtime python37 --trigger-http --allow-unauthenticated' to deploy the website again. This may take a few minutes. In the generated output, click on the url with the form 'https://ZONE-PROJECT_NAME.cloudfunctions.net/translate', this should take you to the new website.
---
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
---
## Cloud Run
Continued working on same project in which Cloud Function is done
- Fork the cloud-nebulous-serverless project on github
- Activated cloudshell and cloned our project
- Changed the directory to the target folder
- Made sure that Cloud Translation API, Cloud Run and Cloud Artifact Registry APIs are enabled
- In the dockerfile, changed the python version from 2 to 3
- Deployed the translation service to Cloud Run by running the command 'gcloud run deploy translate --source . --allow-unauthenticated --platform managed'
---
- On deploying the service, we got a service URL
  >Service URL : https://translate-vr34iishva-uc.a.run.app/
- On clicking the URL or typing the URL in the browser, our application opens
- Now our application is available globally and it shows fractals of dragon, tree and triangle in high and low resolution.
---
## Marp for VS Code
 - Install VS Code Latest Version
 - In the Extensions Type Marp for VS Code
 - Click Install
 - Add a new File -> File name as Slideshow.md
 - Include all the documentation from the readme file in the Marp file
 ---
<h1 align = center>
Thank you
</h1>
<h6> Yiting Cao

Meghana Alaparthy
Chandra Likhitha Chopparapu
</h6>
 

 ---