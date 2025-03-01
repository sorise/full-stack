## [Vite实现的核心原理](#)
**介绍**：Vite 意在提供开箱即用的配置，利用浏览器原生支持 ES Modules 的特性，无需进行打包操作。

----

- [1. 基本介绍](#1-基本介绍)


---
### [1. 基本介绍](#)
**Vite** 是一种新型前端构建工具，能够显著提升前端开发体验，Vite主要由两个部分组成:
- **一个开发服务器**: 它基于[原生ES模块](../../../PL.Learning/javascript/contents/JavaScriptModules.md) 提供了丰富的[内建功能](https://cn.vitejs.dev/guide/features)，如速度快到惊人的 [模块热更新（HMR）](https://link.juejin.cn/?target=https%3A%2F%2Fcn.vitejs.dev%2Fguide%2Ffeatures.html%23hot-module-replacement)。
- **一套构建指令**: 生成环境它使用 [Rollup](https://link.juejin.cn/?target=https%3A%2F%2Frollupjs.org%2F) 打包你的代码，提供指令来优化构建过程，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。

Vite有哪些主要**特性**:
- **极速的冷启动**：Vite采用原生ESM作为模块系统的实现，避免了传统打包工具在启动时的长时间编译过程，从而实现了极速的冷启动。
- **热模块替换（HMR）**：Vite内置了热模块替换功能，可以在不刷新页面的情况下，实时更新模块代码，提高开发效率。
- **原生ESM支持**：Vite直接利用浏览器对ESM的支持，无需额外转换，使得代码更加原生、高效。
- **插件化扩展**：Vite支持通过插件进行扩展，开发者可以根据自己的需求，定制开发流程。
