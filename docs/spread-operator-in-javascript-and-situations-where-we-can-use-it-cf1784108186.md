# JavaScript 中的 Spread …运算符以及我们可以使用它的情况。

> 原文：<https://medium.com/nerd-for-tech/spread-operator-in-javascript-and-situations-where-we-can-use-it-cf1784108186?source=collection_archive---------8----------------------->

![](img/1ab5cd895c3c24139d390fd32b41a4e5.png)

当 iterables 或数组中的所有元素都需要包含在某种列表中时，可以使用 Spread 语法。我们可以使用 spread 运算符来扩展数组并添加新元素。

*当我们需要书写用逗号分隔的多个值时，使用扩展运算符。*

***Spread 运算符不仅适用于数组，也适用于所有的可迭代对象。***

在构建数组或者向函数传递值时，我们可以使用 spread 运算符。

(Iterables:数组、字符串、集合、映射，但不是对象)

*所以基本上一次解包所有的数组元素。*

```
for ex: we have an array const arr = [7, 8, 9];
// now we want to create an new array based on this array. 
// how we will do it without spread ... operator 👉const oldMethodArr = [1, 2 , arr[0], arr[1], arr[2]];console.log(oldMethodArr);
Output: [1, 2, 7, 8, 9];// BUT WITH SPREAD OPERATOR WE CAN DO IT WITH VERY EASY AND HANDY WAY 👉const newMethodArr = [1, 2, ...arr];
console.log(newMethodArr);
Output: [1, 2, 7, 8, 9];that's it..💯note: BUT if remove ... spread operator before the array it will add new array inside that array👉const newMethodArr = [1, 2, arr];output: [1,2, Array]
```

所以我们可以看到这是一个非常简单方便的方法..它节省了大量的时间和混乱的代码。

## 扩展运算符用于许多其他情况，例如👉

*   *当我们想在函数中传递多个参数或元素时。*

```
for ex: const numbers = function(...numbers){
     console.log(1, 2, 3)}numbers([1,2,3]);
output: 1 2 3
```

*我们可以看到，这省去了逐个输入每个参数的麻烦。*

*现在你可能已经注意到 spread 操作符类似于析构，因为它也帮助我们从数组中取出元素。最大的区别是 spread… operator 从数组中取出所有元素，并不把它们赋给变量。*

*   *spread…运算符用于创建数组的浅层副本。*

```
const arr = [1, 2 ,3 ];
const newArr = [...arr];// it's similar to Object.assign()
```

*   *加入两个数组*

```
const numbersArr = [1, 2, 3];
const alphabetsArr = [a, b, c];
const bothArrays = [...numbersArr, ...alphabetsArr]
console.log(bithArrays)
output: [1,2,3,a,b,c]
```

*   因为字符串也是可迭代的……对所有可迭代的都有效。

```
const str = ‘javascript’;
const letters = [...str];
console.log(letters);
output: ['j', 'a' , 'v', 'a', 's', 'c', 'r', 'i', 'p', 't']; 
```

*   最后但同样重要的是，spread 运算符也适用于对象，即使对象不是可迭代的。

```
const mobileSpec = {
   brand: 'apple',
   model: '12 pro max',
   price: 1099,}
// so if we want to add properties of this object to new object and add some new properties we can use spread operator.const moreSpec = {color: 'graphite', ...mobilespec, ram: '4gb'};console.log(moreSpec);
output: {color: 'graphite', 
         brand: 'apple',
         model: '12 pro max',
         price: 1099,
         ram: '4gb'
         }
```

*   *创建物体的浅层拷贝。*

```
const mobileSpec = {
   brand: 'apple',
   model: '12 pro max',
   price: 1099,};
const copyMobileSpec = {...mobileSpec};
console.log(copyMobileSpec);output: {
   brand: 'apple',
   model: '12 pro max',
   price: 1099,};
```

因此，正如我们到目前为止所看到的，当我们想要获得每个元素并以逗号分隔它们时，spread 操作符使工作变得非常容易。我们几乎已经涵盖了我们可以使用它的情况。