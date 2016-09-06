---
layout: post
title: 一些jquery小技巧
description: 整理的一些使用的jquery小技巧
category: coding
---

## 一些jQuery小技巧

### 1.禁用页面的右键菜单

    $(function(){
        $(document).bind("contextmene",function(e){
            return false;
        });
    });

### 2.新窗口打开页面

    $(function(){
        //href="http://"的超链接将会在新窗口打开链接
       $("a[href^='http://']").attr("target","_blank");

       //rel="external"的超链接将会在新窗口打开链接
       $("a[rel$='external']").click(function(){
            this.target="_blank";
       });
    });

### 3.判断浏览器类型

    $(function(){
        //火狐
        if($.browser.mozilla && $.brower.version>='1.8'){
            //do something
        }
        //safari
        if($.browser.safari){
            //do something
        }
        //opera
        if($.browser.opera){
            //do something
        }
        //ie6及以下版本
        if($.browser.msie && $.browser.version<=6){
            //do something
        }
        //ie6以上
        if($.browser.msie && $.browser.version>6){
            //do something
        }
    });

### 4.输入框文字获取和失去焦点

    $(function(){
        $("input.text1").val("输入内容");
        textFill($("input.text1"));
    });
    function textFill(input){
        var ovalue=input.val();
        input.focus(function(){
            if($.trim(input.val())==ovalue){
                input.val("");
            }
        }).blur(function(){
            if($.trim(input.val())==""){
                input.value=ovalue;
            }
        });
    }

### 5.返回头部滑动动画

    jQuery.fn.scrollTo=function(speed){
        var targetOffset=$(this).offset().top;
        $("body").stop().animate({scrollTop:targetOffset},speed);
        return this;
    };
    $("#goheader").click(function(){
        $("body").scrollTo(500);
        return false;
    });

### 6.获取鼠标位置

    $(function(){
        $("document").mousemove(function(e){
            $("#xy").html("x:"+e.pageX+"y:"+e.pageY);
        });
    });

### 7.设置div在屏幕中央

    $(function(){
        jQuery.fn.center=function(){
            this.css("position","absolute");
            this.css("top",($(window).height()-this.height())/2+$(window).scrollTop()+'px');
            this.css("left",($(window).width()-this.width())/2+$(window).scrollLeft()+'px');
            return this;
        }
        $("#xy").center();
    });

### 8.关闭所有动画效果

    $(function(){
        jQuery.fx.off=true;
    });

### 9.回车提交表单

    $(function(){
        $("input").keyup(function(e){
            if(e.which=="13"){
                $("form").submit();
            }
        });
    });

### 10.使用siblings()来选择同辈元素

    //不这样做
    $("ul li").click(function(){
        $("ul li").removeClass("active");
        $(this).addClass("active");
    });
    //应该这样做
    $("ul li").click(function(){
        $(this).addClass("active").siblings().removeClass("active");
    });

