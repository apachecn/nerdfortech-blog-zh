# 构建器设计模式(C++和 C#中的流畅界面)

> 原文：<https://medium.com/nerd-for-tech/builder-design-pattern-fluent-interface-c-70cae9490a91?source=collection_archive---------3----------------------->

![](img/989f3106898fe2b33695c377411e6aaf.png)

构建器设计模式是一种简单但功能强大的创建模式，它通过封装复杂的对象构造逻辑，将对象构造与实现分离，并将构造责任委托给不同的类。该模式适用于从同一对象构建多个表示。

> 构建器模式使用简单的对象和一步一步的方法构建复杂的对象。这种类型的设计模式属于创建模式，因为这种模式提供了创建对象的最佳方式之一。[ [由导师点](https://www.tutorialspoint.com/design_pattern/builder_pattern.htm)

基于[代码气味](https://en.wikipedia.org/wiki/Code_smell#:~:text=In%20computer%20programming%2C%20a%20code,%2C%20developer%2C%20and%20development%20methodology.&text=It%20is%20also%20a%20term%20used%20by%20agile%20programmers.)应用构建器设计模式:

*   您的类有一个带有几个可选参数的构造函数
*   构造函数中复杂的逻辑和多重条件检查
*   一些参数组合是不允许的(例如:洲和国家)

示例:

这个例子演示了如何利用构建器模式，使用多个构建器类和一个流畅的接口来构建电子邮件对象。在这里，电子邮件对象是用 header 和 body builder 类分多个阶段构建的。每个构建器都有一系列的方法，如 `***from()***`、 `***to()***`、`***subject()***`。这个例子可能看起来有点夸张，因为使用的电子邮件并不复杂。

***email.h***

*这是 email 类，有私有变量，如*`*from*`*`*to*`*`*subject*`*`*body*` *和* `*attachment*` *。Email 类有一组 friend 类，以便访问上面的私有变量。该构造函数是私有的，以防止直接从其他类创建电子邮件对象。****

```
**#pragma once
#include <string>
#include <sstream>
class EmailBuilder;class Email
{
public:
    friend class EmailBuilder;
    friend class EmailHeaderBuilder;
    friend class EmailBodyBuilder;
    friend std::ostream &operator<<(std::ostream &os, const Email &obj);
    static EmailBuilder create();private:
    Email() = default; std::string m_from;
    std::string m_to;
    std::string m_subject;
    std::string m_body;
    std::string m_attachment;
};**
```

*****email.cpp*****

**运算符(<**

```
**#include "email.h"
#include "emailbuilder.h"
EmailBuilder Email::create()
{
    return EmailBuilder{};
}
std::ostream &operator<<(std::ostream &os, const Email &obj)
{
    return os
           << "from: " << obj.m_from << std::endl
           << "to: " << obj.m_to << std::endl
           << "subject: " << obj.m_subject << std::endl
           << "body: " << obj.m_body << std::endl
           << "attachment: " << obj.m_attachment << std::endl;
}**
```

*****abstract email builder . h*****

**基本构建器类有构建器创建方法，如`header()`和`body()`来获得所需的构建器实现。它有一个`EmailBodyBuilder` 和`EmailHeaderBuilder`的转发类声明。电子邮件的 Cast 运算符也被重载，以将最终构造的电子邮件对象所有权返回给调用者。**

```
**#pragma once
#include "email.h"
class EmailHeaderBuilder;
class EmailBodyBuilder;
class AbstractEmailBuilder
{
protected:
    Email &m_email;
    explicit AbstractEmailBuilder(Email &email) : m_email(email) {}
public:
    operator Email() const
    {
        return std::move(m_email);
    };
    EmailHeaderBuilder header() const;
    EmailBodyBuilder body() const;
};**
```

*****abstract email builder . CPP*****

```
**#include "abstractEmailBuilder.h"
#include "emailbuilder.h"
#include "emailBodyBuilder.h"
#include "emailHeaderBuilder.h"
EmailHeaderBuilder AbstractEmailBuilder::header() const
{
    return EmailHeaderBuilder{m_email};
};
EmailBodyBuilder AbstractEmailBuilder::body() const
{
    return EmailBodyBuilder{m_email};
};**
```

*****email builder . h*****

**一个具体的类实现，初始化 email 对象并将其传递给抽象类。这将允许生成器类访问该对象。**

```
**#pragma once
#include "email.h"
#include "abstractEmailBuilder.h"
class EmailBuilder : public AbstractEmailBuilder
{
    Email m_email;
public:
    EmailBuilder() : AbstractEmailBuilder{m_email}
    {
    }
};**
```

*****email body builder . h*****

**使用`EmailBodyBuilder` 具体类演示邮件正文的创建。**

```
**#pragma once
#include <string>
#include "emailbuilder.h"
class EmailBodyBuilder : public AbstractEmailBuilder
{
public:
    explicit EmailBodyBuilder(Email &email)
        : AbstractEmailBuilder{email}
    {
    }
    EmailBodyBuilder &body(const std::string &body)
    {
        m_email.m_body = body;
        return *this;
    }
    EmailBodyBuilder &attachment(const std::string &attachment)
    {
        m_email.m_attachment = attachment;
        return *this;
    }
};**
```

*****emailheaderbuilder . h*****

**使用`EmailHeaderBuilder` 具体类演示电子邮件标题的创建。**

```
**#pragma once
#include <string>
#include "emailbuilder.h"
class EmailHeaderBuilder : public AbstractEmailBuilder
{
public:
    explicit EmailHeaderBuilder(Email &email)
        : AbstractEmailBuilder{email}
    {
    }
    EmailHeaderBuilder &from(const std::string &from)
    {
        m_email.m_from = from;
        return *this;
    }
    EmailHeaderBuilder &to(const std::string &to)
    {
        m_email.m_to = to;
        return *this;
    }
    EmailHeaderBuilder &subject(const std::string &subject)
    {
        m_email.m_subject = subject;
        return *this;
    }
};**
```

*****main.cpp*****

```
**#include <iostream>
#include <sstream>
#include <string>
#include "email.h"
#include "emailbuilder.h"
#include "emailHeaderBuilder.h"
#include "emailBodyBuilder.h"
using namespace std;
int main()
{
    Email mail = Email::create()
                     .header()
                         .from("test1[@example.com](mailto:sukithaj@gmail.com)")
                         .to("[test2@](mailto:test@gmail.com)[example](mailto:sukithaj@gmail.com)[.com](mailto:test@gmail.com)")
                         .subject("This is a test mail")
                     .body()
                         .body("This is a test body")
                         .attachment("This is a test attachment");
    std::cout << mail << std::endl;
}**
```

****结论****

**生成器设计模式的变化很少，这个例子是 c++的流畅接口实现。常见的实现是使用基于目录的类结构。如果你想了解更多，网上有很多资源。**

**应用设计模式的理想时机是重构阶段。代码气味是发现什么需要改变以及哪些类和代码段需要改变的最好方法。以后我会写更多与代码气味相关的故事。**

**感谢 [Desmond Harris Fernando](https://medium.com/u/36cefa51daac?source=post_page-----70cae9490a91--------------------------------) 对 [C#代码](https://github.com/sukitha/BuilderPattern)的审核。**