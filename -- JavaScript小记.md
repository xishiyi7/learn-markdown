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
