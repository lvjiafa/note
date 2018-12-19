# this关键字
this代表的是当前类的一个对象    
什么时候用this呢
1. 局部变量与成员变量重名，用this去访问成员变量。
```
class Student{
    privte String name;
    public void setName(String name){
        this.name=name;
    }
}
```
2. 在构造方法中作为方法名初始化对象。
```
class Student{
    privte String name;
    public void Student(String name){
        this.name=name;
    }
    public void Student(){
        this("XiaoMing");
    }
}
```
上面代码实质是在一个构造方法里调用了另外一个构造方法，需要注意的是：
* 调用语句必须在第一句。
* 除了构造方法，其他方法不能调用构造方法。
* 一个构造方法只能调用一个构造方法。









