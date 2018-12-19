# API常见类
**Object类**

* Class Object is the root of the class hierarchy. Every class has Object as a superclass. All objects, including arrays, implement the methods of this class.  
Object类是类层次结构的根，每个类都把Object作为超类，所有对象包括数组都实现该了的方法  
* 构造方法  
public Object()  
* 常用方法  
    * public int hashCode()  
        * Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by HashMap.  
    返回对象的哈希码值，这个方法是为了对哈希表有益  
        * Returns:    
a hash code value for this object.
    * public final Class<?> getClass()
        * Returns the runtime class of this Object. The returned Class object is the object that is locked by static synchronized methods of the represented class. 
        * Returns:    
The Class object that represents the runtime class of this object.
```
class Student {}
class Test{
    public static void main(String[] args) {
        //创建Student的对象
        Student s1 = new Student();
        //调用hashCode方法，返回该对象的哈希值
        System.out.println(s1.hashCode());
        //输出
        //21685669
    }
}
```





