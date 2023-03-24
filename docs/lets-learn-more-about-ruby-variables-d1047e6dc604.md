# 让我们学习更多关于 Ruby 变量的知识

> 原文：<https://medium.com/nerd-for-tech/lets-learn-more-about-ruby-variables-d1047e6dc604?source=collection_archive---------26----------------------->

我的[上一篇文章](/nerd-for-tech/lets-learn-about-ruby-variables-8110f0a725d4)介绍了 Ruby 变量的概念，它充当数据的容器。变量有助于在程序中收集、存储和使用数据。而不是重写一些东西——字符串、数字、数组等等。—每当您在代码中需要它时，您可以将它赋给一个变量，并根据需要经常引用该变量。

**范围**

上次我重点讲了局部变量。顾名思义，局部变量具有局部*范围*。什么是范围？基本上，范围指的是可访问性。例如，局部变量只能在初始化它的代码块(如方法)中访问。我将在后面介绍方法，但这里有一个简单的例子:

```
def method1
  my_message = “I’m totally local. You can only access me in here.”
  puts my_message
end
```

如果我调用上面的方法，存储在局部变量 *my_message* 中的字符串将被打印出来。如果我尝试在另一个方法中使用该变量会发生什么？

```
def method2
  puts my_message
end
```

这一次，我们将看到一条错误消息，告诉我们有一个未定义的局部变量。有什么问题？它告诉我们，我们的第二个方法甚至看不到 *my_message* ，它只能在 *method1* 中访问，因为那是它被初始化的地方。这是局部范围。

除了局部变量，Ruby 还有三种其他类型的变量:全局变量、类变量和实例变量。

![](img/e965d1c93081ba05d11ca0a427885855.png)

照片由[凯尔·格伦](https://unsplash.com/@kylejglenn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/global?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

**走向全球**

你可以通过名字总是以其开头的指示符 *$* (又名*符号*)来识别全局变量。

```
$my_global_message = “I’m a global variable. I’m accessible anywhere!”
```

局部变量的范围最窄，而全局变量的范围最宽。正如我们在 *$my_global_message* 中的字符串所声明的，全局变量可以在整个程序中被访问，而不仅仅是在单个代码块中。

```
$my_global_message = “I’m a global variable. I’m accessible anywhere!”def method1
  my_message = “I’m totally local. You can only access me in here.”
  puts my_message
  puts $my_global_message
enddef method2
  puts $my_global_message
end
```

两种方法都可以看到并使用 *$my_global_message* ，而 *my_message* 在*方法 1* 之外仍然不可访问。

应该使用全局变量吗？一些[会说不](https://www.tutorialspoint.com/Why-should-we-avoid-using-global-variables-in-C-Cplusplus)，或者至少你应该谨慎使用它们。

**带有类别**的变量

Ruby 中的另外两种变量是实例变量和类变量。您将在实例变量名称的开头看到符号 *@* ，而类变量名称以 *@@* 开头。

```
@an_instance_variable = “I can vary from one instance of a class to another.”@@a_class_variable = “I’m the same for all instances of a class.”
```

您将在类中使用这些类型的变量，这是下一次的主题。我只是想先给你提个醒。

**快速问答**

1.  是非判断:局部变量只能在初始化它们的代码块中访问。
2.  局部变量的名称中可以包含哪些字符？
3.  是非判断:局部变量的名称可以以数字开头。
4.  你必须声明一个 Ruby 变量保存的数据类型吗？
5.  以下代码的输出会是什么？

```
num = 63
txt_num = “50”
puts num + txt_num
```

a.113
b. 6350
c .错误信息

6.以下代码的输出会是什么？

```
student = ‘{ “name” => “Roosevelt Franklin”, “age” => 15, “dob” => “February 11, 2006” }’puts student.class
```

a.哈希
b .字符串
c .错误消息

7.看看下面的代码:

```
favorite_character = “Mickey Mouse”
```

变量名与其值之间的 *=* 怎么称呼？

**上一次测验的答案**

1.  Ruby 有多少种数据类型？**回答** : 6
2.  命名这些数据类型。**回答**:字符串、数字、布尔、数组、哈希、符号
3.  是非判断:Ruby 拥有原始数据类型。**答案**:假
4.  是非判断:Ruby 区分整数和浮点数。**回答**:真的
5.  Ruby 中唯一“假”的值是什么？**回答** : *假*和*零*
6.  是非判断:Ruby 是一种纯面向对象的语言。**回答**:真的
7.  *放置*命令和*打印*命令有什么区别？**回答**:*puts*命令创建一个新的换行符，但是 *print* 命令不会。
8.  是非判断:数组中的元素必须是相同的数据类型。**回答** : false(数组可以包含任意混合的数据类型！)