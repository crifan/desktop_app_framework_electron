# Electron支持Python的心得

此处整理Electron支持Python期间的心得：

## npm install时zeromq会报错

* **解决办法**：把node从`13.5.0`换成`8.x`的版本，比如此处的

```bash
brew install node@8
```

安装出来后的版本是：`8.17.0`

详见：

【已解决】mac中npm install报错：make Release/obj.target/zmq/binding.o Error 1

## 能看到electron界面，但是输入计算表达式后，却看不到输出结果

* **原因**：可能有多种
* **思路**：通过electron的默认打开的console的log输出的错误信息中去找原因
  * 如果找不到合适错误原因，则可以尝试去给python的进程加上stdout和stderr

`electron-python-example/main.js`

```js
const createPyProc = () => {
  let script = getScriptPath()
  let port = '' + selectPort()


  if (guessPackaged()) {
    pyProc = require('child_process').execFile(script, [port])
  } else {
    pyProc = require('child_process').spawn('python', [script, port])
  }


  if (pyProc != null) {
    console.log('child process success on port ' + port)


    pyProc.stdout.on('data',function(data){
      var textChunk = data.toString('utf8') // buffer to string
      console.log("textChunk=%s", textChunk)
    })


    pyProc.stderr.on('data', function(data){
      console.log("pyProc error=%s", data)
    });
  }
}
```

用于查看到输出的普通信息和错误信息，以便于找到错误现象和原因

详见：

【已解决】electron-python-example运行后输入计算内容但是不显示结果

## Uncaught ReferenceError process is not defined

**解决办法**：加上参数配置`nodeIntegration: true`

`electron-python-example/main.js`

```js
const createWindow = () => {
  mainWindow = new BrowserWindow(
    {
      width: 800,
      height: 600,
      webPreferences: {
        nodeIntegration: true,
      }
    }
  )
```

详见：

【已解决】electron-python-example运行出错：Uncaught ReferenceError process is not defined

## Uncaught Error: The module node_modules/zeromq/build/Release/zmq.node was compiled against a different Node.js version using NODE_MODULE_VERSION 57. This version of Node.js requires NODE_MODULE_VERSION 75

**解决办法**：把之前`13.5.0`版本的node，换成`8.x版本`（比如`8.17.0`）的`node`后，再重新编译electron

```bash
npm install --save-dev electron-rebuild
./node_modules/.bin/electron-rebuild
```

注：

编译之前，需要安装`cmake`

```bash
brew install cmake
```

详见：

【已解决】Electron报错：Uncaught Error The module zeromq zmq.node was compiled against a different Node.js version

## renderer.js Error Lost remote after 10000ms

* **原因**：python的zerorpc没有安装，或者安装了zerorpc但是python进程没有正常启动
* **解决办法**：确保已安装zerorpc

注：此处在虚拟环境virtualenv中

```bash
pip install zerorpc
```

然后确保

`electron-python-example/main.js`

```js
pyProc = require('child_process').spawn('python', [script, port])
```

正常启动了。

此处有2种确认方式：

* **方式1**：系统中查看是否有对应python进程

```bash
ps aux | grep 61737
```

其中的61737是python进程PID

是通过js中加了调试代码

```js
console.log('spawn: pyProc=%s', pyProc)
```

后，启动

```bash
./node_modules/.bin/electron .
```

时，看到console的log中的

```bash
  spawnfile: 'python',
  _handle: 
   Process {
     owner: [Circular],
     onexit: { [Function] [length]: 2, [name]: '' },
     pid: 61737
```

有对应python的pid的.。

如果有进程，说明python进程没问题，否则说明进程没正常启动。

* **方式2**： 加上`stdout`、`stderr`等监听

```js
  if (pyProc != null) {
    //console.log(pyProc)
    console.log('child process success on port ' + port)


    pyProc.stdout.on('data',function(data){
      var textChunk = data.toString('utf8') // buffer to string
      console.log("textChunk=%s", textChunk)
    })


    pyProc.stderr.on('data', function(data){
      console.log("pyProc error=%s", data)
    });
  }
```

确认没有error等异常错误信息

此处自己就是看到有

```bash
pyProc error=Traceback (most recent call last):
  File "/xxx/electron_python/electron-python-example/pycalc/api.py", line 4, in <module>
    import zerorpc
ModuleNotFoundError: No module named 'zerorpc'
```

从而发现：

是没有进入virtualenv的venv中，导致没有找到zerorpc

该确保进入虚拟环境：

```bash
source venv/bin/activate
```

即可找到zerorpc，正常启动python进程了。
