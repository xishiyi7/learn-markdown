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

###### 回调 callBack

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
