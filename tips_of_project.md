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

