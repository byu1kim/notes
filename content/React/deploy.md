+++
title = "Deploy"
weight = 10
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Netlify

#### Build command

CI= npm run build or CI=false npm run build

#### Redirect

Create file `_redirects` and put inside `public` folder.

**\_redirects**

```
/*    /index.html   200
```

---

# Github

## Deploying to SiteGround - Domain Root

The following command (run from the root folder of your project) will bundle up your app, into the build folder, ready to upload to your host.

```
npm run build
```

In order to see the production code locally, you'll need to do the following:

- Install the node "serve" package globally (for use anywhere on your machine)
  - npm install -g serve
  - Depending on your permissions you may need to run this as Administrator (Windows) or prefix it with sudo (Mac/Linux)
- Start the node server and tell it to run from your build folder
  - serve -s build

#### Uploading to your server

The next step is to upload the contents of your build folder to the public_html folder on your host server.

If you are not using client-side routing (i.e. you didn't use React Router), you should be good to go.

Note: the above steps work ONLY if you are serving your app from the domain root.

Next, we'll look at the additional configuration needed if you want to serve your app in a subdirectory on your website...

## Deploying a Routed App

By default an app that utilizes client-side routing won't work because the server is set to deliver static pages based on the URL.

To fix that, we need to instruct the server to direct all http requests back to index.html

Create a file named .htaccess in the public folder in src, then paste the following rewrite rules into it:

```
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-l
  RewriteRule . /index.html [L]
</IfModule>
```

CAUTION!
If you have a code formatter running in your editor, make sure that it does not reformat the .htaccess file. In the case of Prettier, you will need to create a .prettierignore file in the root of your project, add \*\*/.htaccess to it, save, and then restart VSCode.

With that in place, you need to re-build your app with npm run build

The build process will copy .htaccess to the build folder and then you can go ahead and upload the entire contents of the build folder to the public_html folder on SiteGround (or the host of your choice).

## Deploying to a Subdirectory

In the previous slide we uploaded our app to the root folder (public_html) of our domain.

If we want our app to live in a subdirectory, we have a couple more steps to take in order for our routes and paths to work properly:

#### Set the app homepage

Open package.json and add the following property:

"homepage": "https://example.com/directory-name",

#### Add a basename prop to your BrowserRouter

basename={'/directory-name'}

## Deploying to GitHub Pages

If you don't already have one set up, create an empty repository for your project on github, then add it as a remote for your local repo.

git remote add origin https://github.com/{your-git-name}/{repo-name}.git

Then, open your package.json and add homepage property:

```
"homepage": "https://{your-git-name}.github.io/{repo-name}",
```

We'll also need the gh-pages node module as a dev dependency in order for this to work, so run the following (in your project root directory, as always):

```
npm install --save gh-pages
```

Next, we need to add a pair of scripts in package.json (siblings to the start: and build: scripts)

```
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

The predeploy script will run automatically when we tell npm to run the deploy script.

```
npm run deploy
```

- Bundle the production version of our app into the build folder.
- Push the build folder to the gh-pages branch of your repo.
- Publish the gh-pages branch to the homepage defines in package.json

You can (and should) continue to work, commit, and push on your main branch.
