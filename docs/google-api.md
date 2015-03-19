## API Overview

Google provides a large number of APIs that can be accessed
to configure or customize Google services.  The API admin
console can be accessed through the [Google Developers Console](https://console.developers.google.com).

All API configuration is done through a Google Apps project.
I created a general holding project for all West Kingdom API
use:

- **Name:** West Kingdom
- **Project ID:** west-kingdom-1066

At this point, I do not know if there will ever be any point
in having more than one project.  It is possible to create
multiple service accounts in one project, and each service
account can have different permissions.  See below.

### Enabled APIs

Before using Google APIs, it is necessary
to [enable the ones you are going to use](https://console.developers.google.com/project/west-web-1066/apiui/api).
I enabled the Admin SDK, the Groups Settings API and 
Groups Migration APIs to start with.

### API Keys

You can skip this part, and just create service accounts
using OAuth Keys, per the instructions in the next section.

The Google PHP API documentation includes [instructions on 
how to create an API key](https://developers.google.com/api-client-library/php/auth/api-keys).

I created a server key, and configured it to be usable from
westkingdom.org on Site5, and hydrogen.greenknowe.org, a personal
VPS on digital ocean (only in case I needed to for testing purposes;
I don't expect to actually access Google APIs from here with any
regularity).  I would have included my home server as well, except
it has a dynamic IP address, so this would not be useful.

The API key must be kept secret, so it cannot be recorded here.
It can be found in the [credentials section](https://console.developers.google.com/project/west-web-1066/apiui/credential)
of the Google Deveper Console.  We don't need the API key,
though; using just OAuth Keys works fine.

### OAuth Keys

If you have ssh access into the westkingdom.org web server,
you can just copy the contents of /etc/google-api on the
server to ~/.google-api on your development machine.  Read
on if you need to create your own keys, or modify the server
keys to enable more services.

API keys are not sufficient to access certain user data through
the Google APIs; in order to call these restricted functions, you
need an OAuth Key.  These are also generated in the
[credentials section](https://console.developers.google.com/project/west-web-1066/apiui/credential)
of the Google Deveper Console.

Click on the "Create new ClientID" button, and create a client
ID for a service account.  The private key will be downloaded
to your computer; you must manage this key yourself.  Store it
in the same directory as the API key file 'server.key' (i.e.,
~/.google-api or /etc/google-api).  Make sure to give this
file restrictive permissions (e.g. chmod 600).

Next, create a file
in the same directory called 'service-account.yaml'.  It
should contain the following information:
```
client-id: 278...-keq...apps.googleusercontent.com
email-address: 278..-keq...@developer.gserviceaccount.com
key-file: West Kingdom-ce8e17968820.p12
key-file-password: notasecret
delegate-user-email: user-with-superadmin@westkingdom.org
```
In your copy of this file, you will want to replace the
'client-id' and 'email-address' with the information for
your service account shown in the [credentials section](https://console.developers.google.com/project/west-web-1066/apiui/credential)
of the Google Deveper Console. The key-file should
contain the filename that you saved your private key as;
the key-file-password is necessary only if you changed the
default.  The delegate user email address is very important;
your service account will be granted all of the priviledges
of the user that you name here.

It is also necessary that your service account be
authorized for the services it needs to access on the
[Manage API client access](https://admin.google.com/westkingdom.org/AdminHome?chromeless=1#OGX:ManageOauthClients) page of
the admin console for your organization.  Please see the
Google documentation on [Using OAuth 2.0 for Server to Server Applications](https://developers.google.com/accounts/docs/OAuth2ServiceAccount).

See the [Google API service account documentation](https://developers.google.com/accounts/docs/OAuth2?hl=en_US#serviceaccount) and the [PHP API client library service account documentation](https://developers.google.com/api-client-library/php/auth/service-accounts) for more
information.  There is [sample code for connecting with an
OAuth key](https://github.com/google/google-api-php-client/blob/master/examples/service-account.php) available,
but you won't need it; our wrapper API takes care of this.

### PHP API

- [PHP Google Apps API](https://developers.google.com/api-client-library/php/)
- [API Exporer](http://developers.google.com/apis-explorer)
- [Manage Groups](https://developers.google.com/admin-sdk/directory/v1/guides/manage-groups)

### Rough notes on what we are going to do with the API

First step is to parse the YAML file and set up some scaffolding to work with it.

Second step is to set up a cache for the YAML file, so that we know what changed, and can operate only on the deltas each time our method is called.

Third step is to just figure out how to authenticate and call the Google API.

Fourth step is to figure out how to create a group, and add members to it.

Fifth step is to put it together, so that when you call our update API with a new YAML file, it calls all of the Google APIs to update the groups and memberships based on the deltas.

Sixth step is to figure out how to make aliases for groups, and then also process the aliases sections of the YAML file when it changes.
