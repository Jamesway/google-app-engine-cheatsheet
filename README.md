## Using Google Apis in App Engine

One of the benefits of running in gae is that you don't have to do the oauth2 step when accessing google apis. This is helpful if you want to store secrets in google cloud datastore, so you don't have to pass them app.yaml and expose them to version control.

Docs for Google Cloud are a little "spread out"

```
eg getting a json string of secrets from cloud datastore

class SecretStore
{

    const GCDS_KIND = 'somekind';
    const GCDS_ID = 'someid';

    public static function initEnvVars() {

        $datastore = new DatastoreClient(['projectId' => $_ENV['GOOGLE_CLOUD_PROJECT']]);

        $key = $datastore->key(self::GCDS_KIND, self::GCDS_ID);

        return $datastore->lookup($key);
    }
}
```

### gcloud cli commands
```
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

### Useful links
https://cloud.google.com/appengine/docs/flexible/php/runtime

https://cloud.google.com/sdk/gcloud/reference/app/deploy

https://cloud.google.com/appengine/docs/flexible/php/configuring-your-app-with-app-yaml

https://cloud.google.com/datastore/docs/reference/libraries

https://cloud.google.com/appengine/docs/flexible/php/using-cloud-datastore

https://cloud.google.com/community/tutorials/run-laravel-on-appengine-flexible


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
https://cloud.google.com/docs/authentication/getting-started
https://cloud.google.com/iam/docs/understanding-service-accounts
https://cloud.google.com/datastore/docs/reference/libraries#setting_up_authentication
https://cloud.google.com/docs/authentication/production#providing_credentials_to_your_application

PHP: https://developers.google.com/api-client-library/php/start/get_started
I added a .gitignored .secrets dir and pass the file into the environment using docker-compose

docker-compose.yml
```
#app engine service credentials path
- GOOGLE_CLOUD_PROJECT=appengineId
#relative to php-fpm volume
- GOOGLE_APPLICATION_CREDENTIALS=/var/www/.secrets/gae_api_service_creds.json
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
