---
layout: post
title: UI-Router
description: 关于angularjs中ui-router的使用
category: frame
---

## UI-Router

UI-Router是Angular-UI提供的客户端路由框架，它解决了原生的ng-route的很多不足：视图不能嵌套。这意味着$scope会发生不必要的重新载入。这也是我们在Onboard中引入ui-route的原因。

同一URL下不支持多个视图。这一需求也是常见的：我们希望导航栏用一个视图（和相应的控制器）、内容部分用另一个视图（和相应的控制器）。

UI-Router提出了$state的概念。一个$state是一个当前导航和UI的状态，每个$state需要绑定一个URL Pattern。 在控制器和模板中，通过改变$state来进行URL的跳转和路由。这是一个简单的例子：

    // in app-states.js
    $stateProvider
        .state('contacts', {
            url: '/contacts',
            template: 'contacts.html',
            controller: 'ContactCtrl'
        })
        .state('contacts.detail', {
            url: /contacts/:contactId,
            templateUrl: 'contacts.detail.html',
            controller: function ($stateParams) {
                // If we got here from a url of /contacts/42
                $stateParams.contactId === 42;
            }
        });

当访问/contacts时，contacts $state被激活，载入对应的控制器和视图。在ui-router时，通常使用$state来完成页面跳转， 而不是直接操作URL。例如，在脚本使用$state.go：

$state.go('contacts');  // 指定state名，相当于跳转到 /contacts

$state.go('contacts.detail', {contactId: 42});  // 相当于跳转到 /contacts/42

然后在模板中使用ui-self

## 嵌套视图

不同于Angular原生的ng-route，ui-router的视图可以嵌套，视图嵌套通常对应着$state的嵌套。 contacts.detail是contacts的子$state，contacts.detail.html也将作为contacts.html的子页面。

上述ui-view的用法和ng-view看起来很相似，但不同的是ui-view可以配合$state进行任意层级的嵌套， 即contacts.detail.html中仍然可以包含一个ui-view，它的$state可能是contacts.detail.hobbies。

## 命名视图

在ui-router中，一个$state下可以有多个视图，它们有各自的模板和控制器。这一点也是ng-route所没有的， 给了前端路由极大的灵活性。来看例子：

这一个模板包含了三个命名的ui-view，可以给它们分别设置模板和控制器：

    $stateProvider
      .state('report',{
        views: {
          'filters': {
            templateUrl: 'report-filters.html',
            controller: function($scope){ ... controller stuff just for filters view ... }
          },
          'tabledata': {
            templateUrl: 'report-table.html',
            controller: function($scope){ ... controller stuff just for tabledata view ... }
          },
          'graph': {
            templateUrl: 'report-graph.html',
            controller: function($scope){ ... controller stuff just for graph view ... }
          }
        }
      })
