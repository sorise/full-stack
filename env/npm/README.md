## [Node Package Manager（npm） ](#)
> npm（Node Package Manager）是Node.js的包管理器，用于管理和安装Node.js程序的依赖库。npm允许开发者搜索、安装、卸载和管理不同版本的软件包或模块，这些软件包可以是其他人开发的，也可以是你自己开发并希望与他人共享的。

----
### [1. npm的一些主要功能](#)
要开始使用npm，首先需要在你的计算机上安装Node.js，因为npm通常会随Node.js一起安装。
- **包管理**：通过npm install命令，可以轻松地安装需要的包到你的项目中。同时，npm会自动处理所有相关的依赖关系，确保所有必需的包都被正确安装。
- **版本控制**：npm允许你指定特定版本的包，或者使用版本范围来安装最新的稳定版。这有助于保持项目的兼容性和稳定性。
- **全局和本地安装**：你可以选择将包安装为全局包，这意味着它们可以在你的系统上任何地方被访问；也可以选择作为项目的一部分进行本地安装。
- **发布和分享包**：npm不仅仅是一个包管理工具，它也是一个庞大的社区平台。开发者可以创建自己的包，并通过npm将其发布给全世界的用户。
- **脚本运行**：通过package.json文件中的scripts字段，npm允许你在项目中定义和运行自定义脚本，比如构建、测试、启动服务等。
- **文档和搜索**：npm网站提供了一个强大的搜索引擎，可以帮助你找到你需要的包，并提供了详细的文档支持。

[NPM 官方网站 https://www.npmjs.com/](https://www.npmjs.com/)，[NPM 中文文档 https://www.npmrc.cn/](https://www.npmrc.cn/) 。
### [2. 掌握命令](#)

| 命令             | 说明                | 命令             | 说明               |
|:---------------|:------------------|:---------------|:-----------------|
| npm            | javascript 包管理器   | npm access     | 设置发布软件包的访问权限     |
| npm adduser    | 添加注册中心用户帐户        |  npm audit  | 运行安全检查           |
|npm bin| 显示 npm 的 bin 文件夹  | npx  | 优先本地包中执行命令       |
|npm pkg | 管理你的 package.json |npm bugs web | 浏览器中报告程序包的错误     |
|npm cache| 操作包缓存             |npm ci| 使用干净的面板安装项目      |
|npm completion| 的制表符补全            |npm docs | 在 Web 浏览器中打开包的文档 |
|npm config | 管理 npm 配置文件       |npm doctor | 检查您的环境           |
|npm dedupe | 减少重复              |npm edit| 编辑已安装的软件包        |
|npm deprecate| 弃用软件包的版本          |npm exec| 从包运行命令           |
|npm diff| 注册中心差异对比命令        |npm explain| 解释已安装的软件包        |
|npm dist-tag| 修改包分发标签           |npm help|  获取有关 npm 的帮助    |
|npm test|  测试脚本             |npm explore|在指定的已安装软件包的目录中生成子 shell|
|npm find-dupes|在包树中查找重复项|npm fund|获取捐赠信息|
|npm help-search |搜索 帮助文档|npm hook| 管理注册中心钩子|
|npm init| 创建 package.json 文件
|npm install| 安装包
|npm install-ci-test| 安装一个干净的项目并运行测试
|npm install-test |安装软件包并运行测试
|npm link| 创建软链接到包文件夹
npm logout 退出注册中心
npm ls 列出已安装的软件包
npm org 管理组织
npm outdated 检查过时的包
npm owner 管理包所有者
npm pack 从包中创建一个 tarball
npm ping Ping 注册中心

npm prefix 显示前缀
npm profile 更改注册中心配置文件的设置
npm prune 删除无关的包
npm publish 发布包
npm query 检索过滤的包列表
npm rebuild 重建一个包
npm repo 在浏览器中打开源码仓库页面
npm restart 重启一个包
npm root 显示 根目录
npm run-script 运行任意包脚本
npm search 搜索包
npm set-script 在 package.json 的脚本部分设置任务
npm shrinkwrap 锁定要发布的依赖版本
npm star 标记您喜欢的包
npm start 开始脚本
npm stop 停止脚本
npm team 管理组织团队和团队成员
npm token 管理您的身份验证令牌
npm uninstall 卸载包
npm unpublish 从注册中心中删除软件包
npm unstar 删除你喜欢的包
npm update 更新包
npm version 查看版本
npm view 查看注册中心信息
npm whoami 显示 npm 用户名
