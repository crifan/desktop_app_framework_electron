# NODE_MODULE_VERSION和node版本对应关系

折腾Electron期间，往往会遇到类似于

【未解决】Electron报错：Uncaught Error The module zeromq zmq.node was compiled against a different Node.js version

的问题，就会涉及到：`NODE_MODULE_VERSION`和`node`之间的版本对应关系。

此处总结如下：

|                        |    NodeJS版本  |                 说明             |
| ---------------------- | ------------- | -------------------------------- |
| NODE_MODULE_VERSION 79 | Node.js 13.x  | Node.js 13.0.0 ~ Node.js 13.5.0  |
| NODE_MODULE_VERSION 72 | Node.js 12.x  | Node.js 12.0.0 ~ Node.js 12.14.0 |
| NODE_MODULE_VERSION 75 | （据说是）Node.js 12.7.0 |  |
| NODE_MODULE_VERSION 67 | Node.js 11.x | Node.js 11.0.0 ~ Node.js 11.15.0 |
| NODE_MODULE_VERSION 64 | Node.js 10.x | Node.js 10.0.0 ~ Node.js 10.18.0 |
| NODE_MODULE_VERSION 59 | Node.js 9.x | Node.js 9.0.0 ~ Node.js 9.11.2 |
| NODE_MODULE_VERSION 57 | Node.js 8.x | Node.js 8.0.0 ~ Node.js 8.17.0 |
| NODE_MODULE_VERSION 51 | Node.js 7.x | Node.js 7.0.0 ~ Node.js 7.10.1 |
| NODE_MODULE_VERSION 48 | Node.js 6.x | Node.js 6.0.0 ~ Node.js 6.17.1 |
| NODE_MODULE_VERSION 47 | Node.js 5.x | Node.js 5.0.0 ~ Node.js 5.12.0 |
| NODE_MODULE_VERSION 46 | Node.js 4.x | Node.js 4.0.0 ~ Node.js 4.9.1 |

更多细节详见：

* [以往的版本 | Node.js](https://nodejs.org/zh-cn/download/releases/)
* [Node v12.7.0 (Current) | Node.js](https://nodejs.org/es/blog/release/v12.7.0/)
