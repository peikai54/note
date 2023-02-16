## grid 的使用

网格布局在 web 网站里的应用并不广泛，因为 flex 已经足以实现大部分布局功能。我在工作中也暂未实际应用到网格布局，这里只简单总结使用 grip 实现网站常见的双栏布局，三栏布局的方式。

[grid 布局基础知识点](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)。

### grid 实现双栏布局

grid 实现双栏布局可以直接把网页看作一个 2 \* 1 的网格。

```html
<div class="father">
  <div class="left"></div>
  <div class="right"></div>
</div>

<style>
  .father {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-template-rows: 600px;
  }

  .father > .left {
    background-color: aqua;
    height: 800px;
  }

  .father > .right {
    background-color: azure;
  }
</style>
```

### grid 实现三栏布局

grid 实现三栏布局也很简单，直接把页面切割成一个 3 \* 1 的网格就可以了

```html
<div class="father">
  <div class="left"></div>
  <div class="container"></div>
  <div class="right"></div>
</div>

<style>
  .father {
    display: grid;
    grid-template-columns: 200px 1fr 300px;
    grid-template-rows: 100vh;
  }

  .father > .left {
    background-color: aqua;
  }

  .father > .right {
    background-color: azure;
  }

  .father > .container {
    background-color: aquamarine;
  }
</style>
```
