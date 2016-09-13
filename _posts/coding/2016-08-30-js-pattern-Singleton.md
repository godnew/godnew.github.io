---
layout: post
title: js设计模式研读（单例模式）
description: JavaScript设计模式中的单例模式研读
category: coding
---

## 单例模式

单例模式的定义是保证一个类仅有一个实例，并提供一个访问他的全局访问点。

要实现个单例模式并不复杂，核心内容无非就是用一个变量来标志当前是否已经为某个类创建过对象，如果是，则在下一次获取该类的实例时，直接返回之前创建的对象。下面来个简单的单例模式的例子：

### 例一：

    var Singleton = function( name ){
        this.name = name;
        this.instance = null;
    };

    Singleton.prototype.getName = function(){
        alert ( this.name );
    };

    Singleton.getInstance = function( name ){
        if ( !this.instance ){
            this.instance = new Singleton( name );
        }
        return this.instance;
    };
    var a = Singleton.getInstance( 'sven1' );
    var b = Singleton.getInstance( 'sven2' );
    alert ( a === b ); // true

在这个例子中可以通过Singleton.getInstance 来获取Singleton 类的唯一对象。这个例子比较简单，但并不实用，因为要通过Singleton.getInstance 来获取对象，而不是new出来对象。下面来改善下这个例子。

### 例二：

    var getSingle = function( fn ){
        var result;
        return function(){
            return result || ( result = fn .apply(this, arguments ) );
        }
    };

使用闭包来完成单例的创建，这个函数中，当result有值后就不会再改变。看下他的使用：

    var bindEvent = getSingle(function(){
    document.getElementById( 'div1' ).onclick = function(){
        alert ( 'click' );
    }
        return true;
    });
    var render = function(){
        console.log( '开始' );
        bindEvent();
    };
    render();
    render();
    render();

可以看到，render 函数和bindEvent 函数都分别执行了3 次，但div 实际上只被绑定了一个事件。

jQuery中的one事件其实就是通过类似上述的方法实现的。

单例模式是一种简单但非常实用的模式，在合适的时候才创建对象，并且只创建唯一的一个。