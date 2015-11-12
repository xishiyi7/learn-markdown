# JavaScript 中遇到的点滴

###### 1. 原型 原型对象 prototype

> blog/site  
      a. [通过JQuery学习JS](http://www.cnblogs.com/baochuan/archive/2012/11/22/2782343.html)

> javascript为所有***函数***绑定一个prototype属性，由这个属性指向一个原型对象***fn.prototype***，我们**在原型对象中定义类的继承属性和方法**等，eg：

```
  var A = function(){};
  A.prototype.name = 'John';
  A.prototype.getName = function(){
    return this.name;
  };  
  var a = new A();
  console.log(a.name);  // 'John'
```

> 通过工厂方法创建一个实例

```
  var A = function(){return A.prototype.init()};
  A.prototype.init = function(){return this;};
  A.prototype.name = 'John';
  var a = A();  // a.name:'John';
  console.log(A().name);  // 'John'
```

` 如果直接在构造函数中 return this，拿到的其实是函数所在作用域全局对象，例如，window对象`

###### 2. 回调 callBack

```
function Aaron(List, callback) {
    setTimeout(function() {
      var task = List.shift();
debugger                                        // 在chrome的console中 debugger也能进调试
      task();                                   // 执行函数
      if (List.length > 0) {                    // 递归分解
        setTimeout(arguments.callee, 1000);     // 调用arguments所在作用域函数自身 此处递归调用           
      } else {
        callback();
      }
    }, 25)
  }

  Aaron([function(){
    console.log('a')
  },function(){
    console.log('b')
  }],function(){
    console.log('callback')
  })
  
/* **以上例子只是提醒自己可以在chrome console中调试、callee可以用于递归调用等** */
```

###### 3. 设计模式

 ` 工厂模式`
 ```
      function createBlog(name, url) {
        var o = new Object();
        o.name = name;
        o.url = url;
        o.sayUrl= function() {
          alert(this.url);
        }
        return o;
      }
       
      var blog = createBlog('xishiyi7', 'http://github.com');
      console.log(blog instanceof Object);  // true 工厂模式 只能识别 Object
 ```
 
 ` 构造函数模式`
 ```
      function Blog(name, url) {
        this.name = name;
        this.url = url;
        this.alertUrl = function() {
          alert(this.url);
        }
      }
       
      var blog = new Blog('xishiyi7', 'http://github.com');
      console.log(blog instanceof Blog);  // true 构造函数模式 能识别 Blog, 因此可以构造Array、Date等类型对象
 ```
 
 ` 构造函数模式A 与工厂模式B 的区别：`
 ```
      构造函数模式A 没有显示的创建对象
      构造函数模式A 没有return语句
      构造函数模式A 使用new创建对象
      构造函数模式A 可以识别对象     // 用instanceof 可以识别
 ```
 
###### 4. 内存泄漏

` 内存泄漏的几种情况包括：循环引用、js闭包、DOM插入顺序等`

###### 5. 事件

` 通过委托添加事件，除了能提升性能，还可以很友好的支持动态绑定`

###### 6. 类型

` !!, 通过!!, 0/null/undefined/false等值都会变为false, 而非这些值的变量都会变为true, 方便判断`
```
      var a,b=0,c=null,d=false;
      var aa = !!a, bb = !b, cc = !c, dd = !d;      // 此时 aa===bb===cc===dd===false
      
      var x=1,y='A',z=true,w={};
      var xx = !!x, yy = !!y, zz = !!z, ww = !!w;   // 此时 xx===yy===zz===ww===true        
      
```

