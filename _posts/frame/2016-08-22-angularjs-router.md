---
layout: post
title: Angularjs使用路由后不能使用锚链接
description: 使用router后锚链接失效后的解决方法
category: frame
---

最近的一个项目中使用angularjs做个了展示型的网站，因为其路由的过程中的动画特效做起来很简单，所以这个项目就拿angualr来练练手，但在开发中遇到了个问题，就是不能使用锚链接了，这就尴尬了，展示型网站都很长，页面的导航条需要导航到页面的各个位置，然后页面上要有返回顶部的锚链接。所以这个问题就比较头疼了。网上查阅了大量资料，发现jsGen中封装的anchorScroll正是这个问题的解决办法。研究后对其改写了下。
以下是我的实现过程：

>server.js

    App.factory('anchorScroll', function () {
        function toView(element, top, height) {
            var winHeight = $(window).height();

            element = $(element);
            height = height > 0 ? height : winHeight / 10;
            $('html, body').animate({
                scrollTop: top ? (element.offset().top - height) : (element.offset().top + element.outerHeight() + height - winHeight)
            }, {
                duration: 200,
                easing: 'linear',
                complete: function () {
                    if (!inView(element)) {
                        element[0].scrollIntoView( !! top);
                    }
                }
            });
        }

        function inView(element) {
            element = $(element);

            var win = $(window),
                winHeight = win.height(),
                eleTop = element.offset().top,
                eleHeight = element.outerHeight(),
                viewTop = win.scrollTop(),
                viewBottom = viewTop + winHeight;

            function isInView(middle) {
                return middle > viewTop && middle < viewBottom;
            }

            if (isInView(eleTop + (eleHeight > winHeight ? winHeight : eleHeight) / 2)) {
                return true;
            } else if (eleHeight > winHeight) {
                return isInView(eleTop + eleHeight - winHeight / 2);
            } else {
                return false;
            }
        }

        return {
            toView: toView,
            inView: inView
        };
    });

>使用

    controller：
        $scope.demo = function(){
            anchorScroll.toView('#test', true);
        };
    view:
        <a href="" ng-click="demo();">test</a>
        <div id="test"></div>

在使用路由中还遇到一个问题
![](images/frame/ng-router.png)

以下是我的解决方案：

    App.run(['$rootScope', '$location', function($rootScope, $location) {
        $rootScope.$on('$routeChangeSuccess', function(evt, next, previous) {
            document.documentElement.scrollTop = document.body.scrollTop =0;
        });
    }]);

思路是在路由成功完成时调用其回调函数，设置body的scrcollTop为0。
