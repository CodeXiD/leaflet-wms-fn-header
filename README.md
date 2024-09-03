# leaflet-wms-fn-header

### In this version you can pass headers as a function if you need to calculate headers at runtime.

Custom Headers for Leaflet TileLayer WMS

This lightweight plugin allows you to set custom headers for the WMS interface in Leaflet.

It works seamlessly with both JavaScript and TypeScript, requiring no additional dependencies!

Fork from [leaflet-wms-header](https://github.com/ticinum-aerospace/leaflet-wms-header)

Based on https://github.com/Leaflet/Leaflet/issues/2091#issuecomment-302706529.

### Javascript
```sh
$ npm install leaflet leaflet-wms-fn-header --save
```

```html
<!-- Assuming your project root is "../" -->
<script src="../node_modules/leaflet/dist/leaflet.js"></script> 
<script src="../node_modules/leaflet-wms-fn-header/index.js"></script> 
```

```js

// YOUR LEAFLET CODE

var wmsLayer = L.TileLayer.wmsHeader(
    'https://GEOSERVER_PATH/geoserver/wms?',
    {
        layers: 'ne:ne',
        format: 'image/png',
        transparent: true,
    },
    [
        { header: 'Authorization', value: 'JWT ' + MYAUTHTOKEN },
        { header: 'content-type', value: 'text/plain'},
    ],
    null, // abort function
    (err) => console.log (err) // image loading error handling function (optional)
).addTo(map);
```

### Headers in a function

```js

let dynamicToken = 'SOME_TOKEN_1';

setTimeout(() => {
    dynamicToken = 'SOME_TOKEN_2';
}, 5000)

var wmsLayer = L.TileLayer.wmsHeader(
    'https://GEOSERVER_PATH/geoserver/wms?',
    {
        layers: 'ne:ne',
        format: 'image/png',
        transparent: true,
    },
    () => [
        { header: 'Authorization', value: 'JWT ' + dynamicToken },
        { header: 'content-type', value: 'text/plain'},
    ],
    null
).addTo(map);
```

### Typescript
```sh
$ npm install leaflet @types/leaflet leaflet-wms-fn-header --save
```
```ts
import * as L from 'leaflet';
import 'leaflet-wms-fn-header';

// YOUR LEAFLET CODE

let wmsLayer: L.TileLayer.WMSHeader = L.TileLayer.wmsHeader(
    'https://GEOSERVER_PATH/geoserver/wms?',
    {
        layers: layers,
        format: 'image/png',
        transparent: true,
    }, [
        { header: 'Authorization', value: 'JWT ' + MYAUTHTOKEN },
        { header: 'content-type', value: 'text/plain'},
    ],
    null
).addTo(map);
```

### Abort parameter

Abort parameter allow to abort the http request through an Observable. This optimization function might be usefull to stop the http request when it is not necessary anymore, mostly if many requests are pending. An example is provided on /tests/system-tests.html .

See below an example using an Observable as "abort" parameter.

```ts
let tileLayer: L.TileLayer.WMSHeader = L.TileLayer.wmsHeader(
    'https://GEOSERVER_PATH/geoserver/wms?',
    {
        layers: layers,
        format: 'image/png',
        transparent: true,
    }, [
        { header: 'Authorization', value: 'JWT ' + MYAUTHTOKEN },
        { header: 'content-type', value: 'text/plain'},
    ],
    this.abortWMSObservable$.pipe(take(1))
);
```
