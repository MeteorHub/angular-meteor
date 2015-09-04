{{#template name="tutorial.step_04.md"}}
{{> downloadPreviousStep stepName="step_03"}}

现在，我们已经实现了从服务端到客户端的数据绑定，接下来，要与数据交互，进行数据更新。

在本节中，要实现通过界面插入新 party 以及删除功能。

首先，创建一个表单，点击按钮就可以添加一个新 party。我们把表单放在 "PartiesListCtrl" 控制器对应的 div 中，party 列表之上。

{{> DiffBox tutorialName="angular-meteor" step="4.1"}}

现在，需要实现表单的功能。

## ng-model

第一件事，把输入框的值绑定到一个新的 party 变量。

这要用到简单却强大的 AngularJS 指令 [ng-model](https://docs.angularjs.org/api/ng/directive/ngModel)。

以下面的方式把 `ng-model` 添加到表单：

{{> DiffBox tutorialName="angular-meteor" step="4.2"}}

现在，每当用户在这些输入框进行输入时，newParty scope 变量的值都会自动更新。对应的，如果 `$scope.newParty` 在HTML以外的地方发生变化，输入框的值也会相应更新。

## ng-click

通过 Angular 的 [ng-click](https://docs.angularjs.org/api/ng/directive/ngClick) 指令给按钮绑定 click 事件。

{{> DiffBox tutorialName="angular-meteor" step="4.3"}}

`ng-click` 绑定 click 事件到一个表达式。我们获取 parties scope 数组 (当在HTML中获取 scope 变量时，不用在前面加上 $scope. )，并把 newParty 变量推进去。

打开另一个浏览器窗口，点击按钮，发现所有的客户端都新增了 party。太简单了！

接下来，增加删除功能。

给每一个 party 添加一个 X 按钮：

{{> DiffBox tutorialName="angular-meteor" step="4.4"}}

这次，我们绑定 ng-click 到一个 scope 函数，并把当前的 party 作为参数。

回到控制器中，增加这个函数。

在 `app.js` 的 PartiesListCtrl 中，添加函数：

{{> DiffBox tutorialName="angular-meteor" step="4.5"}}

现在，试着删除几个 parties ，并观察其它客户端也进行了同步删除。

# AngularMeteorCollection functions

$meteor.collections 的返回值是一个 [AngularMeteorCollection](/api/AngularMeteorCollection) 类型的集合。

它不仅负责更新集合，还包含一些 helper 函数，用来保存和删除对象。

我们试着用这些 helper 函数替换当前的实现。

首先，在按钮的点击事件中用 `save` 代替 `push`：

{{> DiffBox tutorialName="angular-meteor" step="4.6"}}

这里的变化不大，除了一点点性能上的提升，接下来，修改删除函数：

{{> DiffBox tutorialName="angular-meteor" step="4.7"}}

更简洁，性能也更好！

添加一个按钮，用于删除所有 parties:

{{> DiffBox tutorialName="angular-meteor" step="4.8"}}

别忘了添加删除函数到 scope:

{{> DiffBox tutorialName="angular-meteor" step="4.9"}}

非常简洁。

有关 AngularMeteorCollection 和它的 helper 函数详见 [API reference](/api/AngularMeteorCollection)。 

# 总结

你已经看到了，使用 Angular 强大的指令操作数据以及用 Meteor 强大的 Mongo.collection API 同步数据是多么的简单。

{{/template}}
