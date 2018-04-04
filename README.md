### gcloud cli commands
```
# deploy
gcloud app deploy
gcloud app deploy --project [project_id]

# available projects
gcloud projects list

# current project
gcloud config list

# change configuration/and or project
gcloud init

or

gcloud init --skip-diagnostics

-------------------------------
Settings from your current configuration [default] are:
core:
  account: some@google.account
  disable_usage_reporting: 'True'
  project: some-project-id

Pick configuration to use:
 [1] Re-initialize this configuration [default] with new settings
 [2] Create a new configuration
Please enter your numeric choice:
-------------------------------
```

### SSH
get the gcloud ssh command from the console page  
App Engine -> Instances -> SSH -> "View gcloud command"
SSH'ing into a running app, logs you into the container host
```
# list the running containers
docker ps

#shell into a running app container (has the really long image name)
docker exec -it [container_id] /bin/bash
```

### Useful links
https://cloud.google.com/datastore/docs/concepts/entities

https://cloud.google.com/appengine/docs/flexible/php/runtime

https://cloud.google.com/sdk/gcloud/reference/app/deploy

https://cloud.google.com/appengine/docs/flexible/php/configuring-your-app-with-app-yaml

https://cloud.google.com/datastore/docs/reference/libraries

https://cloud.google.com/appengine/docs/flexible/php/using-cloud-datastore

https://cloud.google.com/community/tutorials/run-laravel-on-appengine-flexible

https://developers.google.com/gmail/api/v1/reference/users/messages/list
https://developers.google.com/gmail/api/v1/reference/users/history/list

https://developers.google.com/gmail/api/guides/push

https://developers.google.com/gmail/api/guides/sync

https://developers.google.com/gmail/api/guides/filter_settings#actions

https://cloud.google.com/appengine/docs/flexible/php/runtime#customizing_nginx

https://developers.google.com/api-client-library/php/guide/batch

### Enable billing for a project
Go to the Google Cloud Platform Console.
From the projects list, select the project to re-enable billing for.
Open the console left side menu and select Billing .
Click Link a billing account.
Click Set account.

libraries
composer require google/cloud:^0.56
composer require google/apiclient:^2.0

### Auth for api access with a service account
From the google api client.php file:
```
// These conditionals represent the decision tree for authentication
//   1.  Check for Application Default Credentials
//   2.  Check for API Key
//   3a. Check for an Access Token
//   3b. If access token exists but is expired, try to refresh it
```
https://developers.google.com/analytics/devguides/reporting/core/v3/quickstart/web-php
https://developers.google.com/gmail/api/quickstart/php
https://cloud.google.com/docs/authentication/getting-started
https://cloud.google.com/iam/docs/understanding-service-accounts
https://cloud.google.com/datastore/docs/reference/libraries#setting_up_authentication
https://cloud.google.com/docs/authentication/production#providing_credentials_to_your_application

PHP: https://developers.google.com/api-client-library/php/start/get_started
I added a .gitignored .secrets dir and pass the file into the environment using docker-compose

```
docker-compose.yml
...
environment:
  #app engine service credentials path
  - GOOGLE_CLOUD_PROJECT=appengineId

  #relative to php-fpm volume
  - GOOGLE_APPLICATION_CREDENTIALS=/var/www/.secrets/gae_api_service_creds.json
...
```

Once you have access to the apis you can use the apis to interact with user data, BUT you need user authorization  to get a refresh/access token
Python: https://developers.google.com/gmail/api/auth/web-server

### Testing and Deployment (flexible aka docker)
PHP: https://cloud.google.com/appengine/docs/flexible/php/testing-and-deploying-your-app
Python: https://cloud.google.com/appengine/docs/flexible/python/testing-and-deploying-your-app


### Dev with datastore
https://googlecloudplatform.github.io/google-cloud-php/#/docs/cloud-datastore/v1.2.2/datastore/datastoreclient


### Docker images - app engine
I'd like to use my own vanilla based images with app_engine, but if I end up jumping through too many hoops and I decide to use app_engine images, I can pull these:
https://console.cloud.google.com/gcr/images/google-appengine?project=google-appengine



### Gmail API metadata - header names
If you only want certain fields back from Users.messages.get() use "metadata"
```
"metadata": Returns only email message ID, labels, and email headers.
```

GmailApi message body is base64url encoded  
https://developers.google.com/gmail/api/v1/reference/users/messages/get#request

**BUT**  

I couldn't find a list of header names. Turns out, you can see them by using the api form on the right

requires a gmail message id and oauth2 permission for the api to execute on said account
https://developers.google.com/gmail/api/v1/reference/users/messages/get
