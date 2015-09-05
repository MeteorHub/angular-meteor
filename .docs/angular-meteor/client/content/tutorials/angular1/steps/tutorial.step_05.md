{{#template name="tutorial.step_05.md"}}
{{> downloadPreviousStep stepName="step_04"}}

在这一节，你将会学习如何创建 layout 模板，以及使用 Angular 模块 `ui-router` 添加路由，开发多视图 APP。

本节目标：

* 当导航到 `index.html` 时，会跳转到 `index.ng.html/parties` 并显示 party 列表。
* 当点击某个 party 的链接时，跳转到对应的详情页。

# 依赖

路由功能由 [ui-router](https://github.com/angular-ui/ui-router) 模块提供，它并没有包含在 Angular 核心框架中。

我们通过 [Meteor 官方包平台](https://atmospherejs.com/angularui/angular-ui-router)安装 ui-router

执行下面的命令：

    meteor add angularui:angular-ui-router

在 `app.js` 中，添加 ui-router 作为依赖：

{{> DiffBox tutorialName="angular-meteor" step="5.1"}}

# 多视图，路由和 Layout 模板

我们的 APP 慢慢的变得复杂起来。到目前为止，APP 展示给用户的只是单一视图(parties 的列表)，所有的模板代码都位于 `index.ng.html` 文件。

下一步要做的就是，添加用于展示每个 party 详情的视图。

要添加详情页，我们可以选择扩展 `index.ng.html` 文件，将模板代码全都放进去，但是它很快就会变得混乱不堪。

而我们的方法是：将 `index.html` 模板改为 "layout 模板"。作为我们 APP 所有视图的基础。其它的 "partial 模板"会根据当前的路由，选择性的引入到 layout 模板。

Angular 中的路由通过 [$stateProvider](https://github.com/angular-ui/ui-router/wiki) 来声明，它是 $state 服务的 provider。
$state 服务使关联控制器、视图模板和当前URL变得简单。
通过这个特性可以实现深度链接，这样就可以利用浏览器历史(前进和后退)和书签功能。

# 模板

$state 服务通常结合 uiView 指令一起使用。uiView 指令的作用是引入当前路由对应的模板到 layout 模板中。
这很适合我们的 `index.ng.html` 模板。

创建一个新的 HTML 文件，叫做 `parties-list.html`，然后把 `index.ng.html` 文件中的 parties 列表代码拷贝过去：

{{> DiffBox tutorialName="angular-meteor" step="5.2"}}

代码只有一点变化：

- 给 parties 标题增加了链接(会链接到详情页)

回到 `index.html`中，用 `ui-view` 指令替换对应内容：

{{> DiffBox tutorialName="angular-meteor" step="5.3"}}

我们做了三件事：

1. 用 ui-view 替换 ng-include (ui-view 负责根据 URL 引入正确的模板)
2. 增加了 `h1` 标签，链接到 parties 页面
3. 还在头部增加了 `base` 标签(使用 HTML5 location mode 需要) 

现在，可以把 `index.ng.html` 删掉了。它已经没有用处了。

我们来给 party 详情页增加一个占位符。新建文件，名为 `party-details.ng.html`，代码如下：

{{> DiffBox tutorialName="angular-meteor" step="5.4"}}

现在，先用上面的代码作为占位符。稍后，再来填充详情。

# 定义路由

现在，我们来配置路由。在 `app.js` 文件中，Angular APP 定义之后，增加配置代码：

{{> DiffBox tutorialName="angular-meteor" step="5.5"}}

用到了 Angular APP 的 .config() 方法，我们请求 `$stateProvider` 注入到 config 函数，然后使用 state 方法定义路由。

APP 路由定义如下：

* **('/parties')**: 当 URL hash fragment 是 /parties 时，显示 parties 列表。Angular 会用 parties-list.ng.html 模板和 PartiesListCtrl 控制器构建视图。
* **('/parties/:partyId')**: 当 URL hash fragment 匹配 '/parties/:partyId' 时，其中 :partyId 是 URL 中的变量。Angular 会用 party-details.ng.html 模板和 PartyDetailsCtrl 控制器构建视图。
* **$urlRouterProvider.otherwise('/parties')**: 如果 URL 不匹配任何路由时触发跳转到 /parties。
* **$locationProvider.html5Mode(true)**: 设置 URL 看起来和常规的一致，详见[这里](https://docs.angularjs.org/guide/$location#hashbang-and-html5-modes)。
* 每个模板都是根据**绝对路径**加载的。这是由 angular-meteor 构建过程完成的，先加载模板到缓存，并根据路径进行命名。
如果模板来自一个包，会用包名作为前缀，例如：'my-app_my-package_client/views/my-template.ng.html'。
你可以通过[源代码](https://github.com/Urigo/angular-meteor/blob/master/plugin/handler.js)，了解更多关于模板构建过程。

注意第二条路由定义时用到的 `:partyId` 参数。$state 服务将路由定义 - `/parties/:partyId` - 作为模板，用来匹配当前URL。所有通过冒号标记定义的变量都会被提取到 $stateParams 对象。

# 控制器

你可能没有注意到，我们移除了 `index.ng.html` 中的 ng-controller 指令，移到了路由定义中。

我们还需要定义 `PartyDetailsCtrl` 控制器。代码如下：
{{> DiffBox tutorialName="angular-meteor" step="5.6"}}

一切就绪。运行 APP，你会发现：

* 点击任意 party 的标题 - 会转到一个新页面，URL 和页面中都出现了 party 的 id。
* 点击后退 - 你又回到了列表页，这是因为 ui-router 集成了浏览器历史功能
* 试着修改URL - 例如 http://localhost:3000/strange-url。浏览器会自动跳转到 parties 列表页。

#### 常见错误

如果你在定义路由时输入了错误的绝对地址(例如，使用了相对地址)，可能会产生如下错误：

WARNING: Tried to load angular more than once.

如果这样的话，检查你的路径，并确保使用了 `.ng.html` 扩展名。

# 总结

路由建立完成，parties 列表也实现了，下一步要实现 party 详情页。

{{/template}}
