# How to Setup Fast-ads

Welcome to fast ads! Here’s a quick overview on the setup, and how to get started developing locally.

## Download Ads Repo
[Ads](https://github.com/CondeNast/ads) is a mono repo and contains multiple npm modules that are used together. This repo uses [lerna](https://lernajs.io/) to manage internal packages. See the [readme](https://github.com/CondeNast/ads/blob/master/readme.md) for more details.

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
Fast-ads loads it’s scripts through ESI tags pulled in from fastly. Since there is no fastly when developing locally, we built an ESI server that proxies a port, and replaces ESI tags with local files. The ports it proxies to/from and the files it serves are all configurable from the `defaults.yaml` file in the [root](https://github.com/CondeNast/ads/blob/master/packages/esi-server/defaults.yaml). 

#### default.yaml
```yaml
env:
  NODE_PORT: 8001
  PROXY_TARGET: 'http://localhost:8080'
  ESI_TARGET: 'http://localhost:8001'
```
`PROXY_TARGET` - where the autopilot brand is running. NOTE: You will not see ads load on this port until the ESI tags are replaced with scripts. 

`NODE_PORT` and `ESI_TARGET` - These two ports should match, and will be the URL where the autopilot app + filled esi scripts will serve.

Nest, we need to specify the ESI tag path to the file it should load. 

Fast ads has three essential files `head` `footer` and `config`. All three are built to the `dist/` folder. `head` and `footer` don't change per brand, but the config file will differ per brand and environment: `dist/brand/environment/config.js`.

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

Once those files are specified, we can start the proxy server: 

```bash
npm start
```

If any file paths specified here do not exist on your local machine, the ESI server will error out. So now, let’s build these files.

## Fast Ads
Open a new terminal window and change to the fast-ads directory.

```bash
cmt + t # new tab

cd ../fast-ads/ # change to fast-ads
```

#### Build head and footer
``` bash 
npm run build # build once
# or
npm run dev # builds on file change - dev mode
```

This builds `head` and `footer` in 3 forms: 
1. Unminified, dev mode - `head.js` and `footer.js`
2. Minified - production mode - `header.min.js` and `footer.min.js`
3. Script tag minified - esi mode - `head` `footer`

#### Build config
You will need a Github token, and your account must have access to the brand config repo you are trying to build. If you do not have access, contact us in the slack channel #ads-support. 

```bash
GITHUB_TOKEN=$GITHUBTOKEN npm run config brand-name environment
```
`$GITHUBTOKEN` - your github token or an environment variable that points to your github token
`brand-name` - must match the brand’s name in their github config repo. [atp-cns-config-*brand-name*](https://github.com/CondeNast?utf8=%E2%9C%93&q=atp-cns-config-&type=&language=)
`environment` - must match a branch in the github repo. 

Once those files are created, they will be placed into `ads/packages/fast-ads/dist/*`. Make sure your `default.yaml` is pointing to the right files in that folder. Pay extra attention to the config file if you are switching brands.

## Final Steps
Go back to the terminal with the esi-server, and start the server now that we have all of our files.

```bash
npm start
```

