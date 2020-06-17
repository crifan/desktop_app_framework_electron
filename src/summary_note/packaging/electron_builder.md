# electron-builder

## signing Error Command failed codesign sign

* **问题**：用`npm run dist`打包报错：
```bash
signing         file=release/mac/ElectronDemo.app identityName=Apple Development: xxx xx (xxx) identityHash=xxx provisioningProfile=none
Error: Command failed: codesign --sign xxx —force
```
* **原因**：code sign方面的错误。
* **解决办法**：暂未深入研究，所以只是去禁止code sign，以规避此问题。
    * 禁止code sign的方法
      * 先去：`export CSC_IDENTITY_AUTO_DISCOVERY=false`
      * 再继续打包：`npm run dist`

## make-dir.js 86 catch SyntaxError Unexpected token {
* **问题**：打包：
```bash
npm run dist
```
报错：
```bash
/Users/limao/dev/src/electron/electron-quick-start/node_modules/electron-builder/node_modules/fs-extra/lib/mkdirs/make-dir.js:86
      } catch {
              ^
SyntaxError: Unexpected token {
```
* **原因**：之前用 
```bash
npm install electron-builder
```
安装出的是最新版本的`electron-builder`：
```json
  "dependencies": {
    "electron-builder": "^22.6.1"
  }
```
而新版`22.6.1`有bug。

* **解决办法**：换个别的稍微旧一点的版本，比如`20.44.4`：
```bash
npm uninstall electron-builder
npm install -D electron-builder@20.44.4
```

## 
* **问题**：
* **原因**：
* **解决办法**：

