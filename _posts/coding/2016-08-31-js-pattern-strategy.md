---
layout: post
title: js设计模式研读（策略模式）
description: JavaScript设计模式中的策略模式研读
category: coding
---

## 策略模式

策略模式的定义是：定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。

在程序设计中，我们也常常遇到类似的情况，要实现某一个功能有多种方案可以选择。例如以下的一个例子：计算工资。

我们可以编写一个名为calculateBonus 的函数来计算每个人的奖金数额。很显然，calculateBonus 函数要正确工作，就需要接收两个参数：员工的工资数额和他的绩效考核等级。

    var calculateBonus = function( performanceLevel, salary ){
        if ( performanceLevel === 'S' ){
            return salary * 4;
        }
        if ( performanceLevel === 'A' ){
            return salary * 3;
        }
        if ( performanceLevel === 'B' ){
            return salary * 2;
        }
    };
    calculateBonus( 'B', 20000 ); // 输出：40000
    calculateBonus( 'S', 6000 ); // 输出：24000

上述的代码十分的简单，但是缺点非常的多。该函数比较庞大，分支特别多，缺乏弹性，复用性特别差。所以该代码需要改善。

经过思考，我们想到了更好的办法——使用策略模式来重构代码。策略模式指的是定义一系列的算法，把它们一个个封装起来。将不变的部分和变化的部分隔开是每个设计模式的主题，策
略模式也不例外，策略模式的目的就是将算法的使用与算法的实现分离开来。

在这个例子里，算法的使用方式是不变的，都是根据某个算法取得计算后的奖金数额。而算法的实现是各异和变化的，每种绩效对应着不同的计算规则。

一个基于策略模式的程序至少由两部分组成。第一个部分是一组策略类，策略类封装了具体的算法，并负责具体的计算过程。 第二个部分是环境类Context，Context 接受客户的请求，随后
把请求委托给某一个策略类。要做到这点，说明Context 中要维持对某个策略对象的引用。

    var strategies = {
        "S": function( salary ){
            return salary * 4;
        },
        "A": function( salary ){
            return salary * 3;
        },
        "B": function( salary ){
            return salary * 2;
        }
    };
    var calculateBonus = function( level, salary ){
        return strategies[ level ]( salary );
    };
    console.log( calculateBonus( 'S', 20000 ) ); // 输出：80000
    console.log( calculateBonus( 'A', 10000 ) ); // 输出：30000

策略模式的一些优点：策略模式利用组合、委托和多态等技术和思想，可以有效地避免多重条件选择语句。

策略模式提供了对开放—封闭原则的完美支持，将算法封装在独立的strategy 中，使得它们易于切换，易于理解，易于扩展。

策略模式中的算法也可以复用在系统的其他地方，从而避免许多重复的复制粘贴工作。在策略模式中利用组合和委托来让Context 拥有执行算法的能力，这也是继承的一种更轻
便的替代方案。

当然，策略模式也有一些缺点，但这些缺点并不严重。

首先，使用策略模式会在程序中增加许多策略类或者策略对象，但实际上这比把它们负责的逻辑堆砌在Context 中要好。

其次，要使用策略模式，必须了解所有的strategy，必须了解各个strategy 之间的不同点，这样才能选择一个合适的strategy。比如，我们要选择一种合适的旅游出行路线，必须先了解选
择飞机、火车、自行车等方案的细节。此时strategy 要向客户暴露它的所有实现，这是违反最少知识原则的。

