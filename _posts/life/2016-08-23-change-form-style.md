---
layout: post
title: 改变form表单自带样式
category: life
description: 改变form表单自带样式
---

form表单自带的样式实在太丑，实际开发中并不能用它自带的样式，一般ui设计稿给的样式是十分的炫酷的，那么怎么去改？

其实改起来挺简单的，只要十几行代码就能将整个页面上的所有checkbox或radio的样式都改了哦。（前提是你要有修改样式的图片！！！）

    window.onload=function(){
        function getid(id) { return document.getElementById(id); }
        function gettag(tag, obj) { if (obj) { return obj.getElementsByTagName(tag) } else { return document.getElementsByTagName(tag) } }
        (function radio_style() {//3设置样式
            if (gettag("input")) {
                var r = gettag("input");
                function select_element(obj, type) {//1设置背景图片
                    obj.parentNode.style.background = "url(radio选中的样式图片地址) no-repeat left center";//指定背景图相对父元素的位置，一般都是开头
                    if (obj.type == "checkbox") { obj.parentNode.style.background = "url(checkbox选中后的样式图片地址) no-repeat right center"; }
                    if (type) {
                        obj.parentNode.style.background = "url(radio没选中的样式图片地址) no-repeat left center";
                        if (obj.type == "checkbox") { obj.parentNode.style.background = "url(checkbox没选中的样式图片地址) no-repeat left center"; }
                    }
                }
                //遍历所有关于checkbox和radio的节点
                for (var i = 0; i < r.length; i++) {
                    if (r[i].type == "radio" || r[i].type == "checkbox") {
                        r[i].style.opacity = 0; r[i].style.filter = "alpha(opacity=0)";//隐藏真实的checkbox和radio
                        r[i].onclick = function() { select_element(this); unfocus(); }
                        if (r[i].checked == true) { select_element(r[i]); } else { select_element(r[i], 1); }
                    }
                }
                function unfocus() {//2处理未选中
                    for (var i = 0; i < r.length; i++) {
                        if (r[i].type == "radio" || r[i].type == "checkbox") { if (r[i].checked == false) { select_element(r[i], 1) } }
                    }
                }
            }
        })();
    }
