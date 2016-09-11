Deploying changes to the West Kingdom website is a two-step process:

- Deploy from repository to staging site
- Deploy from staging site to live site

## Deploying to the staging site

Any change that is pushed to the master branch of the [West Kingdom Website](https://github.com/westkingdom/website) is automatically deployed to the staging site whenever the tests pass.

This is managed by [CircleCI](https://circleci.com/gh/westkingdom/website), which is configured with a private ssh key whose public key is installed on the staging site only.  The [deployment section of our circle.yml file](https://github.com/westkingdom/website/blob/master/circle.yml#L29) contains a directive to push and install the code. Circle runs this command at the end of the test run if all tests pass.

Once the code has been deployed to the staging site, you should manually inspect it to ensure that everything looks okay.

There is an added login dialog protecting the staging site.  This added authentication is only to discourage spiders from indexing the site; it is not for security, as the staging site is a sanitized copy of the live site (user email addresses and passwords are scrambled).  The username for the authentication dialog is WEST and the password is WEST.

## Deploying to the live site

An automated way to deploy from the staging site to the live site is tbd.

In the interim, running the command from the circle.yml file on @wkweb.live will do the trick.  Doing this requires that your ssh public key be installed on the live site.

Ideally, we would prefer to have a mechanism that would allow authorized individuals to publish from stage to live without an ssh key.
