# final关键字
为什么使用final：
继承中父类的方法可能被子类重写，父类的功能可能会被子类覆盖掉，例如
```
classn Fu{
    public void show(){
        System.out.println("父类的方法，不想被你重写");
    }
}
class Zi extends Fu{
    public void show(){
        System.out.println("我就重写，haha");
    }
}
```
所以可能通过加final关键字，不让子类重写，例如：
```
class Fu{
    public final void show(){
        System.out.println("这下你重写不了吧，haha");
    }
}
```
***final仅可以修饰方法，也可以修饰类和变量***
#####final的特点
* final修饰类，该类不能被继承。
* final修饰方法，该方法不能被重写。
* final修饰变量，该变量不能被重新赋值，这个就是自定义常量。如果重新赋值会报错。