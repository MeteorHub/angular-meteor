{{#template name="tutorial.step_00.md"}}

开始开发我们的 Meteor-Angular 社交 APP。

在Step 0中，你将：

1. 熟悉最重要的源代码文件
2. 学习如何启动 Meteor server
3. 连接到 Angular 前端
4. 在浏览器中运行 APP

第一步 - 安装 Meteor！

打开命令行，执行下面的命令：

    $ curl https://install.meteor.com/ | sh

> 如果你使用的是 Windows，安装步骤请参考[这里](https://www.meteor.com/install)

现在，新建 APP - 在命令行执行下面命令：

    $ meteor create socially

来看下创建的结果，进入文件夹：

    $ cd socially

运行 APP：

    $ meteor

    => Started proxy
    => Started MongoDB.
    => Started your app.
    >=> App running at: http://localhost:3000/

浏览器中访问[http://localhost:3000/](http://localhost:3000/)，看看这神奇的 APP！

如你所见，我们创建了一个完整可运行的 APP，它包含了服务端和客户端！

默认的 Meteor APP 包含三个文件，一个 `js`，一个 `html`，一个 `css` 文件。每个文件都以新建项目时的名字命名。本例中就是 `socially`。

在本教程中，我们会添加自己的文件。所以，先把这三个文件删除：

    socially.css	socially.html	socially.js

现在，开始构建我们的 APP。首先，创建一个 `index.html` 文件，代码如下：

{{> DiffBox tutorialName="angular-meteor" step="0.1"}}

如你所见，没有 `<html>` 标签，也没有 `<head>` 标签，非常简单。

这是因为 Meteor 会用下面的方式组织客户端文件。

Meteor 扫描应用里所有HTML文件并把它们连接起来。

它会自动创建 `HTML`, `HEAD` 和 `BODY` 标签，然后把运行 APP 所需的内容全放进去。

然后，它会搜索包含 `HEAD` 或 `BODY` 标签的 HTML 文件，然后把标签的内容添加到主文件。


在本例中，Meteor 找到 `index.html` 文件，发现里面有 `BODY` 标签，就会把标签里的内容添加到生成的主文件的 `BODY` 标签中。
> (右键 -> 审查元素，审查生成的文件)



是时候使用 AngularJS了！

首先要做的就是，添加 AngularJS 包到项目中(在教程的后面部分会详细介绍 Meteor 包)


回到命令行，运行下面的命令：

    $ meteor add urigo:angular

这个包负责连通 Angular 和 Meteor，并把最新的 AngularJS 库引入到我们的 APP 中。

这样，我们就可以在 Meteor APP 中使用 AngularJS 的能力。

简单起见，在主文件夹中创建一个名为 `index.ng.html` 的新文件，它将作为我们的主 `HTML` 模板页面。

* 使用 `.ng.html` 文件扩展名，这样Blaze - Meteor 的模板系统 就不会编译、重写 AngularJS 表达式。

把 `index.html` 中的 `p` 标签移过来：

{{> DiffBox tutorialName="angular-meteor" step="0.2"}}

在 `index.html` 文件中引入 `index.ng.html` ：

{{> DiffBox tutorialName="angular-meteor" step="0.3"}}

但是，此时在浏览器中显示空白。这是因为我们需要**创建一个 Angular APP**，那接下来我们就创建一个。

* 要注意 - **路径必须是绝对路径，不能是相对路径！** 所以，如果 `index.ng.html` 位于 `client` 文件夹下，
就应该像下面这样：

```
    <div ng-include="'client/index.ng.html'"></div>
```

# AngularJS app

Angular APP 由独立的模块组成。先来创建我们的APP模块。

新建 `app.js` 文件。

现在，你可以看到 Meteor 另一个强大之处 - 无需到处引入这个文件。Meteor会扫描文件夹里所有文件，并自动引入它们。

Meteor 的目标之一就是打破客户端和服务端之间的隔阂，这样代码就可以在任何地方运行（后面会详细介绍）。
但是，我们只在客户端需要 Angular 的能力，怎么办？

有几种方式可以通知 Meteor 只在客户端、服务端或移动端执行代码，
我们先用最简单地方式 - [Meteor.isClient](http://docs.meteor.com/#/full/meteor_isserver)变量。

{{> DiffBox tutorialName="angular-meteor" step="0.4"}}

现在，`if` 语句中的代码只会运行在客户端。 

继续定义 Angular APP 模块。将其命名为 `socially` ，并添加 `angular-meteor` 模块作为依赖：

{{> DiffBox tutorialName="angular-meteor" step="0.5"}}

在 `index.html` 中的 `ng-app` 指令使用相同的APP name

{{> DiffBox tutorialName="angular-meteor" step="0.6"}}

运行 APP。

一切正常，现在，在 `index.ng.html` 中使用 Angular：

{{> DiffBox tutorialName="angular-meteor" step="0.7"}}

再次运行 APP，浏览器中会显示：

    Nothing here yet!

和其它的 Angular APP 一样，Angular 正确解析了表达式。 

# 实验
试着在 index.ng.html 中添加一个新的表达式，做个数学运算：

    <p>1 + 2 = {{dstache}} 1 + 2 }}</p>

# 总结
接下来，继续[step 1](/tutorial/step_01)，给我们的 APP 添加些内容。

{{/template}}
