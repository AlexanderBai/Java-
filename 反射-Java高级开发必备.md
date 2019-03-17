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

#### 1、取得所实现的全部接口

​	java允许多实现，所以获取的接口以数组方式返回

```java
public Class<?>[] getInterfaces()
```

#### 2、取得父类

```java
public static Class<? super T> getSuperclass()
```

#### 3、取得全部构造方法

```java
public Constructor<?>[] getConstructors()
```

#### 4、取得全部方法

```java
public Method[] getMethods()
```

####5、取得全部属性

```java
public Field[] getFields()
```

####6、getFields()和getDeclaredFields()的区别

- 这两个方法很像，但所获取的属性并不一样，getFields()获取的是所有**可以获取**的属性（包括接口、父类和本类）getDeclaredFields()获取的属性不包括父类的属性

- 且需要注意的是以上方法不能获取私有属性，私有属性需要设置访问权限才可访问
- 同样类似的还有**getConstructors()**和**getDeclaredConstructors()**、**getMethods()**和**getDeclaredMethods()**，这两者分别表示获取某个类的方法、构造函数。

>```java
>/**
>	*getDeclaredFields()
>	*Returns an array of {@code Field} objects reflecting all the fields
>    * declared by the class or interface represented by this
>    * {@code Class} object. This includes public, protected, default
>    * (package) access, and private fields, but excludes(移除) inherited fields.
>**/
>```

>```java
>/**
>*getFields()说明
>- Returns a {@code Field} object that reflects the specified public member
>- field of the class or interface represented by this {@code Class}
>- object. The {@code name} parameter is a {@code String} specifying the
>- simple name of the desired field.
>**/
>
>```



####7、示例

```java
package reflect;

import java.lang.reflect.*;

/**
 * @Author AlexanderBai
 * @data 2019/3/17 9:49
 */
interface China{
    public static final String NATIONAL = "China";
    public static final String NAME = "AlexanderBai";
    public void sayChinese();
    public String sayHello(String name);
}
class Person implements China{

    private String name;
    private int age;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    public Person(String name, int age) {
        this.name=name;
        this.age=age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }


    @Override
    public void sayChinese() {
        System.out.println("国籍" +NATIONAL);
    }

    @Override
    public String sayHello(String name) {
        return "你好，我是"+NAME;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + this.name + '\'' +
                ", age=" + this.age +
                '}';
    }
}

public class Test{
    public static void main(String[] args) {
        Class<?> c=null;
        try {
            c=Class.forName("reflect.Person");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        Class<?> interfaces[]=c.getInterfaces();
        for (int i = 0; i < interfaces.length; i++) {
            System.out.println("获取接口的名称：" + interfaces[i]);
        }
        System.out.println("获取父类的名称：" + c.getSuperclass());
        System.out.println();

        Constructor<?> constructors[]=c.getConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("获取构造方法的名称：" + constructor);
        }
        System.out.println();
        Method methods[]=c.getMethods();
        //Method methods1[]=c.getDeclaredMethods();
        for (int i = 0; i < methods.length; i++) {
            Class<?> parameterTypes[]=methods[i].getParameterTypes();
            Parameter parameters[]=methods[i].getParameters();

            System.out.println("取得方法的修饰符：" + Modifier.toString(methods[i].getModifiers()));
            System.out.println("取得方法的返回值类型：" + methods[i].getReturnType());
            System.out.println("取得方法名称：" + methods[i].getName());
            for (Class<?> parameterType : parameterTypes){
                System.out.println("取得参数类型名称：" + parameterType);
            }
            for (Parameter parameter : parameters) {
                System.out.println("取得方法的参数： " + parameter);
            }
            System.out.println();
        }

        System.out.println();
        System.out.println("-----------取得本类属性-----------");
        Field field[]=c.getDeclaredFields();
        for (int i = 0; i < field.length; i++) {
            System.out.println("属性修饰符："+Modifier.toString(field[i].getModifiers()));
            System.out.println("属性类型：" + field[i].getType());
            System.out.println("属性名称：" + field[1].getName());
        }

        System.out.println();
        System.out.println("-----------取得父类属性-----------");
        Field field1[]=c.getDeclaredFields();
        for (int i = 0; i < field1.length; i++) {
            System.out.println("属性修饰符："+Modifier.toString(field1[i].getModifiers()));
            System.out.println("属性类型：" + field1[i].getType());
            System.out.println("属性名称：" + field1[1].getName());
        }
    }
}
```

### 三、Java反射机制的深入应用

#### 1、通过反射调用类中的方法

```java
package reflect;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @Author AlexanderBai
 * @data 2019/3/17 9:49
 */
interface China{
    public static final String NATIONAL = "China";
    public static final String NAME = "AlexanderBai";
    public void sayChinese();
    public String sayHello(String name);
}
class Person implements China{

    private String name;
    private int age;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    public Person(String name, int age) {
        this.name=name;
        this.age=age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }


    @Override
    public void sayChinese() {
        System.out.println("国籍:" +NATIONAL);
    }

    @Override
    public String sayHello(String name) {
        return "你好，我是"+name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + this.name + '\'' +
                ", age=" + this.age +
                '}';
    }
}

public class Test{
    public static void main(String[] args) {
        Class<?> c=null;
        try {
            c=Class.forName("reflect.Person");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        Method method = null;
        try {
            //①此方法有参数
            method = c.getMethod("sayHello", String.class);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
        String string=null;
        try {
            //掉用方法时必须传递对象实例，同时传递对应的参数
            string= (String) method.invoke(c.newInstance(), "AlexanderBai");
            System.out.println(string);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        }

        try {
            //②此方法无参数
            Method method1=c.getMethod("sayChinese");
            try {
                ////掉用方法时必须传递对象实例
                method1.invoke(c.newInstance());
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            } catch (InvocationTargetException e) {
                e.printStackTrace();
            } catch (InstantiationException e) {
                e.printStackTrace();
            }
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        }
    }
}
```

#### 2、调用setter及getter方法

> - **破费版**
>
> ```java
> package reflect;
> 
> import java.lang.reflect.InvocationTargetException;
> import java.lang.reflect.Method;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/17 9:49
>  */
> interface China{
>     public static final String NATIONAL = "China";
>     public static final String NAME = "AlexanderBai";
>     public void sayChinese();
>     public String sayHello(String name);
> }
> class Person implements China{
> 
>     private String name;
>     private int age;
> 
>     public Person() {
>     }
> 
>     public Person(String name) {
>         this.name = name;
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
> 
>     @Override
>     public void sayChinese() {
>         System.out.println("国籍:" +NATIONAL);
>     }
> 
>     @Override
>     public String sayHello(String name) {
>         return "你好，我是"+name;
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
> public class Test{
>     public static void main(String[] args) {
>         Class<?> c=null;
>         try {
>             c=Class.forName("reflect.Person");
>         } catch (ClassNotFoundException e) {
>             e.printStackTrace();
>         }
> 
>         Method method = null;
>         try {
>             //①此方法有参数
>             method = c.getMethod("setName", String.class);
>         } catch (NoSuchMethodException e) {
>             e.printStackTrace();
>         }
>         try {
>             Person newInstance= (Person) c.newInstance();
>             //掉用方法时必须传递对象实例，同时传递对应的参数
>             method.invoke(newInstance, "AlexanderBai");
>             System.out.println(newInstance.getName());
>         } catch (IllegalAccessException e) {
>             e.printStackTrace();
>         } catch (InvocationTargetException e) {
>             e.printStackTrace();
>         } catch (InstantiationException e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```

> - **改进版**
>
> ```java
> package reflect;
> 
> import java.lang.reflect.InvocationTargetException;
> import java.lang.reflect.Method;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/17 9:49
>  */
> interface China{
>     public static final String NATIONAL = "China";
>     public static final String NAME = "AlexanderBai";
>     public void sayChinese();
>     public String sayHello(String name);
> }
> class Person implements China{
> 
>     private String name;
>     private int age;
> 
>     public Person() {
>     }
> 
>     public Person(String name) {
>         this.name = name;
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
> 
>     @Override
>     public void sayChinese() {
>         System.out.println("国籍:" +NATIONAL);
>     }
> 
>     @Override
>     public String sayHello(String name) {
>         return "你好，我是"+name;
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
> public class Test{
>     /**
>      * @param object   操作的对象
>      * @param att     操作的属性
>      * @param value   设置的值    
>      * @param type    参数的类型
>      */
>     public static void setter(Object object, String att, Object value,Class<?> type){
>         try {
>             Method method=object.getClass().getMethod("set" + initStr(att), type);
>             try {
>                 method.invoke(object, value);
>             } catch (IllegalAccessException e) {
>                 e.printStackTrace();
>             } catch (InvocationTargetException e) {
>                 e.printStackTrace();
>             }
>         } catch (NoSuchMethodException e) {
>             e.printStackTrace();
>         }
>     }
> 
>     public static void getter(Object object, String att) {
>         try {
>             Method method = object.getClass().getMethod("get" + initStr(att));
>             try {
>                 System.out.println(method.invoke(object));
>             } catch (IllegalAccessException e) {
>                 e.printStackTrace();
>             } catch (InvocationTargetException e) {
>                 e.printStackTrace();
>             }
>         } catch (NoSuchMethodException e) {
>             e.printStackTrace();
>         }
>     }
> 
>     /**
>      * 单词首字母大写
>      * @param old
>      * @return
>      */
>     public static String initStr(String old) {
>         String string = old.substring(0, 1).toUpperCase() + old.substring(1);
>         return string;
>     }
> 
>     public static void main(String[] args) {
>         Class<?> c=null;
>         Object object=null;
>         try {
>             c=Class.forName("reflect.Person");
>         } catch (ClassNotFoundException e) {
>             e.printStackTrace();
>         }
>         try {
>             object=c.newInstance();
>         } catch (InstantiationException e) {
>             e.printStackTrace();
>         } catch (IllegalAccessException e) {
>             e.printStackTrace();
>         }
>         setter(object,"name","AlexanderBai",String.class);
>         System.out.print("姓名：");
>         getter(object, "name");
>         setter(object,"age",21,int.class);
>         System.out.print("年龄：");
>         getter(object,"age");
>     }
> }
> ```
>
> 



####3、通过反射操作属性

####4、通过反射操作数组

### 四、ClassLoader加载类

### 五、动态代理

### 六、工厂设计模式

