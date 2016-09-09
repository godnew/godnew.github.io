---
layout: post
title: 上传图片添加预览功能
category: life
description: 图片上传时能够在页面上看到你上传的那张图片
---

## uploadPreview

最近做到一个webapp项目，里面要实现的一个功能是这样的
![](images/life/uploadPreview1.png)![](images/life/uploadPreview2.png)
当照片点击上传后，边上能显示出你那个上传的照片。这个功能在webapp中很常见，所以做了个小插件，方便以后使用。

### 以下是功能的实现：

    var uploadPreview = function(setting) {
        /*
        *work:this(当前对象)
        */
        var _self = this;
        /*
        *work:判断为null或者空值
        */
        _self.IsNull = function(value) {
            if (typeof (value) == "function") { return false; }
            if (value == undefined || value == null || value == "" || value.length == 0) {
                return true;
            }
            return false;
        }
        /*
        *work:默认配置
        */
        _self.DefautlSetting = {
            UpBtn: "",//选择文件控件ID
            DivShow: "",//DIV控件ID
            ImgShow: "",//图片控件ID
            Width: 100,//预览宽度
            Height: 100,//预览高度
            ImgType: ["gif", "jpeg", "jpg", "bmp", "png"],//支持文件类型
            ErrMsg: "选择文件错误,图片类型必须是(gif,jpeg,jpg,bmp,png)中的一种",
            callback: function() { }//回调函数，当完成后调用该函数
        };
        /*
        *work:读取配置
        */
        _self.Setting = {
            UpBtn: _self.IsNull(setting.UpBtn) ? _self.DefautlSetting.UpBtn : setting.UpBtn,
            DivShow: _self.IsNull(setting.DivShow) ? _self.DefautlSetting.DivShow : setting.DivShow,
            ImgShow: _self.IsNull(setting.ImgShow) ? _self.DefautlSetting.ImgShow : setting.ImgShow,
            Width: _self.IsNull(setting.Width) ? _self.DefautlSetting.Width : setting.Width,
            Height: _self.IsNull(setting.Height) ? _self.DefautlSetting.Height : setting.Height,
            ImgType: _self.IsNull(setting.ImgType) ? _self.DefautlSetting.ImgType : setting.ImgType,
            ErrMsg: _self.IsNull(setting.ErrMsg) ? _self.DefautlSetting.ErrMsg : setting.ErrMsg,
            callback: _self.IsNull(setting.callback) ? _self.DefautlSetting.callback : setting.callback
        };
        /*
        *work:获取文本控件URL
        */
        _self.getObjectURL = function(file) {
            var url = null;
            if (window.createObjectURL != undefined) {
                url = window.createObjectURL(file);
            } else if (window.URL != undefined) {
                url = window.URL.createObjectURL(file);
            } else if (window.webkitURL != undefined) {
                url = window.webkitURL.createObjectURL(file);
            }
            return url;
        }
        /*
        *work:绑定事件
        */
        _self.Bind = function() {
            document.getElementById(_self.Setting.UpBtn).onchange = function() {
    			console.log(this.files);
                if (this.value) {
                    if (!RegExp("\.(" + _self.Setting.ImgType.join("|") + ")$", "i").test(this.value.toLowerCase())) {
                        alert(_self.Setting.ErrMsg);
                        this.value = "";
                        return false;
                    }
                    if (navigator.userAgent.indexOf("MSIE") > -1) {
                        try {
                            document.getElementById(_self.Setting.ImgShow).src = _self.getObjectURL(this.files[0]);//显示图片
                        } catch (e) {
                            var div = document.getElementById(_self.Setting.DivShow);
                            this.select();
                            top.parent.document.body.focus();
                            var src = document.selection.createRange().text;
                            document.selection.empty();
                            document.getElementById(_self.Setting.ImgShow).style.display = "none";
                            div.style.filter = "progid:DXImageTransform.Microsoft.AlphaImageLoader(sizingMethod=scale)";
                            div.style.width = _self.Setting.Width + "px";
                            div.style.height = _self.Setting.Height + "px";
                            div.filters.item("DXImageTransform.Microsoft.AlphaImageLoader").src = src;
                        }
                    } else {
                        document.getElementById(_self.Setting.ImgShow).src = _self.getObjectURL(this.files[0]);
                    }
                    _self.Setting.callback();//执行回调函数
                }
            }
        }
        /*
        *work:执行绑定事件
        */
        _self.Bind();
    }

上述代码看似很长，但很多是为了对错误使用的处理以及兼容性的处理。

### 使用方法：

    //界面构造(IMG标签外必须拥有DIV 而且必须给予DIV控件ID)
    <div id="imgdiv"><img id="imgShow" width="120" height="120" /></div>
    <input type="file" id="up_img" />
    //调用代码:
    new uploadPreview({ UpBtn: "up_img", DivShow: "imgdiv", ImgShow: "imgShow" });