# electron-rebuild

## build.sh cmake command not found make libzmq Error 127

* **问题**：用`electron-rebuild`编译报错：`build.sh cmake command not found make libzmq Error 127`
* **原因**：缺少`cmake`
* **解决办法**：
```bash
brew install cmake
```
## zeromq vendor napi.h error unknown type name napi_callback_scope
* **问题**：用`electron-rebuild`编译报错：`zeromq vendor napi.h error unknown type name napi_callback_scope`
* **原因**：未深究
* **解决办法**：无需解决，后续继续：

```bash
npm install
./node_modules/.bin/electron-rebuild
```

是可以正常启动Electron的。

## .electron-gyp node v8.h error unterminated conditional directive #ifndef INCLUDE_V8_H_
* **问题**：

用：

```bash
./node_modules/.bin/electron-rebuild
```

报错：

```bash
../binding.cc:1610:23: error: expected '}'
/Users/limao/.electron-gyp/7.1.7/include/node/v8.h:37:14: note: to match this '{'
namespace v8 {
             ^
1 warning and 13 errors generated.
make: *** [Release/obj.target/zmq/binding.o] Error 1
gyp ERR! build error 
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/Users/limao/dev/crifan/mitmdump/mitmdumpUrlSaver/electron_python/electron-python-example/node_modules/node-gyp/lib/build.js:194:23)
gyp ERR! stack     at emitTwo (events.js:126:13)
gyp ERR! stack     at ChildProcess.emit (events.js:214:7)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:198:12)
gyp ERR! System Darwin 18.7.0
gyp ERR! command "/usr/local/Cellar/node@8/8.17.0/bin/node" "/Users/limao/dev/crifan/mitmdump/mitmdumpUrlSaver/electron_python/electron-python-example/node_modules/.bin/node-gyp" "rebuild" "--target=7.1.7" "--arch=x64" "--dist-url=https://electronjs.org/headers" "--build-from-source"
gyp ERR! cwd /Users/limao/dev/crifan/mitmdump/mitmdumpUrlSaver/electron_python/electron-python-example/node_modules/zerorpc/node_modules/zeromq
gyp ERR! node -v v8.17.0
gyp ERR! node-gyp -v v6.0.1
gyp ERR! not ok
```

* **原因**：
  * 表面上是node的v8.h头文件有问题
      * 实际上：的确也是v8.h文件内容有问题
          * 此处内容不全，只有7000多行，实际上完整的文件大概1万多行
  * 但是根本原因在于：
      * 此处的node版本是v8.17.0
          * 最高支持的electron版本是2.0.18
              * 而当前设置的electron的版本是7.1.2
* **解决办法**：去把`electron`版本从`7.1.2`改为`2.0.18`

修改后：

文件：`electron_python/electron-python-example/.npmrc`

```ini
npm_config_target="2.0.18" # electron version
npm_config_runtime="electron" # 为elelctron编译
npm_config_disturl="https://atom.io/download/electron" # 资源下载地址
npm_config_build_from_source="true" # 从源码编译
```

文件：`electron_python/electron-python-example/package.json`
```json
{
...
  "devDependencies": {
    "electron": "^2.0.18",
...
  }
}
```

即可。