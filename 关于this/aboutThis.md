---
theme: juejin
---
### 一些不重要的废话

接上回日记，在我坚持不懈的努力创作下，终于将我的创作者中心中的部分数据变为了正数。

![img01.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4e6d39eee79445a4a9af44dd45b4eb01~tplv-k3u1fbpfcp-watermark.image)

喜大普奔

![7.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/44159dc86d2c45e4ad6ffc0e3bd16cc2~tplv-k3u1fbpfcp-watermark.image)

### 一些重要的废话

不过上次好像有点紧张了，不好意思，忘记了自我介绍。

![9.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/36ede32e12894817827282677477afa2~tplv-k3u1fbpfcp-watermark.image)

大家好，我是IT大军前端军营的**摸鲸校尉**。

### 正文开始

之前逛掘金的时候看到了[大帅老猿](https://juejin.cn/user/2955079655898093)大佬的[一篇文章](https://juejin.cn/post/6962949488646291486)。放上部分原文代码：
```javascript
function throttle(func, delay) {
    var last = 0;
    return function () {
        var now = Date.now();
        if (now >= delay + last) {
            func.apply(this, arguments);
            last = now;
        } else {
            console.log("距离上次调用的时间差不满足要求哦");
        }
    }
}
function resize(e) {
    console.log("窗口大小改变了");
}
window.addEventListener('resize', throttle(resize, 300));
```
有一代码是这样的 `func.apply(this, arguments);`。说实话，没有怎么看懂。于是我在我的高达1024kb储存量的大脑搜寻一下相关知识记忆。apply好像是可以修改this的指向，arguments好像是一个类数组的东西......呵呵呵呵呵，好像帮助并不大。

![11.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/df1e8166fc6648c29ecd86cb59f7a3ed~tplv-k3u1fbpfcp-watermark.image)

既然如此，那我们一步步来。

### 关于this

我依稀记得有个大佬给我说过：
> this只有在运行时才会明确其指向的对象

话不多说，try one try (试一试)。

```javascript
var name = 'a', age = 0, type = 'A'
function fn () {
    console.log( this.name,'年龄是 ', this.age, '血型是 ', this.type );
}
fn() //  expected output: a 年龄是  0 血型是  A
```
这里是在全局作用域下调用的fn，所以this就会指向全局作用域对象window。

```javascript
var name = 'a', age = 0, type = 'A'
function fn () {
    console.log( this.name,'年龄是 ', this.age, '血型是 ', this.type );
}
fn()
//  expected output: a 年龄是  0 血型是  A
var x = {
    name: 'XMAN',
    age: 24,
    type: this.type,
    fn: fn
}
var y = {
    name: 'YMAN',
    age: 25,
    type: 'B',
    fn: fn
}
  
x.fn() //  expected output: XMAN 年龄是  24 血型是  A
y.fn() //  expected output: YMAN 年龄是  25 血型是  B
```
这里其实都是调用的同一个方法，之所以console出来的东西不一样，就是因为this所指向的对象不一样。执行x.fn()时，调用环境是在x中，所以this指向对象x；同样的执行y.fn()时，this指向对象y。
当然this还有很多其他情况，大家可以自己研究。~~其实是我怕整多了讲不好会误导祖国的花朵~~

![8.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/74fe6a13ac164378b40ea154ce08d84e~tplv-k3u1fbpfcp-watermark.image)

### 关于apply、bind、call

先看MDN的描述

>apply() 方法调用一个具有给定this值的函数，以及以一个数组（或类数组对象）的形式提供的参数。

>bind() 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

>call() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。

这仨乍一看好像差不多,好像都能修改this的指向对象，但是区别肯定是有的。
try one try (试一试):
```javascript
  var name = 'a', age = 0, type = 'A'
  function fn () {
    console.log( this.name,'年龄是 ', this.age, '血型是 ', this.type );
  }
  var y = {
    name: 'YMAN',
    age: 25,
    type: 'B',
    fn: fn
  }
  fn() // expected output: a 年龄是  0 血型是  A
  fn.call(y) // expected output:  YMAN 年龄是  25 血型是  B
  fn.apply(y) // expected output:  YMAN 年龄是  25 血型是  B
  fn.bind(y)() // expected output:  YMAN 年龄是  25 血型是  B
```
可以看到bind方法多了一个( ),这个是因为bind方法会创建一个新的函数，所以要调用一下。

try again：
```javascript
  var name = 'a', age = 0, type = 'A'
  var x = {
    name: 'XMAN',
    age: 24,
    type: this.type
  }
  function todo (para1, para2) {
    console.log(this.name,'年龄是:', this.age, '血型是:', this.type, '爱好:', para1, para2)
  }
  todo('篮球', '足球')            //  a 年龄是: 0 血型是: A 爱好: 篮球 足球
  todo.call(x, '篮球', '足球')    //  XMAN 年龄是: 24 血型是: A 爱好: 篮球 足球
  todo.call(x, ['篮球', '足球'])  //  XMAN 年龄是: 24 血型是: A 爱好: ["篮球", "足球"] undefined
  todo.apply(x, '篮球', '足球')   //  报错，apply不能这样的，其第二个参数得是一个数组或者类数组对象
  todo.apply(x, ['篮球', '足球']) //  XMAN 年龄是: 24 血型是: A 爱好: 篮球 足球
  todo.bind(x, '篮球', '足球')()  //  XMAN 年龄是: 24 血型是: A 爱好: 篮球 足球
```
> call( )方法的作用和 apply( ) 方法类似，区别就是call( )方法接受的是参数列表，而apply( )方法接受的是一个参数数组。

### 关于arguments

同样先看MDN怎么说：

>arguments 是一个对应于传递给函数的参数的类数组对象。

还是举个栗子吧：

```javascript
  function testArguments() {
    console.log(arguments);
    // expected output: Arguments(3) ["a", "b", "c", callee: ƒ, Symbol(Symbol.iterator): ƒ]
    
    console.log('arguments.length :>> ', arguments.length);
    // expected output: arguments.length :>>  3
    
    console.log(arguments[0])
    // expected output: a
    
    arguments.push('d')
    // expected output: arguments.push is not a function
  }
  testArguments('a','b','c')
```

从上面可以看出，arguments就是传给函数的参数组成的类数组，并且有length，也可以通过索引去获取对应的数据，但是他不拥有像push这样的方法。其实是可以想到的，毕竟是别人Array的，怎么能给arguments玩呢。

### 总结

通过我们的不懈努力，终于找到了`func.apply(this, arguments);`这行代码的相关知识。所以，答案也呼之欲出了......

![12.jpg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4e7696da0b574782b0b76336dca16c5b~tplv-k3u1fbpfcp-watermark.image)

ちょっと待ってください (等一下)，好像还出不来，容我再想想......

![6.jpg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8bf1a36bb2744cbbbe34d6af7173e021~tplv-k3u1fbpfcp-watermark.image)

还是说说我自己的一些看法吧：他通过apply修改了this的指向，这里的this应该是指向window；当然我觉得更重要的是arguments，由于func是传入的，也就是说不是固定的函数，那么其参数也可能不是一定的，而arguments可以将个数不确定的参数传给func。这样一来，不过是什么函数，需要什么参数，就不需要关心了。

### 最后

第二次写文章，要是有啥没写正确的请大佬们指正。 ~~当然我也不一定会去改。~~




