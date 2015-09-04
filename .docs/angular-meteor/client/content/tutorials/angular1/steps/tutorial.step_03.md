{{#template name="tutorial.step_03.md"}}
{{> downloadPreviousStep stepName="step_02"}}

好了，我们有了一个不错的客户端程序，它创建并呈现了自己的数据。

如果我们用的是其它框架而不是 Meteor，接下来就需要实现一系列的 REST 接口来把服务端和客户端联系起来。还有，需要创建一个数据库和连接数据库的函数。

我们还没有谈到实时性，在这种情况下，可能需要添加 sockets，一个用来缓存的本地数据库以及处理延迟补偿（或者就直接忽略这些功能，开发一个不那么好的现代 APP）

但是，幸运的是，我们选用了 Meteor！

Meteor 使编写分布式的客户端代码变得简单，就像是使用本地数据库一样。

每一个 Meteor 客户端都包含一个内存数据库。服务端发布 JSON 文档集合，客户端订阅这些集合来管理客户端缓存。如果集合中的文档发生变化，服务端自动更新每个客户端缓存。

这引入了一个新概念 - **Full Stack Reactivity**

用 Angular 的语言，我们可以称之为 **3 way data binding**。

Meteor 中通过 `Mongo.Collection` 类来处理数据。用它来声明 MongoDB 集合以及操作数据。

由于 minimongo (Meteor 客户端的 Mongo 模拟器)的存在，所以客户端代码和服务端代码都可以使用 `Mongo.Collection`。

# 声明一个集合

首先，定义一个 parties 集合,用于存储所有的 parties。

{{> DiffBox tutorialName="angular-meteor" step="3.1"}}

把上面的代码添加到 `app.js` 文件的顶部。

注意，代码位于 `isClient` 语句之外。

也就是说，这个集合以及它的操作会运行在客户端(minimongo)和服务端(Mongo)。你只需写一次，Meteor 会自动在两者之间进行同步。

# 绑定到 Angular

创建集合之后，客户端需要订阅它的变化并绑定到 Angular 数组 parties。

我们将用内置的称为 [$meteor.collection](/api/meteorCollection) 的 angular-meteor 服务来绑定它们。

在 `PartiesListCtrl` 控制器内部，用下面的命令替换 `$scope.parties` 的声明：

{{> DiffBox tutorialName="angular-meteor" step="3.2"}}

第七行声明了一个新的 `$scope.parties` 变量(不再使用类似 `$scope.parties = [];` 的方式)，并把它绑定到了 Mongo 集合 Parties。

还需要利用依赖注入将 `$meteor` 服务添加到控制器。

`app.js` 文件如下：

{{> DiffBox tutorialName="angular-meteor" step="3.3"}}

现在 `$scope.parties` 变量发生的任何变化都会自动保存到本地 minimongo 并实时同步到 MongoDB 服务器和其它客户端。

但是现在，集合中还没有任何数据，先来添加一些。我们用之前的数据初始化服务器。

将下面代码添加到 `app.js` 文件末尾：

{{> DiffBox tutorialName="angular-meteor" step="3.4"}}

你可能已经看明白了，新增的代码只运行在服务端，当 Meteor 启动时，它用示例数据初始化数据库。

运行 APP，你就会看到 parties 的列表。

在下一章节，我们会看到操作数据，保存并发布变化到服务端是多么的简单。通过这样做，所有连接上的客户端也会自动更新。

# 通过 console 插入数据

集合中的项称为文档。使用服务端数据库 console 来插入一些文档到集合中。
打开一个新的终端窗口，进入项目目录，执行下面命令：

    meteor mongo

这会打开 APP 的本地开发数据库的 console。输入：

    db.parties.insert({ name: "A new party", description: "From the mongo console!" });

在浏览器中，你会发现 APP 界面立即更新，显示了新插入的 party。你可能注意到了，我们还没写任何有关连接服务端数据库和客户端的代码 - 完全都是自动的。

通过数据库 console 插入其它的 parties。

接下来进行删除。输入下面的命令，查看所有 parties 和它们的属性：

    db.parties.find({});

现在，选择一个你想删除的 party,复制它的 id 属性。然后，使用 id 删除它（用你实际的 id 值替换 N4KzMEvtm4dYvk2TF）：

    db.parties.remove( {"_id": "N4KzMEvtm4dYvk2TF"});

再一次，看到 APP 界面立即更新，被删除的 party 不见了。

试着在 console 执行更多的在操作，例如更新对象等等。

在下一节，将看到如何给 APP UI 添加功能，这样我们就无需通过 console 来插入数据了。

# 总结

在本节中你看到了，创建一个连接客户端、服务端以及其它客户端的 APP 是多么的简单和快速。

{{/template}}
