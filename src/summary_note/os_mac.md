# Mac系统

此处整理关于Mac系统中开发Electron的相关心得。

## npm安装electron速度太慢

mac中npm install很慢，则可以去全局设置源，换成淘宝的源：

```bash
npm config set registry https://registry.npm.taobao.org/
npm config set ELECTRON_MIRROR http://npm.taobao.org/mirrors/electron/
```

且单独给electron的变量，加到启动脚本中：

```bash
vi ~/.zshrc
```

最后加上：

```bash
export ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
```

保存退出后，去立刻生效：

```bash
source ~/.zshrc
```

再去用`echo`验证：

```bash
# echo $ELECTRON_MIRROR
http://npm.taobao.org/mirrors/electron/
```
即可极大地提高 `npm install`去安装electron相关东西的速度。

比如此处是从`<100KB/s` 变成 `> 1MB/s`。

## 获取不到process.env的值

经过之前折腾：

【未解决】Mac中electron打包后process.env是undefined获取不到有效值

确定是：

Mac中，打包后的`app`在运行期间，无法获取到env变量：

`process.env`

或：

```js
const remote = electron.remote;
remote.process.env
```

的值。-》也就无法获取到

`process.env.NODE_ENV`

或：

`remote.process.env.NODE_ENV`

的值了 -》 后续就没法通过别人说的：

```js
const remote = electron.remote
let nodeEnv = remote.process.env.NODE_ENV

const isProd = remote.process.env.NODE_ENV === 'production'

let portableExecutableDir = remote.process.env.PORTABLE_EXECUTABLE_DIR

let initCwd = remote.process.env.INIT_CWD
```

去判断是否是生产环境，以及获取到有效的app可执行文件所在目录了。
