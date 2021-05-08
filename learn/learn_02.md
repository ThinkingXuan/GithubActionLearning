## workflow 文件

`GitHub Actions` 的配置文件叫做 **workflow**文件，存放在代码仓库的`.github/workflows/`目录下。比如写一个`first.yaml`文件，存储的目录就是`.github/workflows/first.yaml`

**workflow/**下的文件采用 `YAML` 格式，文件名可以任意取，但是后缀名统一为`.yml`或者`yaml`，比如`foo.yml`。一个库可以有多个 `workflow` 文件。GitHub 只要发现`.github/workflows`目录下里面有`.yml`文件，就会自动运行该文件。

**workflow/**下的文件的配置字段非常多，详见官方文档。下面是一些基本字段。

#### （1）name

`name`字段是 **workflow** 的名称。如果省略该字段，默认为当前 **workflow** 的文件名。

```yaml
name: GitHub Actions Test
```

#### （2）on

on字段指定触发 **workflow** 的条件，通常是某些事件。

```yaml
on: push
```


上面代码指定，`push`事件触发 **workflow**。

on字段也可以是事件的数组。

```yaml
on: [push, pull_request]
```

上面代码指定，`push`事件或`pull_request`事件都可以触发 **workflow**。

完整的事件列表，请查看[官方文档](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)。除了代码库事件，GitHub Actions 也支持外部事件触发，或者定时运行。

#### （3）on. <push|pull_request>. <tags|branches>

指定触发事件时，可以限定分支或标签。

```yaml
on:
  push:
    branches:
      - master
```


上面代码指定，只有`master`分支发生`push`事件时，才会触发 **workflow**。

#### （4）jobs.<job_id>.name

**workflow** 文件的主体是`jobs`字段，表示要执行的一项或多项任务。

`jobs`字段里面，需要写出每一项任务的`job_id`，具体名称自定义。`job_id`里面的`name`字段是任务的说明。

```yaml
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```


上面代码的jobs字段包含两项任务，`job_id`分别是`my_first_job`和`my_second_job`。

#### （5）jobs.<job_id>.needs

`needs`字段指定当前任务的依赖关系，即运行顺序。

```yaml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```


上面代码中，`job1`必须先于`job2`完成，而`job3`等待`job1`和`job2`的完成才能运行。因此，这个 **workflow** 的运行顺序依次为：`job1`、`job2`、`job3`。

#### （6）jobs.<job_id>.runs-on

`runs-on`字段指定运行所需要的虚拟机环境。它是必填字段。目前可用的虚拟机如下。

```yaml
ubuntu-latest，ubuntu-18.04或ubuntu-16.04
windows-latest，windows-2019或windows-2016
macOS-latest或macOS-10.14
```


下面代码指定虚拟机环境为ubuntu-18.04。

```yaml
runs-on: ubuntu-18.04
```


#### （7）jobs.<job_id>.steps

`steps`字段指定每个 `Job` 的运行步骤，可以包含一个或多个步骤。每个步骤都可以指定以下三个字段。

```yaml
jobs.<job_id>.steps.name：步骤名称。
jobs.<job_id>.steps.run：该步骤运行的命令或者 action。
jobs.<job_id>.steps.env：该步骤所需的环境变量。
```

下面是一个完整的 **workflow** 文件的范例。

```yaml
name: Greeting from Mona
on: push

jobs:
  my-job:
    name: My Job
    runs-on: ubuntu-latest
    steps:
    - name: Print a greeting
      env:
        MY_VAR: Hi there! My name is
        FIRST_NAME: Mona
        MIDDLE_NAME: The
        LAST_NAME: Octocat
      run: |
        echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
```

上面代码中，steps字段只包括一个步骤。该步骤先注入四个环境变量，然后执行一条 Bash 命令，当代码`push`时触发这个**workflow**。

 运行测试

先来看看项目目录：

```
GithubActionLearn/
|-- .github
|   -- workflows
|       -- first.yaml          // yaml文件
|-- README.md
-- learn
    |-- learn_01.md
    -- learn_02.md
```

我们使用`git`把项目提交到`github`，看看效果

![图1](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508093442.png)

点击`Action`，

![图2](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508093551.png)

可以看到`All workflows`里面多了一条信息，点击进入查看详细，然后再次点击 `My Job`

![图3](https://cdn.jsdelivr.net/gh/ThinkingXuan/HexoStaticImage/img/20210508093736.png)

可以看到命令已经执行成功。

----
以上内容来源于：

[GitHub Actions 入门教程](https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)