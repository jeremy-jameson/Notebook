# Errors installing node.js packages on WIN10-DEV1

Friday, June 26, 2015
8:55 AM

## Install node.js packages

```Console
C:\Users\foo>npm install -g node-inspector bower gulp
npm ERR! Windows_NT 6.3.9600
npm ERR! argv "C:\\Program Files\\nodejs\\\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "install" "-g" "node-inspector" "bower" "gulp"
npm ERR! node v0.12.5
npm ERR! npm  v2.11.2
npm ERR! code ECONNRESET
npm ERR! errno ECONNRESET
npm ERR! syscall read

npm ERR! network read ECONNRESET
npm ERR! network This is most likely not a problem with npm itself
npm ERR! network and is related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'
npm ERR! Windows_NT 6.3.9600
npm ERR! argv "C:\\Program Files\\nodejs\\\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "install" "-g" "node-inspector" "bower" "gulp"
npm ERR! node v0.12.5
npm ERR! npm  v2.11.2
npm ERR! code ECONNRESET
npm ERR! errno ECONNRESET
npm ERR! syscall read
...
```

**Workaround:** Disable proxy auto-detect

![(screenshot)](https://assets.technologytoolbox.com/screenshots/74/CCE1375AA2CC962D8182D6C0860C66D6D5F9F674.png)

```Text
C:\Users\foo>npm install -g node-inspector bower gulp
C:\Users\foo\AppData\Roaming\npm\gulp -> C:\Users\foo\AppData\Roaming\npm\node_modules\gulp\bin\gulp.js
C:\Users\foo\AppData\Roaming\npm\bower -> C:\Users\foo\AppData\Roaming\npm\node_modules\bower\bin\bower
/


> v8-profiler@5.2.9 preinstall C:\Users\foo\AppData\Roaming\npm\node_modules\node-inspector\node_modules\v8-profiler
>

-


> v8-debug@0.4.6 preinstall C:\Users\foo\AppData\Roaming\npm\node_modules\node-inspector\node_modules\v8-debug
>

/


> bufferutil@1.1.0 install C:\Users\foo\AppData\Roaming\npm\node_modules\node-inspector\node_modules\ws\node_modules\bufferutil
> node-gyp rebuild


C:\Users\foo\AppData\Roaming\npm\node_modules\node-inspector\node_modules\ws\node_modules\bufferutil>if not defined npm_config_node_gyp (node "C:\Program Files\nodejs\node_modules\npm\bin\node-gyp-bin\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild )  else (node  rebuild )
\
gyp ERR! configure error
gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
gyp ERR! stack     at failNoPython (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:114:14)
gyp ERR! stack     at C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\lib\configure.js:69:11
gyp ERR! stack     at FSReqWrap.oncomplete (evalmachine.<anonymous>:95:15)
...
```
