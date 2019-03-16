##反射-Java高级开发必备

### 一、Class类

####1、认识Class类

> - 我们新建一个Person类。
>
>   ```java
>   package testfile;
>   
>   /**
>    * @Author AlexanderBai
>    * @data 2019/3/16 21:41
>    */
>   class Person {
>       private String name;
>       private int age;
>   
>       public Person() {
>       }
>   
>       public Person(String name,int age) {
>           this.name = name;
>           this.age = age;
>       }
>       
>       public void setAge(int age) {
>           this.age = age;
>       }
>   
>       public void setName(String name) {
>           this.name = name;
>       }
>       public int getAge() {
>           return age;
>       }
>   
>       public String getName() {
>           return name;
>       }
>       
>       @Override
>       public String toString() {
>           return "Person{" +
>                   "name='" + name + '\'' +
>                   ", age=" + age +
>                   '}';
>       }
>   
>   }
>   ```

> - 在java的世界里，万事万物皆对象（除基本数据类型和静态块）。所以应该思考上面的Person是谁的对象。其实java中所有的类都是**Class**这个类的实例化对象。
>
> - 官网：**There is a class named Class.**

#### 2、实例化Class对象

```java
package test;

/**
 * @Author AlexanderBai
 * @data 2019/3/16 22:07
 */
class X{
}
public class Deme {
    public static void main(String[] args) {
        Class<?> c1=null;
        Class<?> c2=null;
        Class<?> c3=null;

        //1、第一种方式
        try {
            c1=Class.forName("test.X");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        //2、第二种方式
        c2=new X().getClass();

        //3、第三种方式
        c3=X.class;
        System.out.println("c1类名称 " + c1.getName());
        System.out.println("c2类名称：" + c2.getName());
        System.out.println("c3类名称：" + c3.getName());
    }
}
```

#### 3、Class实例化其他类的对象

> ①调用无参构造函数实例化对象
>
> ```java
> package test;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/16 21:41
>  */
> class Person {
>     private String name;
>     private int age;
> 
>     public Person() {
>     }
> 
>     public Person(String name,int age) {
>         this.name = name;
>         this.age = age;
>     }
>     
>     public void setAge(int age) {
>         this.age = age;
>     }
>     public void setName(String name) {
>         this.name = name;
>     }
>     public int getAge() {
>         return age;
>     }
>     public String getName() {
>         return name;
>     }
>     
>     @Override
>     public String toString() {
>         return "Person{" +
>                 "name='" + this.name + '\'' +
>                 ", age=" + this.age +
>                 '}';
>     }
> }
> 
> public class TestPerson {
>     public static void main(String[] args) {
>         Class<?> c=null;
>         try {
>             c=Class.forName("test.Person");//传入要实例化类的完整包.类名称
>         } catch (ClassNotFoundException e) {
>             e.printStackTrace();
>         }
>         Person person=null;                 //声明Person对象
>         try {
>             person= (Person) c.newInstance();//实例化对象
>         } catch (InstantiationException e) {
>             e.printStackTrace();
>         } catch (IllegalAccessException e) {
>             e.printStackTrace();
>         }
>         person.setName("AlexanderBai");
>         person.setAge(21);
>         System.out.println(person);
> 
>     }
> }
> ```

> ###使用newInstance()必须有一个无参构造函数(以上没有写构造方法，默认使用无参构造方法)

> ②调用有参构造函数实例化对象
>
> ```java
> package test;
> 
> import java.lang.reflect.Constructor;
> import java.lang.reflect.InvocationTargetException;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/16 21:41
>  */
> class Person {
>     private String name;
>     private int age;
> 
>     public Person() {
>     }
> 
>     public Person(String name, int age) {
>         this.name=name;
>         this.age=age;
>     }
> 
>     public void setAge(int age) {
>         this.age = age;
>     }
> 
>     public void setName(String name) {
>         this.name = name;
>     }
>     public int getAge() {
>         return age;
>     }
> 
>     public String getName() {
>         return name;
>     }
> 
>     @Override
>     public String toString() {
>         return "Person{" +
>                 "name='" + this.name + '\'' +
>                 ", age=" + this.age +
>                 '}';
>     }
> }
> 
> public class TestPerson {
>     public static void main(String[] args) {
>         Class<?> c=null;
>         try {
>             c=Class.forName("test.Person");    //传入要实例化类的完整包.类名称
>         } catch (ClassNotFoundException e) {
>             e.printStackTrace();
>         }
>         Person person=null;                    //声明Person对象
>         Constructor<?> constructors[]=null;    //声明一个表示构造方法的数组
>         constructors=c.getConstructors();
>         try {
>             //向构造方法中传递参数，此方法使用可变参数接收，并实例化对象
>             person= (Person) constructors[1].newInstance("AlexanderBai",21);
>         } catch (InstantiationException e) {
>             e.printStackTrace();
>         } catch (IllegalAccessException e) {
>             e.printStackTrace();
>         } catch (InvocationTargetException e) {
>             e.printStackTrace();
>         }
>         System.out.println(person);
> 
>     }
> }
> ```



### 二、反射的应用-取得类的结构

### 三、Java反射机制的深入应用

### 四、ClassLoader加载类

### 五、动态代理

### 六、工厂设计模式

