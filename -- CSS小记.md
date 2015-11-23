# CSS 随记

> 连续字母断行：
```
white-space:pre-wrap || word-wrap: break-word ||word-break: break-all;
```

> 思维：实现 1.左边一个容器，里面放很多内容 2. 中间一个分割线 3.右边放个图标
```
思路一：
  <div class="row">
    <div class="col-80">
      left
    </div>
    <div class="col-20 row">
      <div class="col">
        sline
      </div>
      <div class="col">
        icon
      </div>
    </div>
  </div>  
```
