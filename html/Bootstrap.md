# Bootstrap

`container-fluid`class名称的`div`下

我们只要给图片添加 `img-responsive` 的class属性,图片的宽度就能自动适配你手机屏幕的宽度啦。

标签添加`text-center`的class属性,标题文字就可以居中了。

你可以使用`btn-block`这个class讲按钮升级为块级元素

`btn-primary`类是该应用的主要颜色类，这个类的颜色对于那些你希望高亮提示用户的操作上非常有用。

 ```html
<div class="row">
        <div class="col-xs-4">
            <button class="btn btn-block btn-primary">Like</button>
        </div>
    	<!-- 总共存在8列 各占4列 -->
        <div class="col-xs-4">
            <button class="btn btn-block btn-info">Info</button>
        </div>
        <div class="col-xs-4">
            <button class="btn btn-block btn-danger">Delete</button>
        </div>
    </div>
 ```

input 输入框的样式 `class="form-control"`