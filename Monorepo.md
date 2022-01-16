## Monorepo

https://www.jianshu.com/p/dafc2052eedc

https://www.jianshu.com/p/c10d0b8c5581

### 什么是monorepo？

Monorepo(monolithic repository) 是管理项目代码的一个方式，指在一个项目仓库 (repo) 中管理多个模块/包 (package)，不同于常见的每个模块建一个 repo。

目前不少大型开源项目采用了这种方式，如 `Babel`、`React`、`Vue` 等。monorepo 管理代码只要搭建一套脚手架，就能管理（构建、测试、发布）多个 package。

在项目的第一级目录的内容以脚手架为主，主要内容都在 `packages` 目录中、分多个 package 进行管理。目录结构大致如下：

```go
├── packages
|   ├── pkg1
|   |   ├── package.json
|   ├── pkg2
|   |   ├── package.json
├── package.json
```

![img](https://upload-images.jianshu.io/upload_images/15825758-e228917fd3a382bc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

此时会有一个问题，虽然拆分子 npm 包管理项目简单了很多，但是当仓库内容有关联时，调试变得困难。**所以理想的开发环境应该是只关心业务代码，可以直接跨业务复用而不关心复用方式，调试时所有代码都在源码中。**

目前最常见的 monorepo 解决方案是 lerna 和 yarn 的 workspaces 特性。用 yarn 处理依赖问题，lerna处理发布问题。

### 优点

**可见性（Visibility）**：每个人都可以看到其他人的代码，这样可以带来更好的协作和跨团队贡献——不同团队的开发人员都可以修复代码中的bug，而你甚至都不知道这个bug的存在。

**更简单的依赖关系管理（Simpler dependency management）**：共享依赖关系很简单，因为所有模块都托管在同一个存储库中，因此都不需要包管理器。

**唯一依赖源（Single source of truth）**：每个依赖只有一个版本，意味着没有版本冲突，没有依赖地狱。

**一致性（Consistency）**：当你把所有代码库放在一个地方时，执行代码质量标准和统一的风格会更容易。

**共享时间线（Shared timeline）**：API或共享库的变更会立即被暴露出来，迫使不同团队提前沟通合作，每个人都得努力跟上变化。

**原子提交（Atomic commits）**：原子提交使大规模重构更容易，开发人员可以在一次提交中更新多个包或项目。

**隐式CI（Implicit CI）**：因为所有代码已经统一维护在一个地方，因此可以保证持续集成[3]。

**统一的CI/CD（Unified CI/CD）**：可以为代码库中的每个项目使用相同的CI/CD[4]部署流程。

**统一的构建流程（Unified build process）**：代码库中的每个应用程序可以共享一致的构建流程[5]。

### 不足

随着单一代码库的发展，我们在版本控制工具、构建系统和持续集成流水线方面达到了设计极限。这些问题可能会让一家公司走上多代码库的道路：

- **性能差（Bad performance）**：单一代码库难以扩大规模，像git blame这样的命令可能会不合理的花费很长时间执行，IDE也开始变得缓慢，生产力受到影响，对每个提交测试整个repo变得不可行。
- **破坏主线（Broken main/master）**：主线损坏会影响到在单一代码库中工作的每个人，这既可以被看作是灾难，也可以看作是保证测试既可以保持简洁又可以跟上开发的好机会。
- **学习曲线（Learning curve）**：如果代码库包含了许多紧密耦合的项目，那么新成员的学习曲线会更陡峭。
- **大量的数据（Large volumes of data）**：单一代码库每天都要处理大量的数据和提交。
- **所有权（Ownership）**：维护文件的所有权更有挑战性，因为像Git或Mercurial这样的系统没有内置的目录权限。
- **Code reviews**：通知可能会变得非常嘈杂。例如，GitHub有有限的通知设置，不适合大量的pull request和code review。

### 提交规范

在构建和发布之前还需要做一些关于代码提交的配置

#### commitizen && cz-lerna-changelog

commitizen 是用来格式化 git commit message 的工具，它提供了一种问询式的方式去获取所需的提交信息。

cz-lerna-changelog 是专门为 Lerna 项目量身定制的提交规范，在问询的过程，会有类似影响哪些 package 的选择。

### 工具

#### 构建系统

许多大公司的**构建系统**都是开源的：

- Bazel[20]：由谷歌发布，部分基于他们自己的构建系统（Blaze）。Bazel支持多种语言，并支持大规模构建和测试。
- Buck[21]: Facebook开源的快速构建系统，支持在多种语言和平台上进行不同的构建。
- Pands[22]：Pands构建系统是与Twitter、Foursquare和Square合作创建的。目前只支持Python，后续还将支持更多的语言。
- RushJS[23]：微软用于JavaScript的可扩展的单一代码库管理器，能够从一个代码库构建和部署多个包。

Monorepos正在获得越来越多的关注，尤其是在JavaScript方面，如下项目所示：

- Lerna[24]：JavaScript的单一代码库管理器，可以与React、Angular或Babel等流行框架集成。
- Yarn workspace[25]：用一个命令在多个地方安装和更新Node.js的依赖项。
- ultra-runner[26]：JavaScript单一代码库管理脚本，支持Yarn、pnpm和Lerna插件，支持并行构建。
- Monorepo builder[27]：通过单一代码库安装和更新PHP包。

**扩展代码库**

源代码控制是单一代码库的另一个痛点，这些工具可以帮助您扩展存储库：

- Virtual Filesystem for Git（VFS）[28]：为Git添加流支持。VFS可以根据需要从Git仓库下载对象。这个项目最初是为了管理Windows代码库（最大的Git仓库）而创建的，只能在Windows上使用，但已经宣布要支持MacOS。
- Large File Storage[29]：Git的开源扩展，为大文件提供了更好的支持。安装了这个扩展，就可以跟踪任何类型的文件，并将它们无缝的上传到云存储中，从而释放代码库的空间，使push和pull的速度更快。
- Mercurial[30]：Git的替代方案，Mercurial是一个分布式版本控制工具，专注于速度。Facebook使用Mercurial，多年来已经发布了许多提高速度的补丁[31]。
- Git CODEOWNERS[32]：允许定义哪个团队拥有代码库中的子目录，当有人提交pull request或push代码到受保护的分支时，代码所有者会自动被要求进行审查。GitHub和GitLab都支持这一特性。

## Multirepo

