# files

`package.json`配置参数中有个`files`，用来控制要打包的文件。此处整理相关心得。

## Application entry file main.js in the .app/Contents/Resources/app.asar does not exist

* **问题**：在改了配置为

```json
    "files": [
      "pymitmdumpstartdist",
      "pymitmdumpotherdist"
    ],
```

后，打包报错：

```bash
⨯ Application entry file "main.js" in the "xxx/mac/mitmdumpUrlSaver.app/Contents/Resources/app.asar" does not exist. Seems like a wrong configuration. 
```

* **原因**：根据官网

[files - Application Contents - electron-builder](https://www.electron.build/configuration/contents#files) 解释，默认`files`配置是：

```json
    "files": [
      "**/*",
      "!**/node_modules/*/{CHANGELOG.md,README.md,README,readme.md,readme}",
      "!**/node_modules/*/{test,__tests__,tests,powered-test,example,examples}",
      "!**/node_modules/*.d.ts",
      "!**/node_modules/.bin",
      "!**/*.{iml,o,hprof,orig,pyc,pyo,rbc,swp,csproj,sln,xproj}",
      "!.editorconfig",
      "!**/._*",
      "!**/{.DS_Store,.git,.hg,.svn,CVS,RCS,SCCS,.gitignore,.gitattributes}",
      "!**/{__pycache__,thumbs.db,.flowconfig,.idea,.vs,.nyc_output}",
      "!**/{appveyor.yml,.travis.yml,circle.yml}",
      "!**/{npm-debug.log,yarn.lock,.yarn-integrity,.yarn-metadata.json}",
    ],
```

其中的 `**/*` 包含了根目录下的各种核心文件，包括入口文件`main.js`

而前面的配置：

```json
    "files": [
      "pymitmdumpstartdist",
      "pymitmdumpotherdist"
    ],
```

表示：去掉其他文件，只包含上述2个文件夹

导致找不到必要的入口文件`main.js`（等其他文件）

* **解决办法**：把自己的配置 加到 默认配置上：

```json
    "files": [
      "**/*",
      "!**/node_modules/*/{CHANGELOG.md,README.md,README,readme.md,readme}",
      "!**/node_modules/*/{test,__tests__,tests,powered-test,example,examples}",
      "!**/node_modules/*.d.ts",
      "!**/node_modules/.bin",
      "!**/*.{iml,o,hprof,orig,pyc,pyo,rbc,swp,csproj,sln,xproj}",
      "!.editorconfig",
      "!**/._*",
      "!**/{.DS_Store,.git,.hg,.svn,CVS,RCS,SCCS,.gitignore,.gitattributes}",
      "!**/{__pycache__,thumbs.db,.flowconfig,.idea,.vs,.nyc_output}",
      "!**/{appveyor.yml,.travis.yml,circle.yml}",
      "!**/{npm-debug.log,yarn.lock,.yarn-integrity,.yarn-metadata.json}",

      "pymitmdumpstartdist/",
      "pymitmdumpotherdist/"
    ],
```

即可。
