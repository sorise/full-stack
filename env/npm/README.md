## [Node Package Manager（npm） ](#)
> npm（Node Package Manager）是Node.js的包管理器，用于管理和安装Node.js程序的依赖库。npm允许开发者搜索、安装、卸载和管理不同版本的软件包或模块，这些软件包可以是其他人开发的，也可以是你自己开发并希望与他人共享的。

----
### [1. npm 的一些主要功能](#)
要开始使用npm，首先需要在你的计算机上安装Node.js，因为npm通常会随Node.js一起安装。
- **包管理**：通过npm install命令，可以轻松地安装需要的包到你的项目中。同时，npm会自动处理所有相关的依赖关系，确保所有必需的包都被正确安装。
- **版本控制**：npm允许你指定特定版本的包，或者使用版本范围来安装最新的稳定版。这有助于保持项目的兼容性和稳定性。
- **全局和本地安装**：你可以选择将包安装为全局包，这意味着它们可以在你的系统上任何地方被访问；也可以选择作为项目的一部分进行本地安装。
- **发布和分享包**：npm不仅仅是一个包管理工具，它也是一个庞大的社区平台。开发者可以创建自己的包，并通过npm将其发布给全世界的用户。
- **脚本运行**：通过package.json文件中的scripts字段，npm允许你在项目中定义和运行自定义脚本，比如构建、测试、启动服务等。
- **文档和搜索**：npm网站提供了一个强大的搜索引擎，可以帮助你找到你需要的包，并提供了详细的文档支持。

官方网站：[NPM 官方网站 https://www.npmjs.com/](https://www.npmjs.com/)，[NPM 中文文档 https://www.npmrc.cn/](https://www.npmrc.cn/) 。

#### [1.1 创建项目命令 npm init](#)
当你在一个新的项目中运行这个命令时，它会引导你创建一个 package.json 文件，该文件是项目的配置文件，包含了有关项目的元数据以及依赖关系等信息。

```
npm init <package-spec> (等同于 `npx <package-spec>`)
npm init <@scope> (等同于 `npx <@scope>/create`)

aliases: create, innit
```
**命令使用流程**：
```shell
npm init <initializer>
```
**创建 package.json**:
* npm init 会生成一个名为 package.json 的文件，此文件位于项目的根目录下。
* 这个文件定义了项目的基本信息，比如名称、版本、描述、入口文件（main）、脚本、依赖库（dependencies）和开发者依赖库（devDependencies）等。

**交互式提问**:
* 默认情况下，npm init 会通过一系列问题来收集关于你的项目的初始信息。例如，它会询问包的名称、版本号、描述、作者等。
* 如果你不喜欢这种交互方式，可以使用 npm init --yes 或 npm init -y 来跳过提问环节，并使用默认值自动创建 package.json。

**设置默认值**:
* 你可以通过环境变量或 .npm-init.js 文件来为这些字段设置默认值，这样在你运行 npm init 时就不必每次都手动输入相同的信息。

**使用模板**:
* 从 npm 5.2.0 开始，**npm init 支持使用特定的初始化包作为模板**。例如，npm init react-app my-app 将使用 Create React App 工具来创建一个新的 React 应用程序。

**安装依赖**:
* 在创建完 package.json 后，你可以开始添加项目所需的依赖库。使用 `npm install <package-name> --save` 可以安装依赖并将它添加到 dependencies 中，而 `--save-dev` 则是添加到 devDependencies 中。

**运行脚本**:
* package.json 中的 scripts 字段允许你定义自定义的 npm 脚本，这使得你可以轻松地执行任务，如构建代码、运行测试或启动服务器。

**版本控制**
* package.json 文件对于版本控制系统（如 Git）来说非常重要，因为它确保了团队成员之间的依赖一致性。

使用以下命令创建一个新的基于 React 的项目 create-react-app：
```shell
npm init react-app ./my-react-app
```

**使用模板的特例**

---
这里的 如果 `initializer` 是一个名为 `create-<initializer>` 的 npm 包，它将被 npm-exec 安装，然后执行它的主 bin -- 大概是创建或更新 package.json，并运行其他与初始化相关的操作。

* `npm init foo` -> `npm exec create-foo`
* `npm init @usr/foo` -> `npm exec @usr/create-foo`
* `npm init @usr` -> `npm exec @usr/create`
* `npm init @usr@2.0.0` -> `npm exec @usr/create@2.0.0`
* `npm init @usr/foo@2.0.0` -> `npm exec @usr/create-foo@2.0.0`

如果用户已经全局安装了 `create-<initializer>` 包，则 npm init 将使用该包。如果你想让npm使用最新版本，或者其他特定版本，你必须指定它:

#### [1.2 创建项目命令 npm run](#)
node运行的命令为 `node xxx.js`。 npm run 是 npm 提供的一个命令，用于执行在 package.json 文件的 scripts 字段中定义的自定义脚本。

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js",
    "build": "webpack --config webpack.prod.js",
    "test": "jest"
  }
}
```
在这个例子中，我们定义了三个脚本：start、build 和 test。

要执行这些脚本，你可以在命令行中使用 npm run `<script-name>` 来调用它们。例如，如果你想要启动应用程序，可以输入
```shell
npm run start
```
**内置脚本** npm 提供了一些内置的命令，可以直接使用而不需要加上 run，比如：
* npm start: 默认运行 start 脚本。
* npm test: 默认运行 test 脚本。
* npm stop: 如果定义了 stop 脚本，则会运行它。
* npm restart: 先运行 stop 脚本（如果存在），然后运行 start 脚本

参数传递
* 你还可以向 npm run 命令传递参数。例如，如果你有一个带有选项的脚本，你可以这样调用它：

```shell
npm run build -- --production
```

### [2. 掌握命令](#)

| 命令                  | 说明                       | 命令            | 说明                      |
|:--------------------|:-------------------------|:--------------|:------------------------|
| npm                 | javascript 包管理器          | npm access    | 设置发布软件包的访问权限            |
| npm adduser         | 添加注册中心用户帐户               | npm audit     | 运行安全检查                  |
| npm bin             | 显示 npm 的 bin 文件夹         | npx           | 优先本地包中执行命令              |
| npm pkg             | 管理你的 package.json        | npm bugs web  | 浏览器中报告程序包的错误            |
| npm cache           | 操作包缓存                    | npm ci        | 使用干净的面板安装项目             |
| npm completion      | 的制表符补全                   | npm docs      | 在 Web 浏览器中打开包的文档        |
| npm config          | 管理 npm 配置文件              | npm doctor    | 检查您的环境                  |
| npm dedupe          | 减少重复                     | npm edit      | 编辑已安装的软件包               |
| npm deprecate       | 弃用软件包的版本                 | npm exec      | 从包运行命令                  |
| npm diff            | 注册中心差异对比命令               | npm explain   | 解释已安装的软件包               |
| npm dist-tag        | 修改包分发标签                  | npm help      | 获取有关 npm 的帮助            |
| npm test            | 测试脚本                     | npm explore   | 在指定的已安装软件包的目录中生成子 shell |
| npm find-dupes      | 在包树中查找重复项                | npm fund      | 获取捐赠信息                  |
| npm help-search     | 搜索 帮助文档                  | npm hook      | 管理注册中心钩子                |
| npm init            | 创建 package.json 文件       | npm logout    | 退出注册中心                  |
| npm install         | 安装包                      | npm ls        | 列出已安装的软件包               |
| npm install-ci-test | 安装一个干净的项目并运行测试           | npm org       | 管理组织                    |
| npm install-test    | 安装软件包并运行测试               | npm profile   | 更改注册中心配置文件的设置           |
| npm link            | 创建软链接到包文件夹               | npm prune     | 删除无关的包                  |
| npm outdated        | 检查过时的包                   | npm publish   | 发布包                     |
| npm owner           | 管理包所有者                   | npm query     | 检索过滤的包列表                |
| npm pack            | 从包中创建一个 tarball          | npm rebuild   | 重建一个包                   |
| npm ping Ping       | 注册中心                     | npm prefix    | 显示前缀                    |
| npm repo            | 在浏览器中打开源码仓库页面            | npm token     | 管理您的身份验证令牌              |
| npm restart         | 重启一个包                    | npm uninstall | 卸载包                     |
| npm root            | 显示 根目录                   | npm unpublish | 从注册中心中删除软件包             |
| npm run-script      | 运行任意包脚本                  | npm unstar    | 删除你喜欢的包                 |
| npm search          |  搜索包                     | npm team      | 管理组织团队和团队成员             |
| npm set-script      | 在 package.json 的脚本部分设置任务 | npm update    | 更新包                     |
| npm shrinkwrap      | 锁定要发布的依赖版本               | npm stop      | 停止脚本                    |
| npm star            | 标记您喜欢的包                  | npm start     | 开始脚本                    |
| npm version         | 查看版本| npm whoami    | 显示 npm 用户名              |
| npm view           | 查看注册中心信息|               |                         |

