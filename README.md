This repo contains the source code for the main Dartino website available at [https://www.dartino.org](https://www.dartino.org).

## Installation instructions

1. Check that you have Ruby (Mac’s already have this):
`ruby --version`

1. We will be running everything inside bundler -- install it:
`sudo gem install bundler`

1. Initialize bundle:
`bundle install`

1. Install the Firebase CLI.

## Making changes

### Editing pages

The site source code is in markdown. Edit the .md files to make changes (e.g.,
edit `index.md` to make a change to `index.html`).


### Validating changes

Ask Jekyll to generate the site and host it locally:
`bundle exec jekyll serve`

View the site in the browser:
[`http://localhost:4000`](http://localhost:4000)

Run automatic validation to check that all links etc. work:
```
bundle exec jekyll build
bundle exec htmlproofer _site
```

## Publishing changes

### Generate a new site

The actual hosted content is in the `_site` directory. Update the content of
that site by asking Jekyll to generate: `bundle exec jekyll build`

### Publish files

The site is hosted on Firebase. After validating the output in the `_site`
directory, take these steps to deploy it to Firebase: `firebase deploy`

Check the deployment status on the admin page:
[`http://go/dartino-firebase-admin`](http://go/dartino-firebase-admin)
