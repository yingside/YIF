# javascript Module(模块)模式
## 从javascript对象创建说起
说起Module(模块)模式，可能一些刚刚接触javascript的同学还是觉得比较陌生，但是相信大多数学习javascript的同学都知道javascript对象创建的方式。最熟悉的莫过于构造器方式创建对象。

### Constructor(构造器)模式

#### 基本Construcor(构造器)
```javascript
function Car(model,year,miles){
	this.model = model;
    this.year = year;
    this.miles = miles;

    this.toShow = function(){
    	return this.model + "已经行驶了:" +this.miles + "公里!";
    }
}

//用法:

//可以创建car的新实例
var benz = new Car('Benz',2012,2000);
var bmw = new Car('bmw',2014,5000);

//打开浏览器控制台(console)，查看这些对象上调用toShow()方法的输出
console.log(benz.toShow());
console.log(bmw.toShow());
```

上面是一个最简单的构造器模式的版本，它存在一些问题，比如让继承变得困难。最关键的问题是，`toShow()`这样的函数是为每个使用Car构造器创建的新对象而分别重新定义的。最不是最理想的，因为这种函数应该在所有的Car类型实例之间共享。所以后又有了基于原型的构造器对象创建

#### 带原型的Constructor(构造器)
```javascript
function Car(model,year,miles){
	this.model = model;
    this.year = year;
    this.miles = miles;
}
//注意这里直接使用了Object.prototype.newMethod,而不是直接定义Object.prototype是为了避免重新定义prototype对象
/*
Car.prototype = {
	constructor: Car,//这样写需要重新定位constructor
    toShow:function(){
    	return this.model + "已经行驶了:" +this.miles + "公里!";
    }
}
*/
Car.prototype.toShow = function(){
	return this.model + "已经行驶了:" +this.miles + "公里!";
}

//用法:

var benz = new Car('Benz',2012,2000);
var bmw = new Car('bmw',2014,5000);

console.log(benz.toShow());
console.log(bmw.toShow());

```
现在toShow()的单一实例就能在所有Car对象之间共享了。

基本的关于构造器创建对象的模式说完了，但是却引出一个问题，这里提出了模式一词，**模式到底是什么**？

## 什么是模式

**模式很容易被复用**
就像上面的构造器模式,它是我们创建对象的一种模式,只要掌握了这种模式,它就是一种立即可用的解决方案。**构造器模式**给我们提出了快速大量简单的创建javascript对象的办法。

**模式是已经验证的解决方法**
解决方案其实就是开发人员的经验和见解,他们为定义和改进这些方法提供了帮助。从而形成了模式。比如构造器模式，从上面的列子就可以看出，为了更好的实现对象的创建,构造器模式在慢慢的改进,让其达到最能适合创建javascript对象的形式

**模式富有表达力**
看到模式时,通常就表示有一个设置好的结构和表达解决方案的词汇。很简单,做项目时,只要你说通过原型的构造器模式创建对象。那么大家都知道你说的是什么意思。或者你说这个问题可以用观察者模式去解决。只要大家都熟悉观察者模式，那么在程序员之间就少了很多沟通的障碍。

**所以模式也就是一种可以复用的解决方案而已**，用来解决软件设计中遇到的常见问题。


- - -
那么现在问题来了，上面的构造器模式主要是用来解决对象创建的问题。那么现在web工程中，javascript所占的比重越来越多。越来越重要。相应的，前台所写的代码也越来越多。代码约多，就越不好管理，一个项目中多人开发，如果出现同名的变量怎么办?这就需要我们的**模块模式**

## Module(模块)模式

模块是任何强大应用程序架构中不可或缺的一部分，它通常能够帮我们清晰的分离和组织项目中的代码单元。简单的比喻的话，javascript中模块就类似于C#中的命名空空间，java中的包。

在javasctipt中,有几种用于实现模块的方法，包括

- **对象字面量表示法**

- **Module模式**

- **AMD模块**

- **CommonJS模块**

- **ECMAScript Harmony模块**

### 对象字面量

对象字面量的表示法相信大家都不会陌生，一个对象被描述为一组包含在大括号(　**{ }**　)中，以逗号(,)分隔的name/value对，对象内的名称可以是字符串或者标识符，后面跟着一个冒号。对象中最后一个name/value对的后面不能加逗号，不然会导致出错
```javascript
var myObjectLiteral = {
	variableKey:variableValue,
    functionKey:function(){
    	//...
    }
}
```
对象字面量不需要使用**new**运算符进行实例化。

下面来看一个比较完整的例子:

```javascript
var myModule = {
	myProperty:"someValue",

    //对象字面量可以包含属性和方法
    //例如：可以声明模块的配置对象
    //注意：无论是对象还是方法写完，如果不是最后一个记得加上(,)
    myConfig:{
    	useCaching:true,
        language:'en'
    },

    //基本方法
    myMethod:function(){
		console.log('输出基本信息');
    },

    //根据配置输出信息
    myMethod2:function(){
    	console.log('缓存是：' + (this.myConfig.useCaching)?'可用的':'不可用');
    },

    //重写配置
    myMethod3 = function(newConfig){
    	if(typeof newConfig === 'object'){
        	this.myConfig = newConfig;
            console.log(this.myConfig.language);
        }
    }
};

//调用:

//输出基本方法
myModule.myMethod();

//输出:可用的
myModule.myMethod2();

//输出：fr
myModule.myMethod3({
	language:'fr',
    useCaching:false
});
```

上面就是一个比较典型的对象字面量的表示形式，很明显的看到比起构造器封装函数简单实用。就创建一个对象。因此对象字面量的最佳实践就是有助于封装和组织代码。但是对于一个封装一个模块来说，对象字面量的作用又不够，因为**单独使用它不好区分私有/公有方法和变量**。

### Module(模块)模式
在javascript中，Module模式用于进一步模拟类的概念，通过这种方式，可以使一个单独的对象拥有公有/私有方法和变量，从而屏蔽来自全局作用域的特殊部分。产生的结果是:**函数名与在页面上其他脚本定义的函数冲突的可能性降低**。

#### 私有
Module模式使用闭包封装"私有"状态和组织。它提供了一种包装混合公有/私有方法和变量的方式，防止其泄露至全局作用域，并与别的开发人员的接口发生冲突。**通过该模式，只需返回一个公有API，而其他的一切都维持在私有闭包里面。**

这样做为我们**提供了一个屏蔽处理底层事件逻辑的整洁解决方案，同时只暴露一个接口供应用程序的其他部分使用**。该模式除了返回一个对象而不是一个函数之外，非常类似于一个立即调用的函数表达式(IIFE)。

讲了一堆概念，先直接来看一下例子：
```javascript
var testModule = (function(){
	//counter在IIFE函数体内部，外部不能直接访问
	var counter = 0;

    //直接返回了匿名对象,所以外部需要通过前缀testModule才能访问
    return {
    	incrementCounter:function(){
        	//由于变量作用域的原因，这里能够访问上一级的变量counter
            //实际counter在这里就形成了常说的闭包,
            //变相延长了counter的作用域
        	return ++ counter;
        },

        resetCounter:function(){
        	console.log('counter的值将被清空,现在的值是:' + counter);
            counter = 0;
        }
    };

})();

//用法：

//增加
testModule.incrementCounter();

//重置
testModule.resetCounter();
```

需要指出的是，**在javascript中没有真正意义上的"私有"**，因为它不像`java`，`C#`等传统面向对象的语言，javascript没有像`private`这些的**访问修饰符**。从技术上来说，我们不能称变量为公有或者私有。因此在javascript利用函数作用域来模拟这个效果。**在Module模式内，由于闭包的存在，声明的变量和方法只有在该模式内部可用。但是返回对象上定义的变量和方法，则对外部使用者是可用的**。

在上面的代码中，`counter`变量实际上是完全与全局作用域隔离的，因此它表现得就像是一个私有变量，它的存在被局限于模块的闭包内，因此唯一能够访问它的只有`incrementCounter()`和`resetCounter()`这两个函数。而且在外部，这两个函数也是不能直接访问的。必须加上前缀`testModule`。达到了模拟命名空间的效果。

我们来看一个用这种模式实现的购物车的例子:
```javascript
var basketModule = (function(){
	//购物车数组,私有变量
	var basket = [];

    //私有函数
    function doSomethingPrivate(){
    	//。。。
    }

    //私有函数
    function doSomethingElsePrivate(){
    	//。。。
    }

    //返回一个暴露出的公有对象
    return {
    	//添加item到购物车
        addItems:function(values){
        	basket.push(values);
        },

        //获得购物车里商品总数
        getItemCount:function(){
        	return basket.length;
        },

        //私有函数的公有形式别名
        doSomething:doSomethingPrivate,

        //获取所有商品的总价格
        getTotal:function(){
        	var itemCount = this.getItemCount(),
            	total = 0;
            while(itemCount--){
            	total += basket[itemCount].price;
            }
            return total;
        }
    };
})();

//用法:
basketModule.addItem({
	item:"面包",
    price:5
});
basketModule.addItem({
	item:"可乐",
    price:2
});

//2
console.log(basketModule.getItemCount());
//7
console.log(basketModule.getTotal());
```
