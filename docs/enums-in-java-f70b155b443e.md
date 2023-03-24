# JAVA 中的枚举

> 原文：<https://medium.com/nerd-for-tech/enums-in-java-f70b155b443e?source=collection_archive---------0----------------------->

![](img/7d55c0475b03150ca9772f73987265b0.png)

Java enum 是一种特殊类型的类，我们可以用它来表示常量变量。
通常我们可以用 final 关键字写出一个常数值。

```
package enumexamples;public class EnumExamples
{
    public static void main(String[] args) {     
        **final String ANIMAL = "dog";**
        System.out.println(ANIMAL);
    }
}//output -> dog
```

如果你需要在主方法之外声明一个常量变量，你必须声明它为一个**公共静态**变量。因为主方法是一个公共的静态方法。没有其他类型可以给出输出。

```
package enumexamples;public class EnumExamples
{
    **public static final String ANIMAL = "dog";**

    public static void main(String[] args) {    
        System.out.println(ANIMAL);
    }
}//output -> dog
```

假设您有多个常量要声明。每次都必须声明为“public static final dataType VariableName”。所以作为一个解决方案，java enum 被引入。

现在让我们看看如何声明、使用枚举及其特性。

# 如何声明 enum？

我们可以将它声明为一个具有类名和常量变量的类。

```
enum animals{
    DOG, CAT, RABBIT;
}
```

在上面的例子中，枚举类名是 animals，常量变量是 DOG、CAT 和 RABBIT。

# 如何调用枚举中的常量变量？

我们可以使用枚举类名和变量名在枚举类中调用常量变量，如下例所示。*(枚举类名.常量变量)*

**方法一**

```
package enumexamples;public class EnumExamples
{
    **enum animals{
       DOG, CAT, RABBIT;
    }**
    public static void main(String[] args) {
        System.out.println(**animals.DOG**);
    }
}
//output -> dog
```

**方法二**

```
package enumexamples;**enum animals{
    DOG, CAT, RABBIT
}**

public class EnumExamples
{    
    public static void main(String[] args) {
       **animals name = animals.RABBIT;**
       System.out.println(name);
    }
}
//output -> RABBIT
```

# 在这里可以使用枚举。

## 1.在一个类中。

```
package enumexamples;public class EnumExamples
{
    **enum animals{
        DOG, CAT, RABBIT;
    }**    
    public static void main(String[] args) {     
       System.out.println(**animals.DOG**);
    }
}
//output -> dog
```

## 2.课外

```
package enumexamples;**enum animals{
    DOG, CAT, RABBIT;
}**

public class EnumExamples
{    
    public static void main(String[] args) {
       System.out.println(**animals.DOG**);
    }
}
//output -> dog
```

## 3.作为同一个包中的外部类。

为此，让我们将 enum 类作为 animals，而 Main 类调用 enum 类中的常量变量。

枚举类-animals.java

```
package enumexamples;public **enum** animals{
    DOG, CAT, RABBIT;
}
```

主要类别-EnumExamples.java

```
package enumexamples;public class EnumExamples
{    
   public static void main(String[] args) {
       System.out.println(**animals.CAT**);
   }
}
//output -> CAT
```

# 枚举的特征

**1。Enum 是一个公共的静态类。**

因为我们在 main 方法内部调用 enums。而且只有静态和公共变量是可以在 main 方法内部调用的。

**2。无法使用枚举创建对象。**

```
package enumexamples;enum animals{
    DOG, CAT, RABBIT
}

public class EnumExamples
{    
    public static void main(String[] args) {
        **animals obj = new animals();
        obj = animals.CAT;**
       System.out.println(**obj**);
    }
}
//output -> **enum types may not be instantiated (error)**
```

**3。可以在开关情况下使用 enum**

我们可以像往常一样调用 enum 变量，并将它们用于如下所示的切换情况。

```
package enumexamples;public class EnumExamples {

    **enum animals {
        DOG, CAT, RABBIT;
    }**

    public static void main(String[] args) {
        **animals name = animals.RABBIT;**
        switch(name) {
            case DOG:
                System.out.println("This is a dog");
                break;
            case CAT:
                System.out.println("This is a cat");
                break;
            **case RABBIT:
                System.out.println("This is a rabbit");
                break;**
            default:
                System.out.println("Another animal");
        }
    }
}
output -> **This is a rabbit**
```

但是在 switch 情况下使用枚举有两个重要的特性。

i. **我们只能使用枚举中声明的变量作为案例名。如果我们使用任何其他的，就会出错。**

```
package enumexamples;public class EnumExamples {

    enum animals {
        DOG, CAT, RABBIT;
    }

    public static void main(String[] args) {
        animals name = animals.RABBIT;
        switch(name) {
            case DOG:
                System.out.println("This is a dog");
                break;
            case CAT:
                System.out.println("This is a cat");
                break;
            case RABBIT:
                System.out.println("This is a rabbit");
                break;
            **case COW:
                System.out.println("This is a cow");
                break;**
            default:
                System.out.println("Another animal");
        }
    }
}
//output -> **an enum switch case label must be the unqualified name of an enumeration constant (error)**
```

二。**我们只能使用 enum 变量来切换变量。**未在枚举类中声明的任何其他变量都不能用于切换情况。

```
package enumexamples;public class EnumExamples {

    enum animals {
        DOG, CAT, RABBIT;
    }

    public static void main(String[] args) {
        **animals name = animals.COW;**
        switch(name) {
            case DOG:
                System.out.println("This is a dog");
                break;
            case CAT:
                System.out.println("This is a cat");
                break;
            case RABBIT:
                System.out.println("This is a rabbit");
                break;
            default:
                System.out.println("Another animal");
        }
    }
}
//output -> **cannot find symbol(error)**
```

**4。枚举类充当普通类。**

以便我们可以在 enum 类中创建构造函数、方法和变量。

枚举类-animals.java

```
package enumexamples;
public enum animals {
    **//EnumVariableName("description", noOfLegs)**   
    DOG("this is a dog", 4),
    PARROT("this is a parrot",2),
    CAT("this is a cat", 4),
    RABBIT("this is a rabbit", 4),
    SPIDER("this is a spider",8);

    **//Final variables**
    private final String DESC;
    private final int NoOfLegs;

    **//constructor**
    animals(String description, int noOfLegs){
        DESC = description;
        NoOfLegs = noOfLegs;
    }

    **//method to get description**
    public String getDesc(){
        return DESC;
    }

    **//method to get no. of legs**
    public int getNoOfLegs(){
        return NoOfLegs;
    }
}
```

主要类别-EnumExamples.java

```
package enumexamples;public class EnumExamples {  
    public static void main(String[] args) {
        **animals name = animals.CAT;**
        System.out.println("Animal name:" + **name**);
        System.out.println("Description:" + **name.getDesc()**);
        System.out.println("No.Of legs:" + **name.getNoOfLegs()**);               
    }
}
//output -> Animal name:CAT
            Description:this is a cat
            No.Of legs:4
```

**5。可以通过使用 for each 循环调用 enum 中的所有值**

对于这个，我们可以使用 for-each 循环。在 for each 循环中，我们可以声明一个 enum 类变量，并调用 enum 类中的所有细节。

主要类别-EnumExamples.java

```
package enumexamples;public class EnumExamples {  
    public static void main(String[] args) {
        **for(animals name : animals.values()){
            System.out.println(name + ", " + name.getDesc() + ", " +name.getNoOfLegs());
        } **              
    }
}
```

在上面的例子里面，对于每个循环，我们都有 call values()方法。此方法调用枚举中的所有值，并将它们存储在一个数组中。

通过使用在第四点中创建的相同的 animals 的 enum 类，如果我们运行上面的 main 类，输出将如下。它显示了 enum 类中的所有细节。

```
DOG, this is a dog, 4
PARROT, this is a parrot, 2
CAT, this is a cat, 4
RABBIT, this is a rabbit, 4
SPIDER, this is a spider, 8
```

**我们可以修改它来打印范围**内的输出。为此，我们需要使用 **java。util。EnumSet 类**。这里它有一个功能叫做 **EnumSet.range** 。在里面，我们可以给出范围。通过使用它，我们可以在一个范围内调用枚举内部的值。r

```
package enumexamples;
**import java.util.EnumSet;**
public class EnumExamples {

   public static void main(String[] args) {
       for(animals name: **EnumSet.range(animals.CAT, animals.SPIDER)**){
           System.out.println(name + " "+ name.getDesc()+" " + name.getNoOfLegs());
       }
   }
}
//output -> CAT this is a cat 4
            RABBIT this is a rabbit 4
            SPIDER this is a spider 8
```

# 总结:

*   枚举是一种特殊类型的类。
*   枚举用于声明常数。
*   Enum 类是一个公共的静态类。
*   我们可以在类内、类外创建枚举，也可以在同一个包中创建一个新文件。
*   无法使用枚举创建对象。
*   For switch case 只能使用枚举中声明的值。
*   可以在枚举类中创建构造函数、方法和变量。

**♥️♥️感谢您的阅读。敬请关注即将发布的文章。♥️♥️**