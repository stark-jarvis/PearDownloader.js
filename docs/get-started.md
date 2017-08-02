# Get Started with PearPlayer

**PearDownloader** is a multi-source and multi-protocol P2P streaming downloader that works in the **browser**. It's easy
to get started!

## Import
### Script
Simply include the
([`pear-downloader.min.js`](dest/pear-downloader.min.js))
script on your page and use `require()`:
```html
<script src="pear-downloader.min.js"></script>
```

### Browserify
To install PearPlayer for use in the browser with `require('PearDownloader')`, run:
```bash
npm install peardownloader --save
```
Then you can require PearPlayer like this:
```js
var PearDownloader = require('PearDownloader');
```

## Quick Examples

### input url and download

```js
var PearDownloader = require('PearDownloader');
var xhr = new XMLHttpRequest();
//CP需要先登录来获取token
xhr.open("POST", 'https://api.webrtc.win:6601/v1/customer/login');
var data = JSON.stringify({
    user:'demo',
    password:'demo'
});
xhr.onload = function () {
    if (this.status >= 200 && this.status < 300) {

        var res = JSON.parse(this.response);
        if (!!res.token){
            console.log('token:' +res.token);

            var downloader = new PearDownloader(url, res.token, {//第一个参数为video标签的id或class
                algorithm: 'firstaid',                 //核心算法,默认firstaid
                useMonitor: true                       //是否开启monitor，会稍微影响性能，默认true
            });
        }
    } else {
        alert('请求出错!');
    }
};
xhr.send(data);
```

There is a complete example in [examples/test.html](examples/download.html)。

### Listen to PearPlayer events

```js
var downloader = new PearDownloader(url, token, {      //第一个参数为url
                    useMonitor: true             //是否开启monitor,会稍微影响性能,默认false
                });

downloader.on('exception', onException);
downloader.on('begin', onBegin);
downloader.on('progress', onProgress);
downloader.on('buffersources', onBufferSources);               //s: server   n: node  d: data channel  b: browser
downloader.on('done', onDone);
                
function onBegin(fileLength, chunks) {
    console.log('start downloading buffer by first aid, file length is:' + fileLength + ' total chunks:' + chunks);
}

function onProgress(downloaded) {
    console.log('Progress: ' + (downloaded * 100).toFixed(1) + '%');
}

function onDone() {
    console.log('finished downloading buffer by first aid');
}

function onException(exception) {
    var errCode = exception.errCode;
    switch (errCode) {
        case 1:                   //当前浏览器不支持WebRTC
        console.log(exception.errMsg);
            break
    }
}
function onBufferSources(bufferSources) {    //s: server   n: node  d: data channel  b: browser
    console.log('Current Buffer Sources:' + bufferSources);
}
```

## Build

PearDownloader works great with [browserify](http://browserify.org/), which lets
you use [node.js](http://nodejs.org/) style `require()` to organize your browser
code, and load packages installed by [npm](https://npmjs.org/).

```bash
npm install -g browserify
```
Install dependencies:
```bash
npm install
```
To get a normal size bundle,use:
```bash
npm run build
```
To get a compressed bundle,use:
```bash
npm run uglify
```

## More Documentation

Check out the [API Documentation](https://github.com/PearInc/PearPlayer.js/blob/master/docs/api.md)
and [FAQ](https://github.com/PearInc/PearPlayer.js/blob/master/docs/faq.md) for more details.