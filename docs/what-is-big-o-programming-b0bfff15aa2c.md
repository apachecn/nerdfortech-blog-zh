# 什么是 Big-O 编程？

> 原文：<https://medium.com/nerd-for-tech/what-is-big-o-programming-b0bfff15aa2c?source=collection_archive---------11----------------------->

![](img/80726bdce353ac84b0a13a0946f91e71.png)

大 O 编程哇，听起来很可怕吧。好吧，我有好消息告诉你，大 O 其实很容易理解。简单来说 Big-O 只是一种谈论算法效率的方式。让我给你举一个大 O 符号的例子。

```
let number = 10
```

最重要的是。为什么是`O(1)`？让我再详细解释一下。`O = the number of operations in a given function.`因为我们上面的代码只运行一次等于 1 的操作。很简单对吧。好吧，让我们更进一步。

```
const myArray = ["a", "b", "c", "d"]const findCharacter = (char, array) => {
    for(let i = 0; i < array.length; i++){
        if(array[i] === char){
            return true
        }
    }
    return false
}
```

你认为`findCharacter`的大-O 会是什么？这个函数是做什么的？

```
const myArray = ["a", "b", "c", "d"]//we already know that this will be O(1) because it will only run once.//here we say. for each elemant in the array check to see if char exsits, if it does stop running and return true.
//if none of the characters return true then we'll finish the loop and return false.
const findCharacter = (char, array) => {
    for(let i = 0; i < array.length; i++){ 
// the number of operations is equal to the size of the array.
        if(array[i] === char){
            return true
        }
    }
    return false
}myArray.length
//=> 4
//if the size of the array is 4 and that means the total number of operations would depend on the size of our array.
```

所以由于`n`的运算次数取决于数组或数据的大小，我们将`O(n)`。其中`n`代表我们的数据。你可以说也是`O(4)`。因为数组有 4 个元素，所以操作数也是 4。让我们看另一个例子。

```
//compare to arrays
const array1 = ["a", "b", "c", "d",]
const array2 = ["x", "y", "z", "c"]
//we want to check if any of that character in array1 are also in array two.
//I'm justing going to code out the brute force solution first.const compareArrays = (arr1, arr2) => {
    for(let i = 0; i < arr1.length; i++){
        for(let j = 0; j < arr2.length; j++){
            if(arr1[i] === arr2[j]){
                return true
            }
        }
    }
    return false
}
```

这有点棘手。如果我们说 bot `array1`和`array2`将总是具有相同的大小，那么我们将得到`O(n^2)`。这样做的原因是因为我们的第一个数组`n`将与我们的第二个数组长度相同。让我再给你讲一遍。

```
const compareArrays = (arr1, arr2) => {
 for(let i = 0; i < arr1.length; i++){// the array is length is 4 
   for(let j = 0; j < arr2.length; j++){ //and for every item in array2 which is 4 run the code 4 times.
     if(arr1[i] === arr2[j]){ //4*4 16 and 4^2 is also 16 therfore we get O(n^2)
       return true
      }
    }
  }
 return false
}
```

很简单对吧。但是如果数组有不同的长度，那么 bigO 就是`O(n*b)`假设数组 1 的长度是 2，数组 1 的长度是 10。然后，如果我们插入这些值，操作的次数是`2*10`我们循环数组 1 两次，对于数组 1 中的每个元素，我们循环数组 2 十次。操作总数现在是 20，这就是为什么我们把它记为`O(n*b)`，其中`n`是数组 1 的数据长度，`b`是数组 2 的数据长度。

Big-O 可能看起来很可怕，但一旦你理解它是什么以及我们为什么使用它，它就真的很简单了。