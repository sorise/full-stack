## [Vite实现的核心原理](#)
**介绍**：Vite 意在提供开箱即用的配置，利用浏览器原生支持 ES Modules 的特性，基于ES module 的 bundleless 构建工具。

----

- [1. 基本介绍](#1-基本介绍)


---
### [1. 基本介绍](#)
**Vite** 是一种新型前端构建工具，能够显著提升前端开发体验，Vite主要由两个部分组成:
- **一个开发服务器**: 它基于[原生ES模块](../../../PL.Learning/javascript/contents/JavaScriptModules.md) 提供了丰富的[内建功能](https://cn.vitejs.dev/guide/features)，如速度快到惊人的 [模块热更新（HMR）](https://link.juejin.cn/?target=https%3A%2F%2Fcn.vitejs.dev%2Fguide%2Ffeatures.html%23hot-module-replacement)。
- **一套构建指令**: 生成环境它使用 [Rollup](https://link.juejin.cn/?target=https%3A%2F%2Frollupjs.org%2F) 打包你的代码，提供指令来优化构建过程，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。

> Vite目的是解决Webpack 在大型项目中可能会遇到构建速度较慢的问题，尤其是在开发环境下。但Webpack 在生产环境下的功能和稳定性仍然是不可替代的。

Vite有哪些主要**特性**:
- **极速的冷启动**：Vite采用了现代的ES Module原生浏览器支持，利用浏览器去解析imports，并且按需编译，因此在开发环境下拥有非常快的冷启动和热更新速度。
- **热模块替换（HMR）**：Vite内置了热模块替换功能，可以在不刷新页面的情况下，实时更新模块代码，提高开发效率。
- **原生ESM支持**：Vite直接利用浏览器对ESM的支持，无需额外转换，使得代码更加原生、高效。
- **插件化扩展**：Vite支持通过插件进行扩展，开发者可以根据自己的需求，定制开发流程。
- **开箱即用**：Vite在设计上更注重开箱即用，大部分场景下用户无需自己写配置文件。

**vite 实际上是 esbuild + rollup 的封装**

#### [1.1 基本概念](#)
“打包”：使用工具抓取、处理并将我们的源码模块串联成可以在浏览器中运行的文件。

**缓慢的服务器启动**：当冷启动开发服务器时，基于打包器的方式启动必须优先抓取并构建你的整个应用，然后才能提供服务。
Vite 通过在一开始将应用中的模块区分为 依赖 和 源码 两类，改进了开发服务器启动时间。

#### [1.2 esbuild](#)
esbuild 是一个用于 JavaScript 和 CSS 的构建工具，它以其惊人的构建速度而闻名。由 Evan Wallace 创建，esbuild 主要通过使用 Go 语言编写来实现高效的性能，并且支持现代的 JavaScript 语法以及 TypeScript，能够将这些代码转换为向后兼容的版本，以便在各种环境中运行。

以下是 esbuild 的一些关键特性：
1. **极快的速度**：由于其底层使用了 Go 语言编写，并且充分利用了并行处理能力，esbuild 在执行打包、压缩等任务时比其他大多数构建工具（如 Webpack、Rollup 或 Parcel）要快得多。
2. **JavaScript 和 CSS 打包**：esbuild 可以将多个 JavaScript 或 CSS 文件打包成一个或多个输出文件，从而减少网络请求的数量，提高页面加载速度。
3. **支持现代语言特性**：它能够处理最新的 JavaScript 语法和 TypeScript，包括 JSX，自动将其转换为向后兼容的代码。
4. **Tree Shaking**：esbuild 能够识别并移除未使用的代码部分，从而减小最终输出文件的大小。
5. **Minification**：除了打包之外，esbuild 还可以压缩你的 JavaScript 和 CSS 代码，使其更紧凑，进一步加快加载时间。
6. **插件系统**：尽管 esbuild 本身非常快速高效，但它也提供了一个插件系统，允许用户扩展其功能以满足特定需求。
7. **开发服务器**：esbuild 提供了一个内置的开发服务器，支持热模块替换（HMR），可以在开发过程中极大地提升效率。

esbuild 因其卓越的性能和易用性，在开发者社区中迅速获得了广泛的关注和支持。无论是对于新项目的初始化还是现有项目的优化，esbuild 都是一个值得考虑的选择。