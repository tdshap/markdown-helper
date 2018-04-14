# How To Setup fast-ads

Welcome to fast ads! Here’s a quick overview on the setup, and how to get started developing locally.

## Download Ads Repo
[Ads](https://github.com/CondeNast/ads) is a mono repo that contains multiple npm modules that are used together. The repo uses lerna to manage internal packages. See the [readme](https://github.com/CondeNast/ads/blob/master/readme.md) for more details.

```bash
git clone git@github.com:CondeNast/ads.git # for ssh 
# or 
git clone https://github.com/CondeNast/ads.git # https
cd ads/
nvm use # or nvm install if you're missing node 8
npm install # install devDepenencies
npm run bootstrap # npm links packages that reference each other together
```

Now you have the packages within the repo installed and linked!

## Autopilot Setup
To run fast-ads, you will need an autopilot-*brand* running in another terminal. For this example, we will use `autopilot-tny`.

Open and run TNY in terminal new tab 
```bash
cmt + t # new tab

cd ../ # change out of ads repo 

# clone tny
git clone git@github.com:CondeNast/autopilot-tny.git # for ssh
# or
git clone https://github.com/CondeNast/autopilot-tny.git # for https

cd autopilot-tny # should be on the default branch 'develop'

npm i 

npm run dev
```

When running dev mode, you will see TNY load at `localhost:8080`.

## ESI Server
Fast-ads loads it’s scripts through ESI tags pulled in from fastly. Since there is no fastly when developing locally, we have built an ESI server that proxies a port and replaces ESI tags with local files. The ports it proxies to/from and the files it servers are all configurable from the `defaults.yaml` file in the [root](https://github.com/CondeNast/ads/blob/master/packages/esi-server/defaults.yaml). 

#### default.yaml
```yaml
env:
  NODE_PORT: 8001
  PROXY_TARGET: 'http://localhost:8080'
  ESI_TARGET: 'http://localhost:8001'
```
`PROXY_TARGET` is where the autopilot brand is running. You will NOT see ads load on this port until the ESI tags are replaced with scripts. 
`NODE_PORT` and `ESI_TARGET` should match ports. This is the where you will see the autopilot app load ads scripts.

Now we need to specify the esi tag path to the file it should load. 

Fast ads has three essential files 1) head.js 2) footer.js and 3) config.js all 3 are build to the `dist/` folder. Head and footer don't change per brand, but the config file will differ per brand and environment: `dist/brand/environment/config.js`

```yaml
# Ads
- uri: "/cns/head.js"
  behavior: serveFile
  filePath: "../fast-ads/dist/head.js"
  contentType: "text/javascript"

- uri: "/cns/head.min.js"
  behavior: serveFile
  filePath: "../fast-ads/dist/head.js"
  contentType: "text/javascript"

- uri: "/cns/footer.js"
  behavior: serveFile
  filePath: "../fast-ads/dist/footer.js"
  contentType: "text/javascript"

- uri: "/cns/footer.min.js"
  behavior: serveFile
  filePath: "../fast-ads/dist/footer.js"
  contentType: "text/javascript"

- uri: "/cns/config.js"
  behavior: serveFile
  filePath: "../fast-ads/dist/the-new-yorker/production/config.js"
  contentType: "text/javascript"
```

If any file paths specified here do not exist on your local machine, the esi server will error out. So now, let’s build these files.

## Fast Ads
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
