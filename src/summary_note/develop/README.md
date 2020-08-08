# 开发

此处整理关于Electron开发期间的一些心得。

## Uncaught ReferenceError process is not defined

* **问题**：文件：`electron-python-example/index.html`
的代码：

```js
  <script>
    require('./renderer.js')
  </script>
```

报错：`Uncaught ReferenceError process is not defined`

* **原因**：Electron `5.0`之后，把之前默认是开启的`nodeIntegration`关闭了，导致找不到进程而报错

* **解决办法**：加上参数去开启：`nodeIntegration:true`

文件：`electron_python/electron-python-example/main.js`

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

## textarea右键复制选中内容

* **背景**：希望`textarea`区域可以右键显示按钮，带复制选项，用于复制所选内容

* **解决办法**：

安装插件 [sindresorhus/electron-context-menu: Context menu for your Electron app](https://github.com/sindresorhus/electron-context-menu)，即可实现此功能。

* **步骤**：

```bash
npm install electron-context-menu
```

使用：

```js
const contextMenu = require('electron-context-menu');
contextMenu({
    prepend: (defaultActions, params, browserWindow) => [
        {
            label: 'Rainbow',
            // Only show it when right-clicking images
            visible: params.mediaType === 'image'
        },
        {
            label: 'Search Google for "{selection}"',
            // Only show it when right-clicking text
            visible: params.selectionText.trim().length > 0,
            click: () => {
                shell.openExternal(`https://google.com/search?q=${encodeURIComponent(params.selectionText)}`);
            }
        }
    ]
});
```

Electron中的textarea，即可支持右键，出现菜单，选择文字后，即可复制：

![textarea_right_menu_copy](../../assets/img/textarea_right_menu_copy.png)

**进一步的需求**：禁止右键中除了`Copy`外其他按钮菜单

去加上配置：

```js
const contextMenu = require('electron-context-menu');
contextMenu({
  showLookUpSelection: false,
  showCopyImage: false,
  showInspectElement: false,
});
```

效果：

![textarea_context_menu_only_copy](../../assets/img/textarea_context_menu_only_copy.png)
