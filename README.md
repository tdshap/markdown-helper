# Monetization Proxy

Inject ads library into any html server's content by proxying their output

### To use

Use NODE_PORT to tell the proxy where the server is.  Go to port 4444 to see the proxied result.

```bash
nvm use
lerna install --global lerna
npm install
lerna bootstrap
cd packages/monetization-proxy
NODE_PORT=8081 npm start
```


###Build Ad Config

The ad configuration determines

1. Which ad slots are included on the page and
2. What the slot's definition will be.

  Slot definition includes:
    - render location
    - sizes
    - flags

Ad configs are stored in [github](https://github.com/CondeNast?utf8=%E2%9C%93&q=atp-cns-config-&type=&language=) The structure and use are further documented in the [wiki](https://cnissues.atlassian.net/wiki/spaces/ATP/pages/38633524/The+CNS+Ads+Configuration).


global configuration : default ad setup and options. All brands inherit these features, and can be overwritten. @condenast/atp-cns-config-global


brand configuration : brand's specific ad layouts that deviate from the global @condenast/atp-cns-config-brand-name


Choose which brand's ad config and environment you want to load locally (localhost:4444) from the file `bin/build-configs.js`

```
GITHUB_TOKEN=$GITHUB_TOKEN npm run config
```

Reset config

```
GITHUB_TOKEN=$GITHUB_TOKEN npm run config:reset
```

### To do

- change the environment variable NODE_PORT to a more accurate name of what it is
- allow the port 4444 to be changed with another environment variable
- remove requirement that project is run from its own root directory
