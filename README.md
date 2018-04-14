HOW TO SET UP FAST ADS WITH A BRAND

Welcome to fast ads! Here’s a quick overview on the setup and how to get started developing locally.

Download the ads repo 
Github: https://github.com/CondeNast/ads
SSH: git@github.com:CondeNast/ads.git
HTTPS: https://github.com/CondeNast/ads.git

Ads is a mono repo that contains multiple npm modules that are used together. The repo uses lerna to manage the internal packages
cd ads/
nvm use # or nvm install if you're missing node 8
npm install # install devDepenencies
npm run bootstrap # npm links packages that reference each other together

Now you have the packages within the repo installed and linked!

To run fast-ads, you will need to have an autopilot-*brand* running in another terminal. 

Fast-ads loads it’s scripts through ESI tags pulled in from fastly. Since there is no fastly when developing locally, we have built an ESI server.

The esi server’s default.yaml file needs to be updated with a few things: 
PROXY_TARGET: 'http://localhost:8081' 
The proxy target field needs to be updated with the URL it will proxy from -- this will be where autopilot-*brand* normally outputs
This file is where you specify what files you want the proxy to serve for specific esi tags. If you are missing any files specified in default.yaml, the esi server will error out. So we need to build these files. 

Now, let’s build the files
Open a new terminal window and change to the fast-ads directory
cd ../fast-ads/

Run the build script for the files
npm run build // for one build 
OR
 npm run dev // if you want to build on files changed

Run the build script for the config
GITHUB_TOKEN=$GITHUBTOKEN npm run config brand-name environment

You will need to insert your github token as an environment variable or provide in the token in place of $GITHUBTOKEN
“brand-name” must match the brand’s name in their github config repo. atp-cns-config-*brand-name*
“environment” must match a branch in that github repo. 

Once those files are created, they will be placed into ads/packages/fast-ads/dist/*. Make sure your default.yaml is pointing to the right files in that folder. Pay extra attention to the config file if you are switching brands.

Go back to the terminal with the esi-server and start the server now that we have all of our files.
npm start

This will proxy the content serving at PROXY_TARGET  to ESI_TARGET . Go to the url specified as the ESI_TARGET. You should see the original content, plus the ESI tags on the new URL.

If you are starting work on a new brand that does not have ESI tags, you will need to add 3 scripts: 

In the brand head: 




Fast ads
