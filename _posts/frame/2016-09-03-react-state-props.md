---
layout: post
title: reactjs中state和props区别
description: 分别在什么地方使用state和props
category: frame
---

需要理解的是，props是一个父组件传递给子组件的数据流，这个数据流可以一直传递到子孙组件。而state代表的是一个组件内部自身的状态（可以是父组件、子孙组件）。

改变一个组件自身状态，从语义上来说，就是这个组件内部已经发生变化，有可能需要对此组件以及组件所包含的子孙组件进行重渲染。

而props是父组件传递的参数，可以被用于显示内容，或者用于此组件自身状态的设置（部分props可以用来设置组件的state），不仅仅是组件内部state改变才会导致重渲染，父组件传递的props发生变化，也会执行。

既然两者的变化都有可能导致组件重渲染，所以只有理解pros与state的意义，才能很好地决定到底什么时候用props或state。

官方指导有说，props放初始化数据，一直不变的，state就是放要变的。

State 应该包括那些可能被组件的事件处理器改变并触发用户界面更新的数据,因为组件本身不能修改自己的 props。
