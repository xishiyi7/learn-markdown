# Responsive Design (响应式布局)

###### 1.设置meta标签

`<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">`
> (user-scalable = no 属性能够解决 iPad 切换横屏之后触摸才能回到具体尺寸的问题)

###### 2.通过媒介查询来设置样式 Media Queries
> Media Queries 是响应式设计的核心，它根据条件告诉浏览器如何为指定视图宽度渲染页面。

` @media screen and (max-width:1000px){
    .test{
      color: red;
    }
}`

###### 3.设置多种视图宽度

> 如果要兼容ipad 和iphone的视图，可以如下设置：

` /* ipad */ `  
` @media only screen and (min-width: 768px) and (max-width: 1024px) {}`  
` /* iphone */ `  
` @media only screen and (min-width: 768px) and (max-width: 1024px) {}`

### **注意：**
1. 宽度需要使用百分比;  
...




