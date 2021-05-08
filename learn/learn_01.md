## 一、GitHub Actions 是什么？
大家知道，持续集成由很多操作组成，比如抓取代码、运行测试、登录远程服务器，发布到第三方服务等等。GitHub 把这些操作就称为 `actions`。

很多操作在不同项目里面是类似的，完全可以共享。`GitHub` 注意到了这一点，想出了一个很妙的点子，允许开发者把每个操作写成独立的脚本文件，存放到代码仓库，使得其他开发者可以引用。

如果你需要某个 `action`，不必自己写复杂的脚本，直接引用他人写好的 `action` 即可，整个持续集成过程，就变成了一个 `actions` 的组合。这就是` GitHub Actions` 最特别的地方。

GitHub 做了一个官方市场(`Marketplace`)，可以搜索到他人提交的 `actions`。另外，还有一个 awesome actions 的仓库，也可以找到不少 `action`。

![img.png](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210507215545.png)

上面说了，每个 `action `就是一个独立脚本，因此可以做成代码仓库，使用`userName/repoName`的语法引用 `action`。比如，`actions/setup-node`就表示`github.com/actions/setup-node`这个仓库，它代表一个 `action`，作用是安装 Node.js。事实上，GitHub 官方的 actions 都放在 github.com/actions 里面。

既然  `action` 是代码仓库，当然就有版本的概念，用户可以引用某个具体版本的 `action`。下面都是合法的 action 引用，用的就是 Git 的指针概念，详见官方文档。


### 二、基本概念

GitHub Actions 有一些自己的术语。

（1）**workflow** （工作流程）：持续集成一次运行的过程，就是一个 workflow。

（2）**job** （任务）：一个 workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。

（3）**step**（步骤）：每个 job 由多个 step 构成，一步步完成。

（4）**action** （动作）：每个 step 可以依次执行一个或多个命令（action）。

----
以上内容来源于：

[GitHub Actions 入门教程](https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)