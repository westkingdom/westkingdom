## API Overview

Google provides a large number of APIs that can be accessed
to configure or customize Google services.  The API admin
console can be accessed through the [Google Developers Console](https://console.developers.google.com).

All API configuration is done through a Google Apps project.
I created a general holding project for all West Kingdom API
use:

- **Name:** West Kingdom
- **Project ID:** west-web-1066

At this point, I do not know if there will ever be any point
in having more than one project.

### Enabled APIs

Before using Google APIs, it is necessary
to [enable the ones you are going to use](https://console.developers.google.com/project/west-web-1066/apiui/api).  The one we need
the most for our first project is the Groups Settings API.
I turned on a number of others that looked like they might
be of some use; a few other services were on by default.

The initial list of enabled APIs includes:

- Admin SDK       
- Analytics API       
- Apps Activity API       
- Audit API       
- BigQuery API  
- Calendar API      
- Debuglet Controller API     
- Gmail API       
- Google Cloud SQL      
- Google Cloud Storage      
- Google Cloud Storage JSON API       
- Google Webmaster Tools API      
- Google+ API       
- Google+ Hangouts API      
- Groups Migration API      
- Groups Settings API       

### API Keys

The Google PHP API documentation includes [instructions on 
how to create an API key](https://developers.google.com/api-client-library/php/auth/api-keys).

I created a server key, and configured it to be usable from
westkingdom.org on Site5, and hydrogen.greenknowe.org, a personal
VPS on digital ocean (only in case I needed to for testing purposes;
I don't expect to actually access Google APIs from here with any
regularity).  I would have included my home server as well, except
it has a dynamic IP address, so this would not be useful.

The API must be kept secret, so it cannot be recorded here.
It can be found in the [credentials section](https://console.developers.google.com/project/west-web-1066/apiui/credential)
of the Google Deveper Console.

### PHP API

- [PHP Google Apps API](https://developers.google.com/api-client-library/php/)
- [API Exporer](http://developers.google.com/apis-explorer)

### Rough notes on what we are going to do with the API

First step is to parse the YAML file and set up some scaffolding to work with it.

Second step is to set up a cache for the YAML file, so that we know what changed, and can operate only on the deltas each time our method is called.

Third step is to just figure out how to authenticate and call the Google API.

Fourth step is to figure out how to create a group, and add members to it.

Fifth step is to put it together, so that when you call our update API with a new YAML file, it calls all of the Google APIs to update the groups and memberships based on the deltas.

Sixth step is to figure out how to make aliases for groups, and then also process the aliases sections of the YAML file when it changes.
