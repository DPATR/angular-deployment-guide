[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Angular Deployment Guide 

Use this guide to deploy Angular apps

## Prerequisites

-   Angular app ready for deployment
-   
    

## Objectives

By the end of this, developers should be able to:

- Deploy an Angular app to Heroku   
- Deploy an Angular app to GitHub Pages

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
Heroku buildpacks:add --index 1 heroku/nodejs
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

## Deploying to Gitub Pages
1.
## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
