# Setting up Snyk Webhooks with Azure Boards 

The purpose of this document is to provide guidance for configuring Snyk Webhooks to send issues found in Azure Repos to Azure Boards as issues.  

Note: This feature is currently in beta. While in this status, we may change the API and the structure of webhook payloads at any time, without notice.

## Preparing Azure DevOps
1. The first step is to set up a Personal Access Token (PAT).

<img width="302" alt="image" src="https://user-images.githubusercontent.com/89480245/162366266-abb0f562-f17a-4609-9b59-43dadd95a9ce.png">


<img width="509" alt="image" src="https://user-images.githubusercontent.com/89480245/162364346-2f1f2a18-a19b-4523-b550-35ea22a5340c.png">

<img width="386" alt="image" src="https://user-images.githubusercontent.com/89480245/162366940-8ab6ce7d-2ee8-4bf3-988f-e622a1eab570.png">

2. Make sure to save your token as you will need it later.

## Deploying the Webhook App to Heroku

3. Next, go to www.heroku.com and set up an account if you don't have one. 

4. Install Heroku CLI, clone the node app snyk-azure-boards-webhook, and push it to Heroku with the respective environment variables. Make sure to replace the variables with your proper values. 

````
$ brew install heroku/brew/heroku
$ heroku login
$ git clone https://github.com/mathiasconradt/snyk-azure-boards-webhook.git
$ cd snyk-azure-boards-webhook
$ npm install

$ heroku create
$ heroku config:set SNYK_WEBHOOKS_SECRET=xxxxxxxxxxx                     Any 16-character password
$ heroku config:set AZURE_DEVOPS_ACCESS_TOKEN=xxxxxxxxxxxx               The PAT you recorded earlier
$ heroku config:set AZURE_DEVOPS_ACCESS_USER=user@domain.com             Your username for Azure Devops 
$ heroku config:set AZURE_DEVOPS_ORGANIZATION=myorgname                  Azure Devops organization
$ heroku config:set AZURE_DEVOPS_PROJECT=myprojectname                   Azure DevOps Project
$ git push heroku main
````

## Snyk Preparation

5. Make sure you have the `outboundWebhooks` feature flag enabled in your Snyk org.

6. Use Postman to register the webhook in Snyk via API, use the Heroku app URL you received after creating the app on Heroku, with `/snyk` at the end. The secret will be the WEBHOOKS_SECRET you created above.

<img width="653" alt="image" src="https://user-images.githubusercontent.com/89480245/162373966-b4517d73-f569-4deb-b355-920ebfaac607.png">

7. Make sure that you have the following line in your Headers.  This will be your Snyk API token.

<img width="652" alt="image" src="https://user-images.githubusercontent.com/89480245/162374486-1de60916-950e-470d-8a4e-9198baa98041.png">

8. As Snyk discovers issues in the Azure repo, it will create work items in Azure Boards like below.

<img width="1334" alt="image" src="https://user-images.githubusercontent.com/89480245/162375476-27564986-e25c-4874-b521-b998660667f0.png">
