# js setter getter 学习

## 一、对象属性


　　对象属性由无序的键值对和一组特性构成。在 ES6,属性值可以用一个或两个方法替代，这两个方法就是 getter 和 setter。由于 getter 和 setter 定义的属性称做“存储属性”,它不同于“数据属性”，数据属性就是一个简单的值。

　　当程序查询存储器属性的值时，JS 调用 getter 方法(无参数)。这个方法的返回值就是属性存取表达式的值。

　　当程序设置一个存取属性的值时，JS 调用 setter 方法，将赋值表达式右侧的值当做参数传入 setter。从某种意义上讲，这个方法负责“设置”属性值。可以忽略 setter 方法的返回值。

　　和数据属性不同，存取器属性不具有可写性。如果属性同时具有 getter 和 setter, 那么它是一个读/写属性。如果它只有一个 getter 方法，那么它是一个只读属性。如果它只有 setter 方法，那么它是一个只写属性（数据属性中有一些例外），读取只写属性总是返回 undefined 

## 二、定义存取器属性

　　定义存取器属性的最简单的方法是使用对象直接量语法的一种扩展写法

	var o = {
		// 普通的数据属性
		data_prop: value,
		// 存取器都是定义为一个或两个和属性同名的函数
		get accessor_prop() { // ...code },
		set accessor_prop() { // ... code}
	};

　　存取器属性定义为一个或两个和属性同名的函数，这个函数定义没有使用 function 关键字，而是使用 get 和 set

　　注意，这里没有使用冒号将属性名和函数体分隔开，但在函数体的结束和下一个方法或数据属性之间有逗号分隔。

　　例如：

	var　p = {
		// x 和 y 是普通的可读写的数据属性
		x: 1.0,
		y: 1.0,
		// r 是可读写的存取器属性，它有 getter 和 setter 
		get r() {
			return Math.sqrt(this.x*this.x + this.y*this.y)
		},
		set r(newvalue) {
			var oldvalue = Math.sqrt(this.x*this.x+this.y*this.y);
			var ratio = newvalue/oldvalue;
			this.x* = ratio;
			this.y* = ratio;
		},
		// theta 是只读存取器属性，它只有 getter 方法
		get theta() { 
			return Math.atan2(this.y, this.x)
		}
	}

## 三、存取器属性是可以继承的
　　
　　和数据属性一样，存取器属性是可以继承的，在此可以将上述代码中的对象 P 当成一个“点”原型。可以给新对象定义它的 x 和 y 属性，但 r 和 theta 属性是可以继承的

	var q = inherit(p); 	//创建一个继承getter和setter的新对象
	q.x=1,q.y=1;			//给q添加两个属性
	console.log(q.r);		//可以使用继承的存取器属性
	console.log(q.theta);

## 四、

	//这个对象产生严格自增的序列号
	var serialnum = {
		//这个数据属性包含下一个序列号
		//$符号暗示这个属性是一个私有属性
		$n : 0 ,
		//返回当前值，然后自增
		get next(){ return this.$n++ },
		set next(n){
		if(n >= this.$n) this.$n = n;
			else throw "序列号的直不能比当前值小"
		}
	};
	//下面这个例子使用getter方法实现一种"神奇"的属性
	//这个对象有一个可以返回随机数的存取器属性
	//例如，表达式 "random.octet"产生一个随机数
	//每次产生的随机数都在0-255之间
	var random = {
		get octet(){ 
			return Math.floor(Math.random()*256); 
		},
		get uint16(){ 
			return Math.floor(Math.random()*65536); 
		},
		get init16(){ 
			return Math.floor(Math.random()*65536)-32768; 
		}
	};