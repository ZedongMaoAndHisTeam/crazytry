### ES6 Set
ES6新增了几种集合类型

> Set对象是值的集合，可以按照插入的顺序迭代集合中的元素。Set中的元素只会出现一次，即Set中的元素是唯一的

实例化方法
```javascript
const set = new Set([iterable])
```
其中`iterable`是一个可迭代对象，里面的所有元素都会添加到Set中。null被看作undefined，也可以不传入`iterable`，通过它的`add()`方法来添加元素。

其实ES6的Set和Java中的Set集合基本思想都是一样的。

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

在ES6之前，是这样的，写一个remove function
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

当数组查找不到某元素时会返回-1，则数组的splice会从末尾往前，移除了最后一个元素，于是我们会这么写
```javascript
function remove(array, element) {
    const index = array.indexOf(element);

    if (index !== -1) {
        array.splice(index, 1);
    }
}
```






















curl 'https://oapi.dingtalk.com/robot/send?access_token=b056c419576a7f1387e687309f02ffe27f4e07cd168a3f21bac7bf21f9f33b4e' \
   -H 'Content-Type: application/json' \
   -d '
  {"msgtype": "text",
    "text": {
        "content": "Presentation写完了吗"
     }
  }'