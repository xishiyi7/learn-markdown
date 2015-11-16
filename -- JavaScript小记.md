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

###### 7. 跨域

**` 支持XMLHttpRequest2的现代浏览器, 服务端设置Access-Control-Allow-Origin返回头即可`**  
**` 使用HTML5中新引进的window.postMessage方法来跨域传送数据`**  
**` 使用jsonp实现跨域(需要评估安全性)`**  
**` 使用window.name`**  
**` 使用document.domain`**

` 几种常见js跨域解决方案`[跨域](http://www.cnblogs.com/2050/p/3191744.html)

` jsonp`
> 参考文章：1. [jsonp](http://www.cnblogs.com/duanhuajian/p/3152617.html)

```
      主要原理，在html中，拥有src属性的标签可以实现，例如script、img、iframe等，又由于json格式的通用性，可以实现跨域。如下:  
      a. 在远程有一个demo.js，代码如下
            alert('demo');
      本地的html中如果引入了这段js，即有  
            <script src="http://xxx.com/demo.js"></script>
      那当页面解析到这句话，就会执行这段代码，弹出demo;
      b. 如果想执行的是一个函数，只要在引用这段js之前先定义一个函数，然后在远程js中执行即可，即：
            <script>
                  var demoFun = function(arg){alert('demoArg='+arg)};
            </script>
            <script src="http://xxx.com/demo.js"></script>
      远程的js中调用代码如下：
            demoFun('b')
      那当解析到远程js文件时，也会执行函数demoFun，弹出demob；
      c. 如果想执行一个动态函数，只要给服务器传递一个函数名称供识别就行：
            <script>
                  var demoFun = function(arg){alert('demoArg='+arg)};
                  var ur = "http://xxx.com/yyy.aspx?callback=demoFun";
                  // 创建script标签，设置其属性
                  var script = document.createElement('script');
                  script.setAttribute('src',url);
                  // 把script标签加入head，此时调用开始
                  document.getElementsByTagName('head')[0].appendChild(script); 
            </script>
      这样，只要服务器在访问aspx页面且参数为demoFun时，返回需要调用的函数即可，此时可以返回：
            demoFun('这里可以是任何json对象或者字符串')
            
      以上分别是三种不一样场景使用jsonp进行跨域的例子

```

###### 8. HTML5本地存储功能
> localStorage: 同一个域下面，多个窗口可以共享给数据  
> sessionStorage: 只有当前窗口保留数据  
> [localStorage与sessionStorage简单用法](http://www.cnblogs.com/xiaowei0705/archive/2011/04/19/2021372.html)  
> [html5本地存储学习一](http://blog.csdn.net/fyq891014/article/details/7625805)
