## PHP API

The PHP Google Apps API is here:

https://developers.google.com/api-client-library/php/

## Rough notes on what we are going to do with the API

First step is to parse the YAML file and set up some scaffolding to work with it.

Second step is to set up a cache for the YAML file, so that we know what changed, and can operate only on the deltas each time our method is called.

Third step is to just figure out how to authenticate and call the Google API.

Fourth step is to figure out how to create a group, and add members to it.

Fifth step is to put it together, so that when you call our update API with a new YAML file, it calls all of the Google APIs to update the groups and memberships based on the deltas.

Sixth step is to figure out how to make aliases for groups, and then also process the aliases sections of the YAML file when it changes.
