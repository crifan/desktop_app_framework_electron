# Electron简介

* Electron概述
    * 所属技术领域：
        * 跨平台的桌面端应用开发
    * 谁开发的：`Github`
    * 旧称：`Atom Shell`
      * 历史
        * 2013年作为构建Github上可编程的文本编辑器Atom的框架而被开发出来，这两个项目在2014春季开源
    * 一句话描述：一个**用纯Web技术来构建跨平台桌面应用程序的开源框架**
      * Web技术：`HTML`、`CSS`和`JavaScript`
        * 对比：传统桌面应用都是非Web技术开发的
      * 跨平台：`Win`/`Mac`/`Linux`等多个平台
      * 桌面应用：主要用来开发桌面端应用
        * 而不是Web应用
    * 基本原理
        * 将Chromium和Node.js合并到同一个运行时环境中
          * 让你使用纯`JavaScript`调用丰富的原生(操作系统) APIs 
        * 并将其打包为`Mac`、`Windows`和`Linux`系统下的应用
    * 可看成
        * 一个 Node. js 的变体
            * 专注于桌面应用而不是 Web Server 端
            * 使用 web 页面作为它的 GUI
        * 一个被 JavaScript 控制的，精简版的 Chromium 浏览器
    * 主页
      * [Electron | 使用 JavaScript，HTML 和 CSS 构建跨平台的桌面应用程序](https://www.electronjs.org)
    * 竞品
        * `nw.js`
    * 现状
        * 已成为开源开发者、初创企业和老牌公司常用的开发工具
          * 详见后续章节：[应用举例](https://book.crifan.com/books/desktop_app_framework_electron/website/electron_intro/who_use.html)
    * 优势
        * 颜值高=界面美观
            * 原因：`Web`技术（`HTML`+`CSS`+`JS`）天生适合页面信息展示
                * 截图
                    * ![electron_dev_easy_start](../assets/img/electron_dev_easy_start.jpg)
            * 对比其他技术：很多是基于各种图形库开发出的桌面应用，很多颜值一般
        * 界面统一
            * `Win`/`Mac`/`Linux`中显示效果几乎完全一样
                * 典型例子：[VSCode](https://book.crifan.com/books/best_editor_vscode/website/)
            * 对比其他技术：界面效果多数不太一样
        * 开发难度低 = 入门快 = 上手方便
            * = 像开发网站一样开发（跨平台桌面）应用
            * 比传统方式要简单很多
            * 说明
                * 传统方式：只支持某个平台，特定语言和框架，才能开发出该平台中的桌面端应用
                    * 举例：
                        * Win
                            * IDE：`Visual Studio` + 语言：`C#` + 框架：`WinForm`/`WPF`
                            * 截图
                                * ![win_vs_csharp_wpf_demo](../assets/img/win_vs_csharp_wpf_demo.png)
                                * ![win_vs_csharp_winform_demo](../assets/img/win_vs_csharp_winform_demo.png)
                        * Mac
                            * IDE：`XCode` + 语言：`Objective-C`/`Swift` + 框架：`Cocoa`
                            * 截图
                                * ![mac_xcode_oc_cocoa_demo](../assets/img/mac_xcode_oc_cocoa_demo.png)
    * 额外特性
        * [自动更新](https://www.electronjs.org/docs/api/auto-updater/)
            * 支持平台
              * 不支持`Linux`
              * 支持`Mac`和`Win`
                  * 都是基于Squirrel去实现的
        * [原生的菜单和通知](https://www.electronjs.org/docs/api/menu)
        * [崩溃报告](https://www.electronjs.org/docs/api/crash-reporter)
        * [调试和性能分析](https://www.electronjs.org/docs/api/content-tracing)
        * [Windows 安装程序](https://www.electronjs.org/docs/api/auto-updater/#windows)