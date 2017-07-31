# EditText隐藏以后显示弹出软键盘

### 场景介绍
1. 在使用EditText中页面初始化的时候EditText是隐藏的通过用户触发显示EditText。那么问题来了，在EditText的显示以后需要再点击一次才能显示光标和弹出软键盘。

### 解决方案
1. 先放代码
```
    /*
    lineNum 是EditText控件
    */
    //setFocusable 表示是EditText获取焦点
    lineNum.setFocusable(true);
    //获取输入法管理器
    InputMethodManager imm = (InputMethodManager)getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.toggleSoftInput(0, InputMethodManager.HIDE_NOT_ALWAYS);
```
