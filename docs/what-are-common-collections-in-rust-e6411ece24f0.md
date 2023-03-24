# Rust 里有哪些常见的收藏？

> 原文：<https://medium.com/nerd-for-tech/what-are-common-collections-in-rust-e6411ece24f0?source=collection_archive---------6----------------------->

![](img/15fdc727fb1e4ee932ba51f03e8c9072.png)

# 常见收藏

Rust 的标准库包括许多非常有用的数据结构，称为集合。大多数其他数据类型表示一个特定值，但集合可能包含多个值。我们将讨论在 [Rust](https://www.technologiesinindustry4.com/) 程序中最常用的三个集合:
1。一个矢量
2。一串
3。哈希映射

## 1.向量:

允许我们相邻存储可变数量的值。

## 2.字符串:

是字符的集合。

## 3.哈希映射:

允许我们将一个值与一个特定的键相关联。这是更一般的数据结构(称为映射)的一个特殊实现。

# 存储值列表

我们将研究的是 Vec <t>，也被称为第一个集合的向量。</t>

## 向量

向量允许我们在一个数据结构中存储不止一个值，这个数据结构将所有的值一个接一个地放在内存中。[向量只能存储相同类型的值。当我们有一个项目列表时，它们很有用。](https://www.technologiesinindustry4.com/)

```
**Creating The Empty Vector:**
fn main() {
let v: Vec<i32> = Vec::new():
}
Creating The Vector Containing Values:
fn main() {
let v = vec![1,2,3];
println!("The First Value is = {},
Second Value is = {},Third Value is
= {} ",v[0], v[1], v[2]);
}**Updating the Vector**
fn main() {                          // Generating The Empty Vector and Pushing The Value
let mut v1 = Vec::new();
v1.push(5);
v1.push(6);
v1.push(7);
v1.push(8);
println!("Empty Vector v1 having no value afterValue pushing in Vector v1 {}, {} , {}, {}",
v1[0], v1[1], v1[2], v1[3]);
}fn main() {
// Creating Vector Having Some Value
let mut v = vec![1, 2, 3];
v.push(4);
v.push(5);
v.push(6);
v.push(7);
println!("The Vector having Value is {}, {}, {}After Value pushing The Vector v Value is {}, {}, {}, {} ",
v[0], v[1], v[2], v[3], v[4], v[5], v[6]);
}
```

## 读取向量的元素:

```
fn main() {
let v = vec![1, 2, 3, 4, 5];
let third: &i32 = &v[2];
println!("The third element is {}", third);
match v.get(2) {
Some(third) => println!("The third element is {}",
third),
None => println!("There is no third element."),
}
}
```

## 阅读元素(惊慌失措):

```
fn main() {
let v = vec![1, 2, 3, 4, 5];
let does_not_exist = &v[100];
let does_not_exist = v.get(100);
}
```

## 读取元素(错误):

```
fn main() {
let v = vec![1, 2, 3, 4, 5];
Let first = &v[0];
v.push(6);
println!(“The First Element is: {}, First);
}
```

## 迭代向量中的值:

```
fn
main() {
let v = vec![100, 32, 57];
for i in &v {
println!(“{}”,i);
}
}
```

## 迭代向量中的可变引用:

```
fn main() {
let v = vec![10, 20, 30];
for i in &mut v6 {
*i += 50;
println!(“{}”,i);
}
}
```

## 使用枚举存储多个

向量只能存储相同类型的值。这可能是可访问的类型；肯定有需要存储不同类型的项目列表的用例。[一个 enum 的可变变量是在同一个 enum 类型下定义的](https://www.technologiesinindustry4.com/)，所以当我们需要在一个 vector 中存储不同类型的元素时，我们可以定义并使用一个 enum！

## 用字符串存储 UTF-8 编码的文本

什么是字符串？

在 Rust 中，有两种类型的字符串:String 和&str。这是堆分配的，flourishale 的，不是空终止的。&str 是一个片段(&[u8])，它指向一个有效的 UTF-8 序列，可以用来查看一个字符串，就像&[T]是一个 Vec <t>的视图一样。</t>

核心语言中的 [Rust](https://www.technologiesinindustry4.com/) 只有一种字符串类型，就是通常在其借用形式& str 中看到的字符串切片 str。我们已经讨论了字符串片段，它们是对存储在其他地方的一些 UTF-8 编码的字符串数据的引用。例如，字符串文字存储在程序的二进制文件中，因此是字符串片。

## 创建字符串:

```
fn main() {
let s = String::new();
println!("Creating a Empty String: {}", s);
let data = "initial contents";
let s = data.to_string();
println!("The Value of s: {}", s);
{
// method do works on a literal directly:
let d = String::from(“initial contents);
println!("The Value of d: {}", d);
}
}
```

## 更新字符串:

```
#![allow(unused_variables)]
fn main() {
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
println!("s2 is {}", s2);
}
```

## 运算符串联:

```
fn main(){
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2;
println!("{}",s3);
} fn main (){
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = s1 + "-" + &s2 + "-" + &s3;
println!("{}",s);
}
```

## 与格式宏连接

```
fn main() {
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{}-{}-{}", s1, s2, s3);
}
```

## 编入字符串的索引:

```
fn main (){
let s1 = String::from("hello");
let h = s1[0];
}
This code will result error:
Rust strings don’t support indexing. [But why not?](https://www.technologiesinindustry4.com/) We need to discuss how Rust stores strings in memory to answer the question.
```

## 字符串字节存储:

```
fn main (){
let len = String::from("Hola").len();
println!("{}",len);
{
let len = String::from("Здравствуйте").len();
println!("{}",len);
}
}
```

## 字节、标量值和字形簇:

在《UTF-8》中，有三种方法可以从 Rust 的角度来看待弦乐:
1。按字节
2。按标量
3。按字形聚类

```
**SLICING STRING:**
fn main (){
let hello = "Здравствуйте";
let s = &hello[0..4];
println!("{}",s)
}**ITERATING OVER STRING:**
fn main (){
for c in "नमस्ते ".chars(){
println!("{}", c);}
{
for b in "नमस्ते ".bytes() {
println!("{}", b);}
}
}
```

## H A S H M A P

类型 K 的键到类型 V 的值的映射存储在类型 HashMap <k v="">中。[这一切都是通过哈希函数](https://www.technologiesinindustry4.com/)完成的，哈希函数决定了如何将这些键和值放入内存。当我们不想像使用向量那样使用索引而是使用任何类型的键来查找数据时，Hashmap 就很有用。</k>

## 创建新的哈希映射

```
use std::collections::HashMap;
fn main() {
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
for (key, value) in &scores {
println!("{}: {}", key, value);
}
println!("{:?}", scores);
}
```

## 创建新散列图的另一种方法

```
use std::collections::HashMap;
fn main() {
let teams = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];
let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();
for (key, value) in &scores {
println!("{}: {}", key, value);
}
println!("{:?}", scores);
}
```

## 哈希映射和所有权

```
use std::collections::HashMap;
fn main() {
let field_name = String::from("Favorite color");
let field_value = String::from("Blue");
let mut map = HashMap::new();
map.insert(field_name, field_value);
println!("{:?}", map);
}
```

## 访问哈希映射中的值

```
use std::collections::HashMap;
fn main(){
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
let team_name = String::from("Blue");
let score = scores.get(&team_name);
for (key, value) in &scores {
println!("{}: {}", key, value);
}
println!("{:?}", score);
}
```

## 更新哈希映射覆盖值:

```
use std::collections::HashMap;
fn main()
{
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
println!("{:?}", scores);
scores.insert(String::from("Blue"), 25);
println!("{:?}", scores);
}
```

## 更新哈希映射插入值:

```
use std::collections::HashMap;
fn main(){
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50);
println!("{:?}", scores);
}
```

## 更新哈希映射更新值:

```
use std::collections::HashMap;
fn main(){
let text = "hello world wonderful world";
let mut map = HashMap::new();
for word in text.split_whitespace() {
let count = map.entry(word).or_insert(0);
*count += 1;
}
println!("{:?}", map);
}
```

更多详情请访问:[https://www . technologiesinindustry 4 . com/2020/11/what-are-common-collections-in-rust . html](https://www.technologiesinindustry4.com/2020/11/what-are-common-collections-in-rust.html)