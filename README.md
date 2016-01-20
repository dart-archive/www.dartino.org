This repo contains the source code for the main Dartino website available at https://www.dartino.org.

## Installation instructions

Install Jekyll, which takes does the markdown to HTML generation:

1. Check that you have Ruby (Mac’s already have this):
`ruby --version`

1. Install Bundler:
`sudo gem install bundler`

1. Install Jekyll and the github-pages plugin (this will take a while):
`sudo gem install github-pages`

Install the Firebase CLI.

## Making changes

### Editing pages

The site source code is in markdown. Edit the .md files to make changes (e.g., edit `index.md` to make a change to `index.html`).


### Validating changes

Ask Jekyll to generate the site and host it locally:
`bundle exec jekyll serve`

View the site in the browser:
`http://localhost:4000`

## Publishing changes

### Generate a new site

The actual hosted content is in the `_site` directory. Update the content of that site by asking Jekyll to generate:
`jekyll build`

### Publish files

The site is hosted on Firebase. After validating the output in the `_site` directory, take these steps to deploy it to Firebase:
`firebase deploy`

Check the deployment status on the admin page:
`http://go/dartino-firebase-admin`
