1. 定义一个类

   > 定义一个类使用关键字 `class`

   ```kotlin
   class Person{}
   ```

   1. 他有一个默认唯一的构造函数

   ```

   ```
   2. 类的继承
		1. 只能继承使用`open`和`abstract` 修饰的类 
		```kotlin
		class Son(age : Int): Father{
		
		}
		```
   ​3. 函数的创建
		1. 使用`fun`关键字修饰
		```kotlin
		fun addAge(num:Int):Int{
		
		}
		```
		2. 没有返回值使用的关键字是`Unit`等同于Java中`void`,又返回值直接在方法会加冒号写返回类型
		
	4. 函数参数
		1. 在kotlin中函数参数与java不同, 需要先写<参数名:参数类型> .这里的参数定义与java也是不同的 <var 参数名称:参数类型>
		2. 函数参数在定义的时候可一设置默认值
		```kotlin
		fun toast(message:String,length:Int = Toast.LENGTH_SHORT){
			Toast.make(this,message,length).show;
		}
		```
		

