
# 开发中发现的自己不了解的知识点

## <font color=#0099ff>LikedList</font>和<font color = #0099ff>ArrayList</font>的区别

ArrayList是基于动态数组的数据结构   
LinkedList是基于链表的数据结构      

1. 如果是get和set方法ArrayList要优于LikedList，LinkedList需要移动指针
2. 如果是add和remove方法LinkedList要优于ArraytList，ArrayList需要移动数据。

> 如果是在ArraList末尾添加数据，ArrayList的速度要优于LinkedList   

> 使用注意：如果需要频繁的插入和删除最好使用LinkedList 如果是随机访问比较多则最好使用ArrayList
