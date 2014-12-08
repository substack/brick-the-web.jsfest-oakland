# brick the web

http://substack.net

https://{github,twitter}.com/substack

---
# web apps

request cycle:

```
browser  |  server

request  ->
         <- response
request  ->
         <- response
request  ->
         <- response
 
        ...
---
# web apps

```
         user
          |
    user  |   user
       \  |   / 
user -- server  -- user
       /  |   \  
    user  |   user
          |
         user
```

---
# web apps

problems:

* centralization creates leverage

* requires a low-latency network

* server can send any payload

---
# web apps

important new browser features:

* webrtc


* appcache / {shared,service} workers


* window.crypto.subtle


---
# web apps

important new browser features:

* webrtc
  -> we don't need (many) servers for networking

* appcache / {shared,service} workers


* window.crypto.subtle


---
# web apps

important new browser features:

* webrtc
  -> we don't need (many) servers for networking

* appcache / {shared,service} workers
  -> we don't need reliable networks

* window.crypto.subtle


---
# web apps

important new browser features:

* webrtc
  -> we don't need (many) servers for networking

* appcache / {shared,service} workers
  -> we don't need reliable networks

* window.crypto.subtle
  -> we can't allow the server to send arbitrary payloads

---
# appcache.manifesto

```
/
/page.appcache
NETWORK:
*
```

---
# appcache.manifesto

```
/
/page.appcache
NETWORK:
*
```

Cache-Control: max-age=3155760000

Turn your web page into a brick on purpose!

---
# bundle everything

demo: dream inline

---
# htmlb.in

demo!

https://6b04e1200a25087ab551f1a14bd1356f04312d2b.htmlb.in/

---
# hyperboot

bricked application version manager

---
# hyperboot

http://demo.hyperboot.org/

demo: dream boot

---
# page bus

realtime without servers!

---
# page bus

``` js
var createBus = require('page-bus');
var bus = createBus();
```

---
# page bus

``` js
var createBus = require('page-bus');
var bus = createBus();




bus.on('hello', function (msg) {
    // ...
});

bus.emit('hello', Date.now());
```

---
# page bus

``` js
var createBus = require('page-bus');
var bus = createBus();

var pre = document.querySelector('pre');


bus.on('hello', function (msg) {
    pre.textContent += msg + '\n';
});

bus.emit('hello', Date.now());
```

---
# page bus

``` js
var createBus = require('page-bus');
var bus = createBus();

var pre = document.querySelector('pre');
var form = document.querySelector('form');

bus.on('hello', function (msg) {
    pre.textContent += msg + '\n';
});

bus.emit('hello', Date.now());

form.addEventListener('submit', function (ev) {
    ev.preventDefault();
    bus.emit('hello', form.elements.msg.value);
    form.elements.msg.value = '';
});
```

---
# page bus

```
$ browserify bus.js > bundle.js
$ html-inline index.html | htmlbin
https://b5339bccce4dbf8359afea58b2da888bb98a40a0.htmlb.in
```

---
# keyboot

single sign-on as a bricked static web page

---
# keyboot

``` js
var keyboot = require('keyboot');
var kbui = require('keyboot-ui');
```

---
# keyboot

``` js
var keyboot = require('keyboot');
var kbui = require('../');

var boot = kbui(keyboot, {
    permissions: [ 'fingerprint', 'sign', 'publicKey' ],
    defaultURL: 'https://keyboot.org'
});
```

---
# keyboot

``` js
// ...
var elems = {
    sign: document.querySelector('#sign'),
    button: document.querySelector('#sign button'),
    txt: document.querySelector('#sign textarea'),
    result: document.querySelector('#sign .result')
};
```

---
# keyboot

``` js
// ...
elems.button.addEventListener('click', function () {
    boot.sign(elems.txt.value, function (err, res) {
        elems.result.textContent = Buffer(res).toString('base64');
    });
});
```

---
# keyboot

``` js
// ...
boot.on('approve', function () { show(elems.sign) });
boot.on('revoke', function () { hide(elems.sign) });
boot.on('close', function () {
    hide(elems.sign);
    elems.result.textContent = '';
    elems.txt.value = '';
});

function show (e) { e.style.display = 'block' }
function hide (e) { e.style.display = 'none' }

document.querySelector('#auth').appendChild(boot.element);
```

---
# keyboot

https://keyboot.org/
https://82a90170090c6899e5cf4b8bc83233cae8e2225a.htmlb.in/

---
# brick the web!

THE END.
