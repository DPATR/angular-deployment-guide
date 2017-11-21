[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Angular Deployment Guide 

Use this guide to deploy Angular apps

## Prerequisites

-   Angular app ready for deployment

## Objectives

By the end of this, developers should be able to:

- Deploy an Angular app to GitHub Pages (easiest)
- Deploy an Angular app to Heroku
- Deploy an Angular app to Amazon S3 (hardest)

## Deploying to Gitub Pages
 - If you have an Angular app created using Angular CLI, deploying to Github Pages should be straightforward
1. Create a new github repository, if you don't already have one for your application
1. Add the remote of your new repository to your local project:
    - `git remote add origin NEW-REPO-URL.git` <-- or replace `origin` with something of your choosing`
1. Install this npm package: https://github.com/angular-buch/angular-cli-ghpages
    - `npm i -g angular-cli-ghpages`
1. You will have to build your project so that angular-cli-ghpages pushes your `dist/` folder to `gh-pages`. You can do this by running the following commands:
    - ng build --prod --base-href "https://<YOUR-GITHUB-USERNAME>.github.io/<YOUR-REPO-NAME>/"` <-- don't forget that backslash
    - angular-cli-ghpages --repo=https://github.com/<YOUR-GH-USERNAME>/<YOUR-GH-REPO-NAME>.git --no-silent  
    - More usage and options available [here](https://github.com/angular-buch/angular-cli-ghpages)
1. Visit your github pages url
1. Profit!

### Troubleshooting
- There's no log for github pages builds, unfortunately.
- Got 404 errors? Ensure your base href is appropriate and that the assets can be found.
- Nothing at your github pages URL? Check your github.com repo's settings and ensure everything went smoothly during the deploy. From the Settings page, scroll down to the bottom and there's a Github Pages section that should have a green check mark if everything went well. The github pages url is also displayed here

## Deployment on Heroku
  - This guide assumes you are deploying a static, standalone front-end Angular application that will connect to a separate back-end application somewhere on another host. Ensure you replace the `"origin"` key below with the location of your api, and ensure your proxy mountpoint, `"/api"`, is what you want.
1. `cd` into your existing Angular project, or create a new one using the Angular CLI
1. Create your app on Heroku using `heroku create`
1. Add a file to the root directory of your project called `static.json` with the following contents:
    ```JSON
    {
      "root": "dist/",
      "routes": {
        "/**": "index.html"
      },
      "proxies": {
        "/api/": {
          "origin": "https://YOUR_API_URL.myspace.com"
        }
      }
    }
    ```
    - [More information on the static application requirements from Heroku](https://github.com/heroku/heroku-buildpack-static)
    - [More information on using a proxy to avoid CORS issues can be found here](https://m.alphasights.com/using-nginx-on-heroku-to-serve-single-page-apps-and-avoid-cors-5d013b171a45)
1. Configure buildbacks on Heroku. From the command line in your project's directory, type:
    ```BASH
    heroku buildpacks:add --index 1 heroku/nodejs
    heroku buildpacks:add --index 2 https://github.com/heroku/heroku-buildpack-static.git
    ```
1. Modify your project's `package.json` file. Replace the contents of the `"scripts"` section of the json file with:
    ```
    "scripts": {
       "ng": "ng",
       "start": "http-server dist/",
       "test": "ng test",
       "lint": "ng lint",
       "e2e": "ng e2e",
       "preinstall": "npm install -g http-server",
       "postinstall": "ng build --target=production"
    },
    ```
    - This allows Heroku to start your application and adds some pre and post install hooks
1. Ensure you have committed the changes you made
1. run `git push origin heroku` to update your Heroku application with the latest source code and reload your Heroku application
### Troubleshooting
  - You don't know what you don't know. If your app doesn't load as expected, run `heroku logs` to view the logs and debug your deploy
  - If your app isn't connecting to your backend API, is your endpoint configured correctly in `static.json`? Is your backend getting any pings from your frontend?

## Deploy to Amazon S3
 - Deploying to Amazon S3 has its benefits but also is a good deal more configuration to get your static site up and running. If you need to tie into the AWS suite of tools like Cloudfront for efficient distribution, this may be the path for you.
 - Read this blog post to get started: https://medium.com/@coorasse/deploy-angular2-on-amazons3-5bbc040ab64a
 - This method should also work with React, Ember, or any other static front-end client application
## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
