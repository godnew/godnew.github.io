---
layout: post
title: jquery源码分析（ready方法）
description: jquery的ready方法是怎么实现的
category: coding
---

## jQuery ready实现

jQuery中的ready方法实现了当页面加载完成后才执行的效果，但他并不是window.onload或者doucment.onload的封装。它的实现原理是：在jquery脚本加载的时候,会设置一个isReady的标记,监听DOMContentLoaded事件(这个不是什么浏览器都有的,不同浏览器,jquery运作方式不一样)。

首先，我们来看jQuery的代码：

    DOMContentLoaded = function()
     {
       //取消事件监听，执行ready方法
      if ( document.addEventListener )
      {
        document.removeEventListener( "DOMContentLoaded", DOMContentLoaded, false );
        jQuery.ready();
      }
       else if ( document.readyState === "complete" )
      {
        document.detachEvent( "onreadystatechange", DOMContentLoaded );
        jQuery.ready();
      }
    };


    jQuery.ready.promise = function( obj ) {
      if ( !readyList ) {
        readyList = jQuery.Deferred();
          //表示页面已经加载完成，直接调用 ready方法
        if ( document.readyState === "complete" ) {
          //将 jQuery.ready压入异步消息队列，设置延迟时间1毫秒（注意，有些浏览器延迟不能小于4毫秒）
          setTimeout( jQuery.ready);
        }
        else if ( document.addEventListener ) //
        {
           //监听DOM加载完成
          document.addEventListener( "DOMContentLoaded", DOMContentLoaded, false );
           //这里是为了确保所有ready执行结束，如果DOMContentLoaded方法执行了，将有一个状态值 isReady被设置为true,因此，
           //ready方法一旦执行，那么将只执行一次，window.addEventListener中的ready 将被 return 中断
          window.addEventListener( "load", jQuery.ready, false );
        } else {
          //低版本的IE浏览器
          document.attachEvent( "onreadystatechange", DOMContentLoaded );
          window.attachEvent( "onload", jQuery.ready );
          var top = false;
          try {
            top = window.frameElement == null && document.documentElement;
          } catch(e) {}
          if ( top && top.doScroll )  //剔除iframe的成分
          {
            (function doScrollCheck() {
              if ( !jQuery.isReady ) {
                try {
                  //根据bug来兼容低版本的IE http://javascript.nwbox.com/IEContentLoaded/
                  top.doScroll("left");
                } catch(e) {
                  //由于低版本的IE 浏览器，onreadystatechange事件不可靠，因此需要根据各个bug来判断页面是否已加载完成
                  return setTimeout( doScrollCheck, 50 );
                }
                jQuery.ready();
              }
            })();
          }
        }
      }
      return readyList.promise( obj );
    };


    ready: function( wait )
     {
      if ( wait === true ? --jQuery.readyWait : jQuery.isReady ) {
        //判断页面是否已完成加载并且是否已经执行ready方法
        return;
      }
      if ( !document.body ) {
        return setTimeout( jQuery.ready );
      }
      jQuery.isReady = true; //指示ready方法已被执行
      if ( wait !== true && --jQuery.readyWait > 0 ) {
        return;
      }
      readyList.resolveWith( document, [ jQuery ] );
      if ( jQuery.fn.trigger ) {
        jQuery( document ).trigger("ready").off("ready");
      }
    },

