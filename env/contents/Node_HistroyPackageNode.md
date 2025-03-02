## [Node.JS 包管理器](#)
**介绍**：包管理器用于自动化处理包的安装、更新、配置和管理，比较混乱有npx、npm、cnpm、pnpm、yarn...。

----

### [1. 包管理器](#)
Node.JS 有很多个包管理工具，npm是其默认包管理器。

#### [1.1 npm](#)
NPM（Node Package Manager），**作为默认的JavaScript应用包管理器**，与Node.js一同安装，它是目前使用最广泛的包管理器，得益于其对大量包的强大支持。
通过 npm，你可以方便地将项目所需的库和依赖添加到项目中，并且可以使用命令如 `npm install <package-name>` 来安装特定的包。

**NPM的工作原理:** NPM拥有一个**集中式的注册中心**，其中托管了数以千计的包。这些包可以是库、框架、助手、工具或实用工具。
当你运行npm install时，NPM 会从 NPM注册中心下载package.json文件中列出的包。

> 下载这些依赖项时，NPM还会生成一个锁文件（package-lock.json），该文件指定了为项目下载的所有依赖项（直接和间接）的确切版本。

**优势**：
- 广泛的支持 — NPM托管着世界上最大的JavaScript包注册中心。
- 简化的依赖管理 — NPM以最简化的方式自动化查找、安装和管理依赖的过程。
- 易于使用 — NPM设置和使用简单，对所有技能级别的开发者都易于接入。

**劣势**：
- 磁盘空间 — 由于NPM使用嵌套依赖树方法保存包，如果不同的依赖需要它们，它需要更多的磁盘空间来保存同一包的多个副本。
- 依赖膨胀 — 如果依赖/包在长期内没有得到适当管理，可能会导致不必要地积累大量包，这可能会增加项目的大小并潜在引入兼容性问题。
- 性能 — 与其他包管理器相比，特别是对于有许多依赖的较大项目，NPM的安装可能会更慢，因为它顺序下载包。

> npm 在项目中创建一个 node_modules 文件夹，其中包含所有的依赖项。每个项目的依赖都是独立安装的，这可能会导致重复的包和较大的存储空间占用

#### [1.2 cnpm](#)
**npm** 的默认官方下载源是 [https://registry.npmjs.org/](https://registry.npmjs.org/)，国内访问经常掉线。因此淘宝推出了淘宝源 [https://registry.npm.taobao.org](https://registry.npm.taobao.org) 。

```shell
//可以在安装包的时候指定下载源 
npm i --registry=https://registry.npm.taobao.org

//改变全局默认下载地址
npm config set registry https://registry.npm.taobao.org

npm install cnpm -g --registry=https://registry.npm.taobao.org
```
cnpm 的坑：**package-lock.json** 是用来锁定安装时的包的版本号，如果之前用 npm 安装产生了package-lock.json，后
面再用cnpm来安装package.json、package-lock.json安装可能会跟你安装的依赖包不一致，这是
因为 cnpm 不受package-lock.json影响，只会根据package.json进行下载。

> 使用npm安装包时，需要去npm仓库获取，而npm仓库在国外，很不稳定，有时获取会失败。
> 为了解决这个问题，淘宝搭建了一个国内npm服务器，会定时拉取国外npm仓库内容，就是把国外的搬运到国内
> 你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

使用 `cnpm install <package-name>` 来代替 `npm install`。

#### [1.3 Yarn](#)
**Yarn** 是由 Facebook 推出的一个快速、可靠、安全的依赖管理工具。Yarn 解决了 npm 在某些方面的问题，比如速度和安全性。
它引入了锁文件（yarn.lock）确保依赖版本的一致性，并通过并行操作提高安装速度。使用方法与 npm 类似，例如
`yarn add <package-name>`。

**工作方式** 
- 使用yarn init命令初始化一个项目，这会在项目中生成一个package.json文件。
- 通过命令yarn add <package_name>添加任何包。
- 如果你有一个预配置的项目，并且想要安装依赖，可以运行yarn install命令，这将从NPM注册中心下载所有依赖并生成一个锁文件。

> yarn也是一个包管理器，它和npm其实没有本质区别，都是管理和安装包的。只是它解决了早期npm的一些问题比如：不支持离线模式、树形结构的依赖、依赖安装不确定性等。

```shell
#安装：
npm install -g yarn
#安装包：
yarn add [package]
#删除包：
yarn remove [package]
```
**yarn相当于早期npm优势**： 支持离线安装（npm@5已支持）、依赖扁平化结构（npm@3已支持）、依赖安装确定性yarn.lock（npm@5增加了package-lock.json）、安装速度快并行下载、安装失败自动重试。

**Yarn的优点**
- 更快安装速度：与NPM相比，Yarn在安装包时可以并行执行，从而加快了安装速度。
- 离线支持：Yarn利用本地缓存加速安装过程。它在全局位置存储包的缓存，可以在不同项目之间共享，这样不仅提高了速度，还实现了NPM所没有的离线支持功能。使用yarn cache dir命令可以查看Yarn保存其包缓存的目录。
- 更少的磁盘使用：Yarn采用平级依赖结构，避免了包的重复和嵌套，从而最小化了磁盘使用。
- Monorepo支持：Yarn还旨在通过称为 WORKSPACE 的特性支持monorepo。Monorepo是一个单一的仓库，其中存在多个包，每个包都有自己的package.json。Yarn Workspaces通过从中心位置安装所有包的依赖来简化依赖管理。

```shell
# 配置源

# 查看镜像源
yarn config get registry
# 绑定镜像源 （使用淘宝镜像）
yarn config set registry https://registry.npmmirror.com
# 删除镜像源（注意这里是 delete）
yarn config delete registry

# 安装依赖模块
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]

# 删除依赖模块
yarn remove [package]

# 更新依赖模块

yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

#### [1.4 pnpm](#)
**performant npm 意思为 高性能的 npm** ，旨在解决 npm 和 yarn 的一些性能和磁盘空间使用问题它通过使用硬链接和符号链接将一个版本的包存储在一个地方，而不是在每个项目中重复下载，从而节省磁盘空间pnpm 也提供了更快的安装速度和更严格的依赖关系管理，以避免意外的包版本冲突。

[pnpm 官网](https://www.pnpm.cn/)

|npm 命令|	pnpm 等价命令|
|:---|:---|
| npm install	|pnpm install 安装全部依赖|
| npm install 包名	|pnpm add (-D) 包名 安装指定包|
| npm uninstall 包名	|pnpm remove 包名 移除指定包|
| npm run 脚本	|pnpm 脚本 运行脚本|

#### [1.5 npx](#)
npx 是 npm 5.2.0 版本后引入的一个工具，用于执行 npm 包中的命令，而无需全局安装这些包。这对于需要一次性使用的工具特别有用。例如，如果你想创建一个新的 React 应用，你可以直接运行 npx create-react-app my-app，而不需要先全局安装 create-react-app。

**主要作用**:
- **无需全局安装**：通过 `npx`，你可以直接运行一个 npm 包，而不需要先全局安装这个包。例如，如果你想创建一个新的 React 应用，可以使用 `npx create-react-app my-app`，而无需先全局安装 `create-react-app`。
- **执行二进制文件**：`npx` 可以用来执行位于 `node_modules/.bin/` 目录下的任何可执行文件。这使得在脚本或命令中调用本地安装的工具变得非常简单。
- **临时使用工具**：对于那些不经常使用的工具，使用 `npx` 来临时调用比全局安装更加方便和节省磁盘空间。

**使用场景**: 

执行项目依赖中的命令：如果你的项目已经包含了某个包作为依赖项，那么你可以使用 npx 来执行该包提供的命令，而不需要全局安装。
```shell
npx some-package-command
```
测试包的功能：想要快速尝试某个 npm 包的功能，但又不想长期安装它？npx 让你可以一次性地运行任意包内的命令。
```shell
npx <package-name> [args]
```
避免版本冲突：在不同项目中可能需要不同版本的同一个工具，使用 npx 可以确保每个项目都使用其指定版本的工具，而不是全局版本。

