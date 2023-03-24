# 单一责任原则

> 原文：<https://medium.com/nerd-for-tech/the-single-responsibility-principle-6f74574ccede?source=collection_archive---------3----------------------->

这是**坚实**原则的 5 条规则中的第一条。

单一责任又名 **SRP** 规定每个软件组件应该有且只有一个责任。这个组件可以是一个类，一个方法，甚至是一个模块。

与其有一把**解决一切的瑞士军刀**，不如有一堆可以适合单一责任的单刀，只待切割。

SRP 的目标是以一种有效的方式击败凝聚力。在一个更正式的定义中，内聚性是软件组件的各个部分相关的程度。

一个很好的类比是，当一个地方有一堆垃圾时，这些垃圾按照垃圾的类型进行隔离和分类。这种分离过程可能产生高内聚力，也可能产生相反的结果。

这里有一个应用程序中可能的实际解决方案的快速示例。

```
public class Square { int side = 5; public int calculateArea() {
    return side * side;
  } public int calculatePerimeter() {
    return side * 4;
  } public void draw() {
   if (highResolutionMonitor) {
    // render high resolution image
   } else {
    // render normal image
   }
  } public void rotate(int degree) {
  // rotate the image with the selected degree
 }
}
```

这段代码提供了 4 个方法，但是`calculateArea`和`calculatePermiter`是具有真正紧密内聚级别的方法，而`draw`和`rotate`方法没有那么紧密。

如果你还记得的话，我们的目标是获得好的内聚性，我们应该总是以在一个组件中编写高内聚性为目标。

`draw`和`rotate`方法都可以放在别处，并“委托”呈现功能的责任。所以，请假设这两个方法都将从`Square.java`文件和`Square`类定义中消失。

为了简单起见，这个新的渲染代码将被命名为`SquareUI.java`，下面是它的样子

```
public class SquareUI { public void draw() {
  if(highResolutionMonitor) {
    // render high resolution image
  } else {
    // render normal image
  }
 } public void rotate(int degree) {
  // rotate the image with the selected degree
 }
}
```

`Square.java`现在看起来像这样。

```
public class Square { int side = 5; public int calculateArea() {
  return side * side;
} public int calculatePerimeter() {
  return side * 4;
 }
}
```

所以。

1.  `Square.java`仅用于测量方块
2.  `SquareUI.java`用于渲染目的。

这是用于演示目的的最简单的示例。但这可以适用于一切。

```
public class Student {private String studentId;
private Date studentDOB;
private String address;public void save() {
    String objectStr = MyUtils.serializeIntoAString(this);
    Connection connection = null;
    Statement stmt = null;
    try {
      Class.forName("com.mysql.jdbc.Driver");
      DriverManager.getConnection("jdbc:mysql://localhost:3306", "MyDB", "root", "password");
      stmt = connection.createStatement;
      stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
    } catch (Exception e) {
      e.printStackTrace();
    }
  }public String getStudentId() {
    return studentId;
  }public void setStudentId(String studentId) {
    this.studentId = studentId;
  }}
```

这段代码在`save`方法中执行数据库连接，这也基于启动的实例获取数据(OOP 视角)。

这在 ORM 实现中很常见。但是当然，这可以改变，并分裂成具有特定关注点的文件。

这里发现的另一个有趣的概念是耦合，它被定义为各种软件组件之间的相互依赖程度。耦合越紧密，就越不理想。

那么，是如何实现的呢？

嗯，`save`方法只是执行到数据库的连接，它只是将`Student`类序列化为持久化的形式，但是类属性、`getStudentId`和`setStudentId`与数据交互。

`Student`类应该理想地处理相关的功能，而不应该知道与处理后端接口相关的底层细节。

所以，再一次。

对于`Student.java`类，重构将如下所示

```
public class Student {private String studentId;
  private Date studentDOB;
  private String address;public void save() {
     new StudentRepository().save(this);
  }public String getStudentId() {
    return studentId;
  }public void setStudentId(String studentId) {
    this.studentId = studentId;
  }}
```

另一个文件将被创建，称为`StudentRepository.java`将用于后端实例

```
public class StudentRepository {public void save() {
    String objectStr = MyUtils.serializeIntoAString(this);
    Connection connection = null;
    Statement stmt = null;
    try {
      Class.forName("com.mysql.jdbc.Driver");
      DriverManager.getConnection("jdbc:mysql://localhost:3306", "MyDB", "root", "password");
      stmt = connection.createStatement;
      stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
    } catch (Exception e) {
      e.printStackTrace();
    }
  }}
```

这有助于更好地遵守 SRP

`Student`类将处理核心学生档案数据，而`StudentRespository`类将处理后端/数据库操作。

我认为在这一点上，整个原则变得非常清晰。

软件从不休眠，它总是在不断变化。如果你有任何改变的理由，改变的频率就会增加，错误的数量也会增加。

遵循 SRP 可以节省大量的软件维护成本。

让我们在下一篇文章中继续讨论开闭原则。