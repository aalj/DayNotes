# View的时间分发机制

## 点击事件的换地规则

1. `dispatchOnTouchEvent(MotionEvent ev)`

  如果点击事件能够传递给当前的View那么此方法一定会被调用,表示是否消耗当前事件  

2. `onInterceptTouchEvent(MotionEvent ev)`  

  判断是否拦截某个事件  true 表示拦截时间

3. `onTouchEvent(Motion ev)`  

  具体的消费事件  

  一下为伪代码
```
public boolean diapatchTouchEvent(MotionEven ev){
    boolean consume = false;
    if (onInterceptTouchEvent(ev)) {
        consume = onTouchEvent(ev);
    }else{
        consume = child.dispatchTouchEvent(ev);
    }
    return consume;
}
```
关于`OnTouchListener`

只要设置了`OnTouchListener` `onTouch`方法就会被调用,但是如果返回`false`当前`View`的`onTouchEvent`就被调用,返回的是`true` `onTouchEvent`将不会被调用. onTouchListener 优先级比onTouchEvent高. onClickListener 优先级最低 因为onClick 方法是在onTouchEvent中调用的

事件的传递顺序  Activity—>Windows—>View