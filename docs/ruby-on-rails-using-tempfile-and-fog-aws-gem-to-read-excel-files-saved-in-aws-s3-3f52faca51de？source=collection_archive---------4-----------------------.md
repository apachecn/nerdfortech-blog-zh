# Ruby on Rails:使用 Tempfile 和 Fog-AWS Gem 读取保存在 AWS S3 中的 excel 文件

> 原文：<https://medium.com/nerd-for-tech/ruby-on-rails-using-tempfile-and-fog-aws-gem-to-read-excel-files-saved-in-aws-s3-3f52faca51de?source=collection_archive---------4----------------------->

![](img/a757beb9257188730c61ffe1c0567c9a.png)

图片由[Williams 创作](https://pixabay.com/users/williamscreativity-17210051/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5469712)来自 [Pixabay](https://pixabay.com/)

[AWS S3(AWS 简单存储服务)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)已经成为人们存储文件或图像等对象的流行云服务。更有可能的是，我们需要在应用程序中实现从 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 访问文件或将文件上传到 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 的功能。最近，我也遇到一个需要阅读一堆。xlsx 文件存储在我的 Rails 应用程序中的 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 中，我只是想读取内容，所以将所有文件永久保存在我们的应用程序中并不是一个好主意。在尝试了一些解决方案后，我发现 [Fog-AWS](https://github.com/fog/fog-aws) Gem 和 Ruby 类方法: [Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) 是实现结果能够满足需要的有用方法。为了分享我如何使用它们，我写了这篇文章。本文分为两部分:第一部分是关于如何使用 [Fog-AWS](https://github.com/fog/fog-aws) Gem 来访问存储在 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 中的文件，第二部分是关于如何使用 Ruby Class [Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) 方法来存储。xlsx 文件，并在 Rails 应用程序中打开它。

# 访问存储在 AWS S3 中的文件

[Fog-AWS](https://github.com/fog/fog-aws) 是一个有用的宝石，可以从 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 访问文件，或者从你的 Rails 应用程序上传文件到 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 。只需将这一行添加到您的应用程序的 Gemfile 中，并运行`bundle install`，然后您就可以开始使用 [Fog-AWS](https://github.com/fog/fog-aws) 。

```
gem 'fog-aws'
```

## 初始化与 AWS S3 的连接

在从 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 访问文件之前，您需要先连接它。 [Fog-AWS](https://github.com/fog/fog-aws) 提供一个类`[Fog::Storage](https://github.com/fog/fog-aws/blob/master/lib/fog/aws/storage.rb)`来初始化连接。有两种方法连接 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 。一个是使用`aws_access_key_id`和`aws_secret_access_key`

```
s3 = Fog::Storage.new(
  provider: 'AWS', 
  region: 'us-west-2', 
  aws_access_key_id: YOUR_S3_ACCESS_KEY,
  aws_secret_access_key: YOUR_S3_SECRET_KEY
)
```

如果您的应用程序部署在 [AWS EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) 上，那么您可以使用另一种方式连接到 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 。那就是使用 [AWS IAM 角色](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)。通过设置可以访问 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 的特定 EC2 IAM 角色，您的应用程序可以通过在初始化连接时设置`use_iam_profile = true`来连接到 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 。

```
s3 = Fog::Storage.new(
  provider: 'AWS', 
  region: 'us-west-2', 
  use_iam_profile: true
)
```

## 列出 AWS S3 存储桶中的所有文件和访问文件

你可以在一个特定的 [AWS S3 桶](https://searchaws.techtarget.com/definition/AWS-bucket)中列出所有的文件。

```
s3_files = s3.directories.new(key: 'my-bucket').files
```

为了在 AWS S3 中访问文件，类`[Fog::Storage](https://github.com/fog/fog-aws/blob/master/lib/fog/aws/storage.rb)`提供了`get`方法来获取我们需要的文件。

```
s3_file = s3.directories.new(key: 'my-bucket')
                        .files
                        .get('my_folder/test.xlsx')
```

从 AWS S3 获取文件后，我们不能直接打开它，因为它只是一个如下的`[Fog::Storage::AWS::File](http://fog.io.s3-website-us-east-1.amazonaws.com/0.10.0/rdoc/Fog/Storage/AWS/File.html)`对象，我们应该通过 URL 解析它。

```
<Fog::Storage::AWS::File
    key="my_folder/test.xlsx",
    cache_control=nil,
    content_disposition=nil,
    content_encoding=nil,
    content_length=5488,
    content_md5=nil,
    content_type="binary/octet-stream",
    etag="38eed92f509046d915583875dd45454f",
    expires=nil,
    last_modified=2021-07-13 03:41:48 +0000,
    metadata={"x-amz-id-2"=>"XXX", "x-amz-request-id"=>"XXX"},
    owner=nil,
    storage_class=nil,
    encryption=nil,
    encryption_key=nil,
    version=nil,
    kms_key_id=nil
  >
```

## 创建文件 URL 并解析文件

类`[Fog::Storage](https://github.com/fog/fog-aws/blob/master/lib/fog/aws/storage.rb)`提供了创建文件 URL 的 URL 方法。它还允许我们设置 URL 过期的时间。

```
s3 = Fog::Storage.new(
  provider: 'AWS', 
  region: 'us-west-2', 
  use_iam_profile: true
)
s3_file = s3.directories.new(key: 'my-bucket')
                        .files
                        .get('my_folder/test.xlsx')
**url = s3_file.url(Time.zone.now + 60)**
```

在文件 URL 准备好之后，我们可以使用 Ruby 类`[URI](https://docs.ruby-lang.org/en/2.1.0/URI.html)`提供的`parse`方法通过它的 URL 解析文件。解析结果是 Ruby 类`[StringIO](https://ruby-doc.org/stdlib-2.5.1/libdoc/stringio/rdoc/StringIO.html)`的一个对象，如下

```
s3_temp_file = URI.parse(url).open=>#<StringIO:0x0000555e74fe1438 @base_uri=#<URI::HTTPS https://my-bucket.s3-us-west-2.amazonaws.com/my_folder/test.xlsx?X-Amz-Expires=60&X-Amz-Date=20210713T151552Z&X-Amz-Security-Token=XXX#X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIARPKY2DTZXLCOCK65/20210713/us-west-2/s3/aws4_request&X-Amz-SignedHeaders=host&X-Amz-Signature=XXX>, @meta={"x-amz-id-2"=>"XXX", "x-amz-request-id"=>"4MNS2VFY3DT0JKAT", "date"=>"Tue, 13 Jul 2021 15:16:02 GMT", "last-modified"=>"Tue, 13 Jul 2021 03:41:48 GMT", "etag"=>"\"38eed92f509046d915583875dd45454f\"", "accept-ranges"=>"bytes", "content-type"=>"binary/octet-stream", "server"=>"AmazonS3", "content-length"=>"5488"}, [@metas](http://twitter.com/metas)={"x-amz-id-2"=>["t+D6xAC6o1MrEwhHjyBFkPdQoBfvTmfQh4ytJ/lAheZ89R/u6yj78d6Secc0AuQhfUODdwedOIQ="], "x-amz-request-id"=>["4MNS2VFY3DT0JKAT"], "date"=>["Tue, 13 Jul 2021 15:16:02 GMT"], "last-modified"=>["Tue, 13 Jul 2021 03:41:48 GMT"], "etag"=>["\"38eed92f509046d915583875dd45454f\""], "accept-ranges"=>["bytes"], "content-type"=>["binary/octet-stream"], "server"=>["AmazonS3"], "content-length"=>["5488"]}, [@status](http://twitter.com/status)=["200", "OK"]>
```

当阅读对象时，内容不容易使用和理解。

```
s3_temp_file.read=>"PK\x03\x04\x14\x00\x00\x00\b\x00P\x1C\xEER\aAMb\x81\x00\x00\x00\xB1\x00\x00\x00\x10\x00\x00\x00docProps/app.xmlM\x8E=\v\x021\x10D\xFF\xCAq\xBD\xB7A\xC1Bb@\xD0R\xB0\xB2\x0F{\e/\x90dC\xB2B~\xBE9\xC1\x8Fn\x1Eo\x18F\xDF\ng*\xE2\xA9\x0E-\x86T\x8F\xE3\"\x92\x0F\x00\x15\x17\x8A\xB6N]\xA7n\x1C\x97h\xA5cy\x00;\xE7\x91\xCE\x8C\xCFHI`\xAB\xD4\x1E\xA8\t\xA5\x99\xE6M\xFE\x0E\x8EF\x9Fr\x0E\x1E\xADxN\xE6\xEA\xB1pe'\xC3\xA5!\x05\r\xFFrm\xDE\xA9\xD45\xEF&\xF5\x96\x1F\xD6\xF0;i^PK\x03\x04\x14\x00\x00\x00\b\x00P\x1C\xEERt\x80r,\xEF\x00\x00\x00+\x02\x00\x00\x11\x00\x00\x00docProps/core.xml\xCD\x92QK\xC30\x10\xC7\xBF\x8A\xE4\xBD\xBD\xA6\x15\x1D\xA1\xCB\x8BcO\n\x82\x03\xC5\xB7\x90\xDC\xB6`\x93\x86\xE4\xA4\xDD\xB77\xAD[\x87\xE8\a\xF01w\xFF\xFC\xEEwp\xAD\x0EB\xF7\x11\x9Fc\x1F0\x92\xC5t3\xBA\xCE'\xA1\xC3\x9A\x.....
```

为了使用文件内容并对其进行处理，我们应该使用 Ruby 类`File`将解析结果保存为一个文件，或者使用 Ruby 类`Tempfile`将其保存为一个临时文件并打开它。在下一节中，我将分享如何使用类`Tempfile`将解析结果保存为临时文件并打开它。

# 将 Excel 文件保存为临时文件并打开它

[Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) 已经内置了 Ruby 类，我们不需要安装。当您创建一个 [Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) 对象时，它将创建一个具有唯一文件名的临时文件，您可以对其执行所有常规文件操作，例如读取数据、写入数据、更改其权限等。当一个 [Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) 对象被垃圾收集时，或者当 Ruby 解释器退出时，其关联的临时文件被自动删除

## 创建临时文件

我们可以使用 [Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) 提供的`new`或`create`方法来创建一个临时文件。下面的代码展示了使用`new`方法创建一个临时文件。这将创建一个文件名包含`foo`的临时文件

```
tempfile = Tempfile.new('foo')
=> #<File:/var/folders/57/m85kgny17n70l64v009zhcfhmyj0p9/T/foo20210715-83391-157to5v>
```

将一些内容写入一个临时文件，它将返回写入了多少字符。

```
tempfile.write('hello world')
=> 11
```

如果我们想用`create`方法做和用`new`方法一样的事情，代码如下，它也创建一个文件名包含`foo`的临时文件

```
Tempfile.create('foo') do |f|
  f.write('hello world')
end
```

`new`和`create`方法的唯一区别是 create 方法接受一个块。

## 设置临时文件类型并读取文件

我们可以用特定的文件类型创建一个临时文件，例如。xlsx，。CSV…等。

```
Tempfile.new(['foo', '.xlsx'], encoding: 'ascii-8bit')=> #<File:/var/folders/57/m85kgny17n70l64v009zhcfhmyj0p9/T/foo20210715-33983-e1vs8.xlsx>
```

在我们创建了 temp 文件之后，在我们使用它之前，应该将文件内容写入 temp 文件，并且我们应该将文件设置为从第一行开始读取，因为在文件被写入一些内容之后，它将在最后一个新行中被读取，并且没有内容。

```
tempfile = Tempfile.new('foo')
tempfile.write('hello world')
tempfile.read=> ""
```

我们可以使用类`[Tempfile](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html)`提供的`rewind`方法来设置从第一行读取的临时文件。

```
tempfile = Tempfile.new('foo')
tempfile.write('hello world')
tempfile.rewind
tempfile.read=> "hello world"
```

因此，将来自 [AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) 的. xlsx 文件保存为临时文件的代码如下

```
s3_temp_file = URI.parse(url).open            
tempfile = Tempfile.new([test, '.xlsx'], encoding: 'ascii-8bit')            
tempfile.write(s3_temp_file.read)            
tempfile.rewind
```

## 打开 excel 类型的临时文件(。xlsx)

最后，我们可以开始使用临时文件。tempfile 是一个. xlsx 文件。为了读取这种文件，Gem [Roo](https://github.com/roo-rb/roo) 提供了打开它的有用方法。Gem [Roo](https://github.com/roo-rb/roo) 也支持打开其他类型的 excel 文件。csv，。xlsm..等等。

只需将 [Roo](https://github.com/roo-rb/roo) 添加到 Gemfile 并运行`bundle install`，然后我们就可以使用类`[Roo::Spreadsheet](https://www.rubydoc.info/gems/roo/2.7.1/Roo/Spreadsheet)`打开临时文件。

```
xlsx = Roo::Spreadsheet.open(tempfile)
```

之后。xlsx 文件是打开的，只需指明需要读取的工作表

```
sheet = xlsx.sheet(0)
```

然后可以使用`[Roo::Spreadsheet](https://www.rubydoc.info/gems/roo/2.7.1/Roo/Spreadsheet)`提供的方法访问该表中的所有行和单元格。所有方法的详细信息可以在 Gem [Roo](https://github.com/roo-rb/roo) Github 页面上找到。

# 参考

 [## 临时文件

### 用于管理临时文件的实用程序类。当您创建一个 Tempfile 对象时，它将创建一个带有…

ruby-doc.org](https://ruby-doc.org/stdlib-2.5.3/libdoc/tempfile/rdoc/Tempfile.html) [](https://www.rubyguides.com/2019/05/ruby-tempfile/) [## 如何在 Ruby 中创建临时文件

### 创建一个临时文件会给你一个空文件，在你的操作系统中有一个随机的名字

www.rubyguides.com](https://www.rubyguides.com/2019/05/ruby-tempfile/) [](https://github.com/fog/fog-aws) [## 雾/雾-自动气象站

### 在使用 fog-aws 之前，将这一行添加到您的应用程序的 Gemfile:中，然后执行:或者自己安装成:这样…

github.com](https://github.com/fog/fog-aws) [](https://github.com/roo-rb/roo) [## 鲁迈拉运营公司经常预算/鲁迈拉运营公司

### Roo 实现了对所有常见电子表格类型的读访问。它可以处理:Excel 2007 - 2013 格式(xlsx，xlsm)…

github.com](https://github.com/roo-rb/roo)  [## 斯特林乔

### 类别:StringIO - Ruby 2.5.1

ruby-doc.org](https://ruby-doc.org/stdlib-2.5.1/libdoc/stringio/rdoc/StringIO.html)  [## 模块 URI

### 从给定的枚举生成 URL 编码的表单数据。这会生成 HTML5 中定义的 application/x-www-form-urlencoded 数据…

docs.ruby-lang.org](https://docs.ruby-lang.org/en/2.1.0/URI.html)  [## 什么是亚马逊 S3？

### 亚马逊简单存储服务(亚马逊 S3)是互联网存储。它旨在使网络规模的计算…

docs.aws.amazon.com](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)