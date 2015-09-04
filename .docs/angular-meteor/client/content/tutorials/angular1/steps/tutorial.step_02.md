{{#template name="tutorial.step_02.md"}}
{{> downloadPreviousStep stepName="step_01"}}

是时候用 AngularJS 将页面变为动态的了。

Step 2 将主要关注客户端 Angular。Step 3 会展示 Meteor 的强大之处。

# 视图和模板

在 Angular 中，视图是模型通过模板的映射。也就是说，无论何时模型发生了变化，Angular 刷新合适的绑定点来更新视图。

来把我们的模板改为动态的：

{{> DiffBox tutorialName="angular-meteor" step="2.1"}}

用 [ngRepeat](https://docs.angularjs.org/api/ng/directive/ngRepeat) 指令和两个 Angular 表达式替换硬编码的 party 列表：

* `li` 标签的属性 `ng-repeat="party in parties"` 是一个 Angular repeater 指令。repeater 通知 Angular 给列表里的每一个 party 创建一个 `li` 标签。
* 双花括号包裹的表达式 ( `{{dstache}}party.name}}` 和 `{{dstache}}party.description}}` ) 会被表达式的值替换。

我们还用到了一个新的指令 `ng-controller`，它把 `PartiesListCtrl` 控制器附加到 `div` 标签上。
这样，双花括号包裹的表达式就可以引用 `PartiesListCtrl` 控制器中创建的模型，


# 模型和控制器

现在，我们要创建控制器和模型。新建 `PartiesListCtrl` 控制器，并添加数据。

{{> DiffBox tutorialName="angular-meteor" step="2.2"}}

我们声明了一个叫做 `PartiesListCtrl` 的控制器，并把它注册到了 Angular 模块 - `socially`。

现在，数据模型在 `PartiesListCtrl` 控制器中进行了实例化。
尽管现在控制器还没做啥工作，但是它很关键。通过提供数据模型上下文，控制器建立了模型和视图之间的数据绑定。我们用下面的方式建立了展示、数据和逻辑组件之间的联系：

* `body` 标签上的 ngController 指令，引用着控制器的名字 `PartiesListCtrl` (定义在javascript文件 `app.js`)
* `PartiesListCtrl` 控制器把 party 数据关联到被注入到控制器的 `$scope` 上。
所有在 `<div ng-controller="PartiesListCtrl">` 标签里的绑定都可以获取控制器的 scope。

# ng-annotate 和 .ng.js

如你所见，在声明控制器的时候，使用了字符串形式的[依赖注解](https://docs.angularjs.org/guide/di#dependency-annotation)，避免代码压缩时出现问题：

    angular.module('socially').controller('PartiesListCtrl', ['$scope',
      function($scope){
        // ...
    }]);

[ng-annotate](https://github.com/olov/ng-annotate)是个很流行的 Angular 工具，它可以保证我们正常编写的代码不会在压缩时出问题。

angular-meteor 自动使用了该工具。你只需将 `.js` 文件改为 `.ng.js` 后缀。

然后，你就可以用下面的方式写依赖注入：

    angular.module('socially').controller('PartiesListCtrl',
      function($scope){
        // ...
    });

# 总结

现在，我们有了一个动态 APP，包含独立的模型，视图和控制器组件。

但是，这全是在客户端。在一个真实的 APP 中，我们需要在服务端保存数据并同步给客户端。

接下来，[step 3](/tutorial/step_03)将展示如何结合 Meteor 的强大之处。

{{/template}}
