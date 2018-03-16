## Using Google Apis in App Engine

One of the benefits of running in gae is that you don't have to do the oauth2 step when accessing google apis. This is helpful if you want to store secrets in google cloud datastore, so you don't have to pass them app.yaml and expose them to version control.

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


gcloud Commands
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
