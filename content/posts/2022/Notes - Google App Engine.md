---
title: "Notes   Google App Engine"
date: 2022-07-28T15:33:54-07:00
categories:
  - Tech
  - Cloud
tags:
  - App Engine
  - Go
draft: false
---

## App Engine - Go
### Step by Step
1. Create A Project
   * In the [Google Cloud Console](https://console.cloud.google.com/welcome), on the project selector page, select or create a Google Cloud project.
   * Example: Create a new project: Name: `MyIP`; Project ID: `myip-123`

2. Enable Billing
   * Make sure that billing is enabled for your Cloud project.

3. Enable the Cloud Build API

4. Create an App Engine application in the project
   * Go to [App Engine Dashboard](https://console.cloud.google.com/appengine/start/create?walkthrough_id=resource-manager--create-project&project=myip-123)
   * Select project `MyIP`
   * Create Application > Select Region `West1` > Language: Go; Environment: Standard

5. Install and initialize the `Google Cloud CLI`
   * Download and install [gcloud CLI](https://cloud.google.com/sdk/docs/install) tools
   * Initialize your SDK: `gcloud init`
   * If you didn't create project in the first step, you can create it from command line: `gcloud app create --project=[YOUR_PROJECT_ID]`. When prompted, select the region where you want to locate your App Engine application.
   * Run the following command to install the gcloud component that includes the App Engine extension for Go 1.12+: `gcloud components install app-engine-go`

6. Deploy to App Engine 
   * Create Go App Engine project in your computer
   * Deploy to App Engine: `gcloud app deploy`

### How to serve a static website?
File and folder structure:
```
MyPrj
  +-- app.yaml
  +-- www
        +-- favicon.ico
        +-- index.html
        +-- static
              +-- app.css
              +-- poster.jpg
              +-- background.jpg
```

Here is `app.yaml`. 
```
runtime: go116

handlers:
  - url: /
    static_files: www/index.html
    upload: www/index.html

  - url: /(.*)
    static_files: www/\1
    upload: www/(.*)
```

index.html
```
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="static/app.css">
  <title>App Static Site</title>
  <style>
    body {
      background-image: url('static/background.jpg');
    }
  </style>
</head>
<body>
...
</body>
</html>
```

### Where can I find APP Engine logs?
https://console.cloud.google.com/logs

## Links
* https://cloud.google.com/go/docs
* https://cloud.google.com/go/docs/setup
* https://cloud.google.com/appengine/docs/standard/go/create-app
* [Google Cloud - Go Examples](https://github.com/GoogleCloudPlatform/golang-samples)