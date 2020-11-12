#私服设置

## yarn

```shell script
yarn config set registry https://registry.npm.taobao.org --global  && \
yarn config set disturl https://npm.taobao.org/dist --global && \
yarn config set sass_binary_site https://npm.taobao.org/mirrors/node-sass --global  && \
yarn config set electron_mirror https://npm.taobao.org/mirrors/electron/ --global  && \
yarn config set puppeteer_download_host https://npm.taobao.org/mirrors --global  && \
yarn config set chromedriver_cdnurl https://npm.taobao.org/mirrors/chromedriver --global  && \
yarn config set operadriver_cdnurl https://npm.taobao.org/mirrors/operadriver --global  && \
yarn config set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs --global  && \
yarn config set selenium_cdnurl https://npm.taobao.org/mirrors/selenium --global  && \
yarn config set node_inspector_cdnurl https://npm.taobao.org/mirrors/node-inspector --global
```

## npm

```shell script
npm set registry https://registry.npm.taobao.org && \
npm set disturl https://npm.taobao.org/dist && \
npm set sass_binary_site https://npm.taobao.org/mirrors/node-sass && \
npm set electron_mirror https://npm.taobao.org/mirrors/electron && \
npm set puppeteer_download_host https://npm.taobao.org/mirrors && \
npm set chromedriver_cdnurl https://npm.taobao.org/mirrors/chromedriver && \
npm set operadriver_cdnurl https://npm.taobao.org/mirrors/operadriver && \
npm set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs && \
npm set selenium_cdnurl https://npm.taobao.org/mirrors/selenium && \
npm set node_inspector_cdnurl https://npm.taobao.org/mirrors/node-inspector && \
npm cache clean --force
```
