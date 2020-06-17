# Win系统

此处整理关于Windows平台中打包相关心得。

## 运行时Win的exe所在路径

背景：Win中Electron打包后的可执行文件格式一般是`exe`。

需求：希望在运行期间，获取到当前的实际的exe所在目录的路径。

心得：双击`exe`运行后，其内部会解压到临时目录，一般是当前用户的`AppData`目录，类似于这种：

`C:\Users\xxx\AppData\Local\Programs\`

-》如果想要获取原本（双击以启动运行的）exe所在目录，则就很困难了。

期间去试过参数 `--win portable`，对应配置：

```json
    "win": {
      "target": "portable",
      "icon": "assets/icon/logo.png"
    }
```

结果却是，虽然打包出的exe无需安装，双击即可运行，但是运行期间解压的目录是这种更彻底的临时目录：

`C:\Users\xx~1\AppData\Local\Temp\1Wgi1npnbDTNiOqe7pBCQL1FL0Z`

![exe_appdata_local_temp_folder](../../assets/img/exe_appdata_local_temp_folder.png)

更加没法找到exe所在目录。

后来去试了试别人[提到](https://github.com/electron-userland/electron-builder/issues/3841)的相关参数：

`process.env.PORTABLE_EXECUTABLE_DIR`

最终是可以获取到当前exe所在目录的：

exe所在目录：

![exe_running_folder_ok](../../assets/img/exe_running_folder_ok.png)

相关代码：

```js
let isWin = process.platform === "win32"

if (isWin){
  let portableExecutableDir = process.env.PORTABLE_EXECUTABLE_DIR
  console.log("portableExecutableDir=%s", portableExecutableDir)
}
```

输出：

```bash
portableExecutableDir=D:\dev\DevRoot\mitmdumpurlsaver\electron-python-example\builder_output
```

供参考。
