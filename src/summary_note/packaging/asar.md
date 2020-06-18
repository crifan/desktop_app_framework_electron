# asar

* asar简介
  * 是什么：一种压缩文件格式
  * 目的=为何要压缩：
    * 减小文件体积
      * 打包后的应用文件尽量小，不要太大
    * 避免暴露源码
  * 特点
    * Electron 可以无需解压整个文件，即可从其中读取任意文件内容

## 如何使用=开启asar

为了使用一个 asar 档案文件代替 app 文件夹，你需要修改这个档案文件的名字为 app.asar，

然后将其放到 Electron 的资源文件夹下，然后 Electron 就会试图读取这个档案文件并从中启动。 

asar 目录结构：

* Mac

```bash
electron/Electron.app/Contents/Resources/
└── app.asar
```

* Window 和 Linux

```bash
electron/resources/
└── app.asar
```

更多细节详见：

[应用程序打包 | Electron](https://electronjs.org/docs/tutorial/application-packaging)

## asar使用心得

### 开启asar但不压缩部分文件和目录

有些时候需要实现，虽然整体开启asar，但是不想asar压缩部分内容，则可以用：`asarUnpack`

举例：

```json
    "asar": true,
    "asarUnpack": [
      "main.js",
      "pymitmdumpstartdist",
      "pymitmdumpotherdist"
    ],
```

确保预期效果：打包后的mac的app中有了2个dist目录和一个文件：

* `electron-python-example/builder_output/mac/mitmdumpUrlSaver.app/Contents/Resources/app.asar.unpacked/`
  * `pymitmdumpstartdist`
  * `pymitmdumpotherdist`
  * `main.js`

从而确保`main.js`运行期间去判断对应的PyInstaller打包后的python的二进制文件

* `xxx/builder_output/mac/mitmdumpUrlSaver.app/Contents/Resources/app.asar/`
  * `pymitmdumpstartdist/mitmdumpStartApi/mitmdumpStartApi`
  * `pymitmdumpotherdist/mitmdumpOtherApi/mitmdumpOtherApi`

都可以被

```js
isPackaged = require('fs').existsSync(fullPath)
```

识别到，确保后续代码逻辑可以正常运行。

详见：

【已解决】Mac中electron-python用electron-builder打包后app运行失败：Error Lost remote after 10000ms
