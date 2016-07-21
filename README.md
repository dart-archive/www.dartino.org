This repo contains the source code for the main Dartino website available at [https://www.dartino.org](https://www.dartino.org).

## Configuration instructions needed to make and verify edits

1. Check that you have Ruby (Mac’s already have this):<br>
`ruby --version`

1. We will be running everything inside [bundler](http://bundler.io/) -- install
it:<br>
`sudo gem install bundler`

1. Initialize bundler:<br>
`bundle install`

1. Install the Firebase CLI:<br>
`npm install -g firebase-tools` ([details](https://www.firebase.com/docs/hosting/command-line-tool.html))

## Making changes

### Editing pages

The site source code is in markdown, from which the site html is generated. Edit
the .md files to make changes (e.g., edit `index.md` to make a change to
`index.html`).

Note that URLs are automatically *prettified* so that `foo.md` becomes `/foo/`,
and `/guides/compile.md` becomes `/guides/compile/`.


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

The site is automatically deployed to Firebase hosting whenever new files are
pushed to Github in the master branch. If you want to edit but not deploy, push
your changes to a branch, and then merge to master when you are ready to deploy.

The Travis CI job performs the following tasks:

1. Builds the site:<br>
`bundle exec jekyll build`

1. Checks links:<br>
`bundle exec htmlproofer _site`

1. Deploys to Firebase:<br>
`firebase deploy`

[![Deployment status -](https://travis-ci.org/dartino/www.dartino.org.svg?branch=master)](https://travis-ci.org/dartino/www.dartino.org)

You can check the Firebase deployment status on the Firebase admin page:
[`http://go/dartino-firebase-admin`](http://go/dartino-firebase-admin)
