1. 考察this三板斧
1.1
function show () {
	console.log('this:', this);
}

var obj = {
	show: show
};

obj.show();   //show

function show () {
	console.log('this:', this);
}

var obj = {
	show: function () {
		show(); 
	}
};

obj.show();   //window

1.2
var obj = {
	show: function () {
		console.log('this:', this);
	}	
};
(0, obj.show)();//window

1.3
var obj = {
	sub: {
		show: function () {
			console.log('this:', this);
		}
	}
};

1.4
var obj = {
	show: function () {
		console.log('this:', this);
	}
};
var newobj = new obj.show();

1.5
var obj = {
	show: function () {
		console.log('this:', this);
	}
};
var newobj = new (obj.show.bind(obj))();

1.6
var obj = {
	show: function () {
		console.log('this:', this);
	}
};
var newobj = new (obj.show.bind(obj))();

1.7
var obj = {
	show: function () {
		console.log('this:', this);
	}
};
var elem = document.getElementById('book-search-results');
elem.addEventListener('click', obj.show);
elem.addEventListener('click', obj.show.bind(obj));
elem.addEventListener('click', function () {
	obj.show();
});

2. 作用域
2.1
var person = 1;
function showPerson() {
	var person = 2;
	console.log(person); 
}
showPerson();  //2

2.2
var person = 1;
function showPerson() {
	console.log(person);
	var person = 2;
}
showPerson(); //undefind 只是声明了person  还没有赋值

2.3
var person = 1;
function showPerson() {
	console.log(person);
	var person = 2;
	function person() {}
}
showPerson(); //person() {}  变量提升   函数优先于变量  变量重新声明不会影响之前的赋值

2.4
var person = 1;
function showPerson() {
	console.log(person);
	function person() {}
	var person = 2;
}
showPerson();  //person() {}  变量提升   函数优先于变量  变量重新声明不会影响之前的赋值

2.5
for(var i = 0; i < 10; i++) {
	console.log(i); // 0，1，2，3，4，5，6，7，8，9
}
for(var i = 0; i < 10; i++) {
	setTimeout(function(){
		console.log(i);  //10个10  
	}, 0);
}
for(var i = 0; i < 10; i++) {
	(function(i){
		setTimeout(function(){
			console.log(i);// 0，1，2，3，4，5，6，7，8，9  闭包每次传了指定的i值
		}, 0)
	})(i);
}
for(let i = 0; i < 10; i++) {
	console.log(i);// 0，1，2，3，4，5，6，7，8，9
}


2.向对象
1.1 
function Person() { 
	this.name = 1; 
	return {}; 
}
var person = new Person(); 
console.log('name:', person.name); //返回undefind Person里return的是空对象 里面没有name属性

1.2 
function Person() {
	this.name = 1;
} 
Person.prototype = {
	show: function () {
		console.log('name is:', this.name); 
		} 
};
var person = new Person(); 
person.show();                //返回1 new关键字创建的新对象继承了父对象的那么属性

1.3 
function Person() {
	this.name = 1;
}
Person.prototype = { 
	name: 2, 
	show: function () {
		console.log('name is:', this.name); 
	}
};
var person = new Person(); 
Person.prototype.show = function () {
	console.log('new show'); 
};
person.show();               //返回'new show' 父类上的show方法被修改

1.4 
function Person() {
	this.name = 1;
}
Person.prototype = {
	name: 2,
	 show: function () {
		 console.log('name is:', this.name);
	}
}; 
var person = new Person();
var person2 = new Person();
person.show = function () {
	console.log('new show'); 
};
person2.show();             //返回name is:1 this.name大于prototype的优先级？？？
person.show();              //返回'new show' person的show方法被修改

2、综合题 
function Person() {
	this.name = 1; 
}
Person.prototype = { 
	name: 2,
	show: function () {
		console.log('name is:', this.name); 
	}
};
Person.prototype.show();	//返回2 调用了Person.prototype的show方法
(new Person()).show();	    //返回1 同this.name大于prototype的优先级？？？
