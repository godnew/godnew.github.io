---
layout: post
title: 一些jquery小技巧(2)
description: 整理的一些使用的jquery小技巧第二部分
category: coding
---

## jQuery小技巧（2）

### 1.为任何与选择器相匹配的元素绑定事件

    //为table里面动态加载的td元素绑定click事件
    //1.4.2版本之前
    $("table").each(function(){
        $("td",this).live("live",function(){
            //do something
        });
    });
    //1.4.2版本以后
    $("table").delegate("td","click",function(){
        //do something
    });
    //1.7.7版本以后
    $("table").on("click","td",function(){
        //do something
    });

### 2.限制Text-Area域中的字符的个数

    jQuery.fn.maxLength=function(){
        this.each(function(){
            var type=this.tagName.toLowerCase();
            var inputType=this.type?this.type.toLowerCase():null;
            if(type=="input"&&input=="text"||inputType="password"){
                this.maxLength=max;
            }else if(type=="textarea"){
                this.onkeypress=function(e){
                    var ob=e||event;
                    var keyCode=ob.keyCode;
                    var hasSelection=document.selection?document.selection.createRange().text.length>0:this.selectionStart!=this.selectionEnd;
                    return !(this.value.length>=max&&(keyCode>50||keyCode==32||keyCode==0||keyCode==13)&&!ob.ctrlKey&&!hasSelection);
                };
                this.onkeyup=function(){
                    if(this.value.length>max){
                        this.value=this.value.substring(0,max);
                    }
                };
            }
        });
    }
    //使用
    $("textarea").maxLength(10);

### 3.扩展String对象的方法

    $.extend(String.prototype,{
        isPositiveInteger:function(){
            return (new RegExp(/^[1-9]\d*$/).test(this));
        },
        isInteger:function(){
            return (new RegExp(/^\d+$/).test(this));
        },
        isNumber:function(){
            return (new RegExp(/^-?(?:\d+|\d{1,3}(?:,\d{3})+)(?:\.\d+)?$/).test(this));
        },
        trim:function(){
            return this.replace(/(^\s*)|(\s*$)|\r|\n/g,"");
        },
        trans:function(){
            return this.replace(/&lt;/g,"<").replace(/&gt;/g,'>').replace(/&quot;/g,"=");
        },
        replaceAll:function(){
            return this.replace(new RegExp(os,'gm'),ns);
        },
        skipChar:function(){
            if(!this||this.length==0){return '';}
            if(this.charAt(0)==ch){return this.substring(1).skipChar(ch);}
            return this;
        },
        isValidPwd:function(){
            return (new RegExp(/^([_]|[[a-zA-Z0-9]){9,32}$/).test(this));
        },
        isValidMail:function(){
            return (new RegExp(/^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/).test(this.trim()));
        },
        isSpaces:function(){
            for(var i=0;i<this.length;;i+=1){
                var ch=this.charAt(i);
                if(ch!=' '&&ch!="\n"&&ch!="\t"&&ch!="\r"){return false;}
                return true;
            }
        },
        isPhone:function(){
            return (new RegExp(/^1(3|4|5|7|8)\d{9}$/).test(this));
        },
        isUrl:function(){
            return (new RegExp(/(h|H)(r|R)(e|E)(f|F) *= *('|")?(\w|\\|\/|\.)+('|"| *|>)?/).test(this));
        }
    });
    //使用
    $("input").val().isInteger();