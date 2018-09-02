### ES6中的Set

> Author：FrankLo  
> Date：2018-09-02

ES6新增了几种集合类型

> Set对象是值的集合，可以按照插入的顺序迭代集合中的元素。Set中的元素只会出现一次，即Set中的元素是唯一的

实例化方法
```javascript
const set = new Set([iterable])
```
其中`iterable`是一个可迭代对象，里面的所有元素都会添加到Set中。null被看作undefined，也可以不传入`iterable`，通过它的`add()`方法来添加元素。

其实ES6的Set和Java中的Set集合基本思想都是一样的。

#### 属性
- Set.prototype.size:返回Set实例的成员数量。
- Set.prototype.constructor:默认的构造Set函数。
  
#### 方法
- add(value): 添加某个值，返回Set结构本身。
- delete(value): 删除某个值，返回一个布尔值，表示删除成功。
- has(value): 返回一个布尔值，表示参数是否为Set的成员。
- clear(): 清除所有成员，没有返回值。
- keys(): 返回一个键名的遍历器
- values(): 返回一个值的遍历器
- entries(): 返回一个键值对的遍历器
- forEach(): 使用回调函数遍历每个成员

#### 栗子
js数组是没有remove这个方法的。当我们需要从一个数组里面移除一个特定的元素时，我们通常会怎么写？

在ES6之前，是这样的，写一个remove function
```javascript
function remove(array, element) {
    const index = array.indexOf(element);
    array.splice(index, 1);
}
```

然后我们可以这么用

```javascript
const arr = ["a", "e", "i", "o", "u", "x"];
arr; //["a", "e", "i", "o", "u", "x"]
// 移除其中的“x”
remove(arr, "x");
arr; // ["a", "e", "i", "o", "u"]
// 细心的同学会发现我们前面那么写的问题，如果我们再次移除“x”的话，会发生移除最后一个元素
remove(arr, "x");
arr; // ["a", "e", "i", "o"]
```

当数组查找不到某元素时会返回-1，数组的splice会从末尾往前，移除了最后一个元素，于是我们会这么写
```javascript
function remove(array, element) {
    const index = array.indexOf(element);

    if (index !== -1) {
        array.splice(index, 1);
    }
}
```

这样做每次我们总要去检测index的值，我们也可以用filter来写remove，这样则返回一个新的数组
```javascript
function remove(array, element) {
    return array.filter(e => e !== element);
}
```

有了Set之后，我们可以这么写
```javascript
const set = new Set(["a", "e", "i", "o", "u", "x"]);
set.delete("x"); // true
set; // Set {"a", "e", "i", "o", "u"}

set.delete("x"); // false
set; // Set {"a", "e", "i", "o", "u"}
```

#### One more
Set中的值是唯一的，那么我们可以使用Set来去重  

原来去重我们会这么写
```javascript
let arr = [1,'1', 2, 3, 2, 4, 5, 4, 1];
let arr_unique = arr.filter(function(item, index, array) {
return array.indexOf(item, index + 1) === -1;
});
arr_unique;//["1", 3, 2, 5, 4, 1]
```

或者利用对象key的唯一性，这么写

```javascript
let arr = [1,'1', 2, 3, 2, 4, 5, 4, 1];
let tmpObj = {};
let arr_unique = [];
arr.forEach(function(a) {
  let key = (typeof a) + a;
  if (!tmpObj[key]) {
    tmpObj[key] = true;
    arr_unique.push(a);
  }
});
arr_unique;//[1, "1", 2, 3, 4, 5]
```

于是现在还能这么写
```javascript
let arr = [1,'1', 2, 3, 2, 4, 5, 4, 1];
let set = new Set(arr);
let arr_unique = Array.from(set);//Array新增了一个静态方法Array.from，可以把类似数组的对象转换为数组
arr_unique;//[1, "1", 2, 3, 4, 5]
```
除了Array.from，我们也可以这么转化数组
```javascript
let set = new Set(['a','b','c']);
let arr = [...set];
arr;//['a','b','c']
```
而利用Array与Set的相互转化，还可以很容易地实现并集（Union）和交集（Intersect）
```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);
let union = new Set([...a, ...b]);
union;// [1, 2, 3, 4]
let intersect = new Set([...a].filter(x => b.has(x)));
intersect;// [2, 3]
```

#### 总结
与Array相比：
- Set中存储的元素是唯一的，而Array中可以存储重复的元素。
- Set中遍历元素的方式：Set通过for…of…，而Array通过for…in…。
- Set是集合，不能像数组用下标取值。








curl 'https://oapi.dingtalk.com/robot/send?access_token=b056c419576a7f1387e687309f02ffe27f4e07cd168a3f21bac7bf21f9f33b4e' \
   -H 'Content-Type: application/json' \
   -d '
  {"msgtype": "text",
    "text": {
        "content": "第一次提醒，写总结"
     }
  }'