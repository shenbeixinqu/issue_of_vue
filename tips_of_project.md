### vue2创建项目

```shell
1-vue init webpack app_demo #app_demo 项目名称
2-npm install --registry=https://registry.npm.taobao.org #安装项目依赖
```

### 引用element-ui

```shell
1-npm i element-ui -S #安装
2-在main.js中引用
	import ElementUI from 'element-ui'
	import 'element-ui/lib/theme-chalk/index.css'
	import 'element-ui/lib/theme-chalk/display.css'
	Vue.use(ElementUI)
```

### 使用mockjs

```shell
1-npm install mockjs --save-dev #安装
2-在入口js（main.js）里引入mockjs
```

### 安装axios

```shell
1-npm install axios --save
```

### css编译

```shell
1-npm install less less-loader --save-dev
	其中 less-loader版本不宜过高
	 npm install -D less-loader@5.x
```

### vue中使用better-scoll

```shell
BetterScroll是一款重点解决移动端各种滚动场景需求的开源插件，适用于滚动列表、选择器、轮播图、索引列表、开屏引导等应用场景。在移动端使用可以开发出更好更炫的滑动效果。
1-npm install  better-scroll --save  #安装
2-import BScroll from 'better-scroll' #引用
然后需要一个容器，需要注意的是，better-scroll容器只会使第一个子元素滚动，所以推荐使用下面这种结构：
<div ref="wrapper" class="wrapper">  //better-scroll容器
    <div>
         <!-- 要放的元素  -->
    </div>
</div>
接下来就是初始化better-scroll，这里有一个需要注意的地方，better-scroll初始化必须在dom元素加载完毕后才能初始化成功，所以一般都放在mounted这个钩子函数去执行，但有时候页面加载比较慢，还是会出现错误，所以下面推荐两种比较简单也是最常用的方法：
mounted() {
  setTimeout(() => {
    if(!this.$refs.wrapper)
      {
        return   //再加一次判断，防止dom没有加载出来而报错
      }
      this.scroll = new BScroll(this.$refs.wrapper,{})  
      // better-scroll的初始化接收两个参数，第一个是dom元素，第二个是配置项对象，可以不传
  },20)
}
第二种方法和上面类似，不使用setTimeout，使用vue中的$nextTick函数,在这个函数里初始化。

到这里初始化就完成了，但！很容易忽略的一个点，注意到我上面给容器加了一个class吗，如果不给这个容器设置高度，这个better-scroll是无效的，而且需要加上overflow:hidden属性,一般移动端的需求是整屏的高度或者整屏去掉头尾的高度，可以这样写:
.wrapper{
    overflow:hidden;
    position:fixed;
    top: xxxxxx   //看你具体需求
    bottom: xxxxxx  //同上
}
```

### promise

```shell
1-什么是promise
	在并发编程中,由于某些计算(或网络请求)尚未结束,我们需要一个对象来代理未知的结果,这个代理未知结果的对象就成为promise
```

### 保留小数点

```shell
toFixed() 方法可把 Number 四舍五入为指定小数位数的数字。
ex:
	<span>{{(item.value / 10000).toFixed(2)}}</span>万
```

### !important

```shell
在CSS中，通过对某一样式声明! important ，可以更改默认的CSS样式优先级规则，使该条样式属性声明具有最高优先级。
```

### 样式问题

```shell
1-box-sizing: border-box;
    如果将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。
2-display
inline:
	使元素变成行内元素，拥有行内元素的特性，即可以与其他行内元素共享一行，不会独占一行. 
	不能更改元素的height，width的值，大小由内容撑开. 
可以使用padding，margin的left和right产生边距效果，但是top和bottom就不行.
block:
	使元素变成块级元素，独占一行，在不设置自己的宽度的情况下，块级元素会默认填满父级元素的宽度. 
	能够改变元素的height，width的值. 
	可以设置padding，margin的各个属性值，top，left，bottom，right都能够产生边距效果.
 inline-block:
	结合了inline与block的一些特点，结合了上述inline的第1个特点和block的第2,3个特点.
	用通俗的话讲，就是不独占一行的块级元素
	
```

