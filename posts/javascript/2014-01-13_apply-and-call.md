##apply与call
---
apply和call是javascript中很有用的两个函数，这两个方法都可以实现`劫持一个对象的方法,使用另一个对象来替代当前对象`。个人理解是：`替换对象方法里的this对象`。

###语法
```
    fun.apply(thisArg[, argsArray])

    fun.call(thisArg[, arg1[, arg2[, ...]]])
```
    
从语法上看，二者第一个参数一致，都是需要`替代this的对象`，从第二个参数开始(后面的参数都是提供给调用方法的)有区别，apply方法的`第二个参数为一个参数对象数组`，call方法`从第二个参数开始为参数序列`。

注意：如果thisArg为null或undefined，则会被替代为全局对象。

<!--more-->

###用例
```js

function A(id , name){
    this.id = id;
    this.name = name;
}

function B(id , name){
    A.apply(this , arguments);
}
B.prototype.log = function(){
    alert('id:'+this.id+' name:'+this.name);
};
var b = new B(1 , 'rey');
b.log();

```

执行这段代码就会发现，b中的id和name已经被设置为了指定的值，我们在B中并没有定义设置id和name的代码段，但是却成功设置了指定属性值。这就是apply的作用，它调用了A方法，并且修改了其中的this对象，将当前对象指定为this，并执行相应逻辑代码。

如果把`A.apply(this , arguments);`更改为`A.call(this , id , name);`，我们发现可以达到相同效果。

###apply和call的选择

从上文可以看出，既然两个函数实现效果相同，那我们还需要区分使用场景么？答案是肯定的。两个方法最大区别在于调用的参数语法不同，重点就在这里。

具体选择那种方法，主要跟具体业务场景中提供的参数形式有关，如果可以方便以数组方式获取函数的参数，并且数组内参数顺序与调用方法的参数顺序是一致的（如用例中可以用arguments方便获取参数数组，并且id和name的顺序与A函数的参数顺序是一致的），那么就使用apply，如果参数需要重新组织，分别传入，或者获取的数组格式参数顺序与调用方法参数顺序不一致（如果用例中A定义为了function A(name,id)），那么就只能使用call了。

所以，可以看出，选择那种方法，是跟所能获取的`参数形式`息息相关的。

###apply的特殊用法

apply除了上面的作用外，它还有一种特殊的效果。

它可以`把参数数组转换成一个个参数组成的序列`，像上文中将arguments转换成id和name传递给A函数一样。

使用apply的这种特性，我们得好一些非常巧妙的用法。

数组中查找最大最小值：

```js

var arr = [3,6,0];
Math.max.apply(null,arr);
Math.min.apply(null,arr);

```

数组连接：
```js

var arr1 = [1,2,3],
	arr2 = [4,5,6];
Array.prototype.push.apply(arr1,arr2)

```

