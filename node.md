# About npm
## Cannot read property 'pause' of undefined
```shell
$ npm version
{ npm: '5.4.2',
  ares: '1.10.1-DEV',
  http_parser: '2.7.0',
  icu: '57.1',
  modules: '48',
  node: '6.3.0',
  openssl: '1.0.2h',
  uv: '1.9.1',
  v8: '5.0.71.52',
  zlib: '1.2.8' }
```

```shell
$ npm install -g node-inspector
> v8-profiler@5.7.0 preinstall /home/yyy/versions/node/v6.3.0/lib/node_modules/node-inspector/node_modules/v8-profiler
> node -e 'process.exit(0)'

npm ERR! Cannot read property 'pause' of undefined

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/yyy/.npm/_logs/2017-12-06T08_18_37_636Z-debug.log
```

Update npm
```shell
$ npm update -g npm
/home/yyy/versions/node/v6.3.0/bin/npm -> /home/yyy/versions/node/v6.3.0/lib/node_modules/npm/bin/npm-cli.js
/home/yyy/versions/node/v6.3.0/bin/npx -> /home/yyy/versions/node/v6.3.0/lib/node_modules/npm/bin/npx-cli.js
+ npm@5.6.0
added 79 packages, removed 13 packages and updated 129 packages in 13.99s
```

Retry

```shell
$ npm install -g node-inspector
/home/yyy/versions/node/v6.3.0/bin/node-inspector -> /home/yyy/versions/node/v6.3.0/lib/node_modules/node-inspector/bin/inspector.js
/home/yyy/versions/node/v6.3.0/bin/node-debug -> /home/yyy/versions/node/v6.3.0/lib/node_modules/node-inspector/bin/node-debug.js
+ node-inspector@1.1.1
updated 1 package in 7.445s
```
