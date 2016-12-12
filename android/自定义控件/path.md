## Path
### 将两个Path合并成一个Path

> addPath(Path src)   
> addPath(Path src,float x,float y)  
> addPath(Path src,Matrix matrix)


方法一、直接让两个Path重合   
方法二、给一个基点两个Path以这个基点重合
方法三、通过Matrix对Path进行变换操作

##　添加一个圆弧到Path中　　
> addArc(RectF oval,float startAngle,float sweepAngle)   
> addTo(RectF oval,float startAngle,float sweepAngle)  
> addTo(RectF oval,float startAngle,float sweepAngle,boolean forcaMoveTo)   


| 参数 | 作用 |
|---------|
|oval|圆弧的大小，范围|
|startAngle|圆弧开始的角度|
|sweepAngle|圆弧旋转的角度，-360=<sweepAngle<360 |
|forcaMoveTo|是否强制使用MoveTo|

addArc和addTo的区别两者都能直接添加一个圆弧到path中，但是addTo圆弧的起点和上次最后一个坐标点不相同，两个点会连接
