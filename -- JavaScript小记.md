# JavaScript 中遇到的点滴

> javascript为所有函数绑定一个prototype属性，由这个属性指向一个原型对象，我们**在原型对象中定义类的继承属性和方法**等，eg：

` var A = function(){};`  

` A.prototype.name = 'John';
  A.prototype.getName = function(){return this.name;};`

` var a = new A();`

` console.log(a.name);// 'John'`
