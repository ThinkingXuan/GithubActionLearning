## React 项目发布到 GitHub Pages

下面是一个实例，通过 GitHub Actions 构建一个 React 项目，并发布到 GitHub Pages。发布后的参考网址为

[https://thinkingxuan.github.io/GithubActionLearning/](https://thinkingxuan.github.io/GithubActionLearning/)

#### 1 申请github密钥

这个示例需要将构建成果发到 GitHub 仓库，因此需要 GitHub 密钥。按照[官方文档](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)，生成一个密钥。然后，将这个密钥储存到当前仓库的`Settings/Secrets`里面。然后点击 `New repository secret`

![](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508105436.png)

上图是储存秘密的环境变量的地方。环境变量的名字可以随便起，这里用的是`ACCESS_TOKEN`。如果你不用这个名字，后面脚本里的变量名也要跟着改。

#### 2 本地生成 React应用

本地计算机使用[`create-react-app`](https://github.com/facebook/create-react-app)，生成一个标准的 React 应用。

```shell
$ npx create-react-app GithubActionLearning
$ cd GithubActionLearning
```

> 如果有没有安装npx，直接下载我的代码仓库生产好的就是可以的

然后，打开`package.json`文件，加一个`homepage`字段，表示该应用发布后的根目录：

```
"name": "GithubActionLearning",
"homepage": "https://[username].github.io/GithubActionLearning",
```

比如我的地址username 为 thinkingxuan  

```
  "homepage": "https://thinkingxuan.github.io/GithubActionLearning",
```

#### 3 编写workflows文件

第在这个仓库的`.github/workflows`目录，生成一个 workflow 文件，名字可以随便取，这个示例是`ci.yaml`。

我们选用一个别人已经写好的 action：[JamesIves/github-pages-deploy-action](https://github.com/marketplace/actions/deploy-to-github-pages)，它提供了 workflow 的范例文件，稍微修改就行了。

````yaml
name: GitHub Actions Build and Deploy Demo
on:
  push:
    branches:
      - main
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2.3.1 # If you're using actions/checkout@v2 you must set persist-credentials t
        with:
          persist-credentials: false
      - name: Install and Build
        run: |
          npm install
          npm run-script build
      - name: Deploy
        uses: JamesIves/github-pages-deploy-action@4.1.1
        with:
          ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
          BRANCH: gh-pages
          folder: build # The folder the action should deploy.
````

> 需要 主要触发的分支为  on.push.branches = main   (如果你是master请自行修改)
>
> 添加 ACCESS_TOKEN字段。

上面这个 workflow 文件的要点如下。

> 1. 整个流程在`main`分支发生`push`事件时触发。
> 2. 只有一个`job`，运行在虚拟机环境`ubuntu-latest`。
> 3. 第一步是获取源码，使用的 action 是`actions/checkout`。
> 4. 第二步是构建和部署，使用的 action 是`JamesIves/github-pages-deploy-action`。
> 5. 第二步需要四个环境变量，分别为 GitHub 密钥、发布分支、构建成果所在目录、构建脚本。其中，只有 GitHub 密钥是秘密变量，需要写在双括号里面，其他三个都可以直接写在文件里。

#### 4 上传代码到github

```shell
$ git add .
$ git commit -m "deploy react app"
$ git push
```

等待Action完成：

![](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508111238.png)

#### 5 开启Github Pages

代码上传完成后，会通过action自动编译部署React应用，并把生成的静态文件放在 `gh-pages`上![](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508110738.png)

我们需要手动开启`Gtihub Pages` ，并且修改`Source` 到 `Branch: gh-pages`，可能需要稍等片刻才能生效。

点击生成的网页地址：

![](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508111001.png)