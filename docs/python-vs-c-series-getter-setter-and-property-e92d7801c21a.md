# Python vs C++系列:Getter、Setter 和 Property

> 原文：<https://medium.com/nerd-for-tech/python-vs-c-series-getter-setter-and-property-e92d7801c21a?source=collection_archive---------5----------------------->

作为一个从 C++03 开始的专业 C++程序员，C++ way 面向对象的思想已经深深地植入了我的脑海，在我重新拾起 C#和 Java 等一门新语言的时候，它给了我很大的帮助。然而，当我第一次遇到 Python 时，这种好处并不明显。Python 也是一种面向对象的编程语言，但与其他面向对象的编程语言(如 C++和 Java)有很大的不同。因此，本系列试图指出一些可能让 C++程序员感到惊讶的值得注意的 Python 编程，而且它不是 Python 教程。我希望我的经验可以帮助懂 C++的人更简单地掌握 Python。

Python 与 C++系列的第一篇文章从一个基本的面向对象编程概念开始——封装和访问函数。

注意，本系列中的 Python 代码假设使用 Python 3.7 或更高版本。

# 封装的简要回顾

封装是一个面向对象的编程概念，它封装了一个类的实现细节。为什么不透露实施细节？一个好的编程实践是客户端代码应该只访问类的公共接口。只要接口保持不变，如果实现改变，客户机代码就不需要改变。封装还降低了复杂性，简化了调试过程。此外，封装的类有助于保护数据并防止误用，因为客户端代码不能访问类的受保护部分。

应该保护什么？如果需要，如何访问？

除了实现细节之外，数据成员通常也是应该保护的细节。但是，提供一个公共接口允许客户端代码访问类上下文中的数据成员可能是合适的。这种类型的接口通常被称为访问功能。访问函数通常有两种风格:getter 和 setter。getter 是当我们访问数据成员进行读取时调用的方法。与 getter 不同，setter 是一种被调用来修改数据成员的方法。

**访问功能的好处**

除了提供数据成员的可访问性，访问函数还提供了其他好处。例如，setter 可以在返回之前执行一些操作，getter 可以以友好的格式返回值。我们可以为 getters 添加检查逻辑，以便在更新数据成员之前验证输入值是否有效。

# C++中的访问函数

对 C++类成员的访问限制由类体内的访问说明符标记，这些说明符是 *public* 、 *private* 和 *protected* 。public 部分中的类成员是可公开访问的。只有类 self 可以访问私有部分中的成员。受保护的成员可以由类 self 及其派生类使用。

下面的例子演示了 C++中 getters 和 setters 的基本思想。

```
class MyClass
{
    public:
        int getMyData() {
            return myData;
        }

        void setMyData(int value) {
            myData = value;
        }

    private:
        int myData = 0;
};
```

# Python 中的访问控制和属性

Python 不使用访问说明符来限制对类的访问。事实上，Python 没有一种机制来阻止客户端代码访问私有成员。在 Python 中一切都是可访问的；一切公开。注意术语 **private** 通常不在 Python 编程中使用，因为在 Python 中没有真正私有的属性。术语 **internal** 用于表示支持在内部使用的属性。

尽管 Python 不限制任何访问，但这并不意味着封装对 Python 不再重要。这只是意味着 Python 有不同的方法来支持数据封装。

# 命名约定

Python 编程依赖于命名约定来建立代码所有者和用户之间的契约。如果一个类成员是内部的，它的名字以一个下划线开始；否则，它就是公共的。举个例子，

```
class MyClass:

    def __init__(self) -> None:
        # this variable starts with an underscore (_),
        # it indicates it is supposed to be used internally only
        self._my_data: int = 0

    def get_my_data(self) -> int:
        return self._my_data

    def set_my_data(self, value: int) -> None:
        self._my_data = value

    # A method starts with an underscore (_), also means private.
    def _private_method(self) -> None:
        pass
```

当 Python 程序员看到一个以前导下划线命名的类成员时，他们会知道这是打算在内部使用的，而不是由外部代码访问的。

但是，如果客户端真的想这样做，客户端代码仍然可以访问内部成员。以下代码完全有效。

```
my_class = MyClass()
my_class._my_data = 10
```

(示例代码可从[https://github . com/shunsvineyard/shunsvineyard/blob/main/python _ vs _ CPP _ series/getter _ setter _ and _ property/python _ example . py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/getter_setter_and_property/python_example.py)获得)

**姓名莽撞**

除了以单下划线为前缀之外，双前导下划线也表示私有。然而，它们的行为略有不同。区别在于名称管理——当一个类成员用双前导下划线命名时(例如， *__my_member* )，名称管理被调用，其名称被替换为一个在实际名称前包含下划线和类名的名称，如 *_ClassName__my_member。*

名称混乱使得访问内部成员更加困难。但是，客户端代码仍然可以通过名称访问带有双前导下划线的内部成员。请参见下面的示例。

```
class MyClass:
    def __init__(self) -> None:
        # Name mangling
        self.__my_member = 0
if __name__ == "__main__":
    my_class._MyClass__my_member = 10
```

虽然没有什么真正阻止对内部属性的访问，但是命名约定至少告诉客户属性是在内部使用的；如果你真的想访问它们，请小心。

关于 Python 命名约定的更多细节可以在 [PEP8 —命名约定](https://www.python.org/dev/peps/pep-0008/#naming-conventions)中找到。

**单前导下划线还是双前导下划线？**

关于使用单前导下划线或双前导下划线来命名内部属性，我们应该总是倾向于单前导下划线。使用双下划线是令人沮丧的。根据 [PEP8](https://www.python.org/dev/peps/pep-0008/#method-names-and-instance-variables) ，“只对非公共方法和实例变量使用一个前导下划线。为了避免与子类的名称冲突，使用两个前导下划线来调用 Python 的名称管理规则。此外，用双下划线前缀命名的成员也会降低可读性，这可能会使客户端与 Python [的特殊方法](https://docs.python.org/3/reference/datamodel.html#special-method-names)混淆。

# Pythonic 实现 getters 和 setters 的方法:Property

因为 Python 中的一切都是公共的，所以像 C++一样提供 getter 和 setter 等访问函数没有意义。但有时，我们需要为内部属性设置 getters 和 setters，不是为了保护它们，而是为了在返回值或更新内部成员之前执行一些操作或检查。Python 的方式是使用 Python 语言提供的内置[装饰器](https://www.python.org/dev/peps/pep-0318/)的 *—* 。

下面的代码演示了如何使用 *@property* decorator 实现一个 getter 和一个 setter。

```
class Contact:
    def __init__(self, first_name: str, last_name: str) -> None:
        self._first_name = first_name
        self._last_name = last_name
        self._email: Optional[str] = None

    @property
    def name(self) -> str:
        # The @property decorator turns the name() method into
        # a getter for a read-only attribute with the same name,
        # so a client can access it by doing c.name if c is an 
        # instance of Contact.
        return f"{self._first_name} {self._last_name}"

    @property
    def email(self) -> str:
        # The @property docorator turns the email() method into
        # a getter.
        if self._email:
            return self._email
        else:
            return "No associated email"

    @email.setter
    def email(self, email_address) -> None:
        # A property object also has a setter method used as a 
        # decorator that create a copy of the property with the
        # corresponding accessor function set to the decorated 
        # function. Therefore, the setter decorator for email is
        # @email.setter. And a client can access it by doing 
        # c.email = email@email.com if c is an instance of Contact.
        if re.fullmatch(r"^\S+@\S+$", email_address):
            self._email = email_address
        else:
            raise ValueError(f"{email_address} is invalid.")
```

(示例代码可从[https://github . com/shunsvineyard/shunsvineyard/blob/main/python _ vs _ CPP _ series/getter _ setter _ and _ property/python _ property . py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/getter_setter_and_property/python_property.py)获得)

*@property* decorator 提供了一种实现 getters 和 setters 的好方法，我们可以对它们应用一些操作或检查。对于代码开发人员和客户来说， *@property* decorator 还增加了 getters 和 setters 代码的可读性。在上面的例子中，客户端代码可以像下面这样访问属性。

```
contact = Contact(first_name="John", last_name="Wick")
contact.email = "john.wick@email.com"
print(f"Name: {contact.name}")
print(f"Email: {contact.email}")
```

输出如下所示:

```
Name: John Wick
Email: john.wick@email.com
```

# 结论

尽管 *@property* 并不阻止客户端访问下划线成员(在本文的示例中为 *_first_name* 、 *_last_name* 和 *_email* )，但它提供了一种 Pythonic 方式来定义访问函数(即 getters 和 setters)。任何有经验的 Python 程序员都知道，他们应该访问 *@property* 成员，而不是内部成员。

*原载于 2021 年 9 月 25 日*[*https://shunsvineyard . info*](https://shunsvineyard.info/2021/09/25/python-vs-c-series-getter-setter-and-property/)*。*