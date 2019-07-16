# doc-pipeline-app
Step-by-step C# AWS deployment

Note: do not use command line for Elastic Beanstalk deployment

## Step 1: Fork the repository
![alt text](/assets/fork-repo.png "fork repository")

## Step 2: Clone the repository to your machine
![alt text](/assets/clone-repo.png "fork repository")


## Step 3: cd into the project directory
If any changes are necessary, checkout a new branch at this time

## Step 4: Go to your AWS Console
Navigate to the Elastic Beanstalk service.

## Step 5: Create a new application
![alt text](/assets/create-application.png "fork repository")

## Step 6: Click the link to create a new environment
![alt text](/assets/create-environment.png "fork repository")

## Step 7: Select web server environment
![alt text](/assets/select-web-server.png "fork repository")

## Step 8: Select name for web server environment
Add a custom domain if desired.

![alt text](/assets/name-webserver-env.png "fork repository")

## Step 9: Select pre-configured platform
Go with the suggested selections and click "create environment".

 ![alt text](/assets/configuration.png "fork repository")

## Step 10: Wait 3-5 minutes for environment to be created

## Step 11: Test the environment

Click the green box
![alt text](/assets/application-box.png "fork repository")

Then click the deployment link
![alt text](/assets/deployed-link.png "fork repository")

You should see a confirmation page for your deployment:
![alt text](/assets/confirmed-deployment.png "fork repository")

## Step 12: Navigate to Code pipeline in AWS Console

Click create pipeline button

![alt text](/assets/create-pipeline.png "fork repository")

## Step 13: Choose pipeline settings
Option 1: Select "new service role", AWS will auto-populate role name.

Option 2: Select "existing service role" and pick a role from the dropdown list.

Select "new service role".

![alt text](/assets/choose-pipeline-settings.png "fork repository")

## Step 14: Select source provider - Github

Connect github to your AWS

![alt text](/assets/connect-github.png "fork repository")

Select github webhooks

![alt text](/assets/webhooks.png "fork repository")

Select the correct repo and master branch

## Step 15: Build stage
Select "skip build stage"

![alt text](/assets/skip-build.png "fork repository")

Confirm skip

![alt text](/assets/skip.png "fork repository")

## Step 16: Add deploy stage
1: Select AWS Elastic Beanstalk
2: Select region
3: Select application name (click in box to see options)
4: Select environment name (click in box to see options)

![alt text](/assets/deploy-stage.png "fork repository")

## Step 17: Click "next" to review settings

Click "create pipeline".

In your command terminal from the root directory:

Check out a new branch.

touch aws-windows-deployment-manifest.json and add the following code:
```
{
    "manifestVersion": 1,
    "deployments": {
        "aspNetCoreWeb": [{
                "name": "YOURAPPNAMEHERE",
                "parameters": {
                    "appBundle": "./site",
                    "iisPath": "/",
                    "iisWebSite": "Default Web Site"
                }
            }
        ]
    }
}
```

touch a buildspec.yml and add the following code:

```
version: 0.2

phases:

build:
commands:
  - dotnet restore YOURAPPFOLDER/YOURAPPNAME.csproj
  - dotnet build YOURAPPFOLDER/YOURAPPNAME.csproj
  - dotnet publish YOURAPPFOLDER/YOURAPPNAME.csproj -o site
 artifacts:
  files:
    - YOURAPPFOLDER/site/**/*
    - YOURAPPFOLDER/aws-windows-deployment-manifest.json
```

APP and Merge to Master
