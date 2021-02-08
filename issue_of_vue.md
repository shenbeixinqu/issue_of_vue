### issue_of_vue

#### 生命周期

```shell
beforeCreate ：初始化了部分参数，如果有相同的参数，做了参数合并，执行 		 beforeCreate ；
created ：初始化了 Inject 、Provide 、 props 、methods 、data 、		   computed 和 watch，执行 created ；
beforeMount ：检查是否存在 el 属性，存在的话进行渲染 dom 操作，执行 	    beforeMount ；
mounted ：实例化 Watcher ，渲染 dom，执行 mounted ；
beforeUpdate ：在渲染 dom 后，执行了 mounted 钩子后，在数据更新的时		   候，执行 beforeUpdate ；
updated ：检查当前的 watcher 列表中，是否存在当前要更新数据的 watcher 	   ，如果存在就执行 updated ；
beforeDestroy ：检查是否已经被卸载，如果已经被卸载，就直接 return 出	    去，否则执行 beforeDestroy ；
destroyed ：把所有有关自己痕迹的地方，都给删除掉；

```

#### computed

```shell
# 定义
### 当依赖的属性的值发生变化时,计算属性会重新计算,反之,则使用缓存中的属性值
# 实例
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

#### watch

```shell
# 定义 
### watch监听器主要用来监听变量的变化
# 实例
## 对普通变量的监听
	比如对data中的count变量进行监听,两个参数,一个是新值,一个是旧值
	export default{
		name: "Test",
		data(){
			return {
				count: 0,
			}
		},
		watch:{
			count(newval,oldval){
				// do something
			}
		}
	} 
## 对对象的监听
	如果我们监听的是一个对象,会发现执行this.queryList.name = 'jack',是无法触发watch监听的,
	这时候我们就需要用到deep属性进行深度监听,监听器会一层层的向下遍历,给对象的所有属性都加上这个监听器,但是这样的开销就会非常大,任何修改queryList里面的任何一个属性都会触发这个监听器里的handler
	export default {
		name: "Test",
		data(){
			return: {
				queryList:{
					count: 0,
					name: "",
				}
			}
		},
		watch:{
			queryList:{
				handler(newval,oldval){
					// do something
				}
			}
		}
	}
	但是有时候只想监听queryList.name的变化,对于queryList.count不需要监听,那么可以采用下面这种写法,对这个对象里面的特定属性进行属性监听
	export default {
		name: "Test",
		data(){
			return: {
				queryList: {
					count: 0,
					name: ""
				}
			}
		},
		watch: {
			'queryList.name':{
				handler(newval,oldval){
					// do something
				}
			}
		}
	}
```

#### computed和methods的区别

```shell
# 前倾提要 computed(reversedMsg)  methods(reversedMsg1)
computed是基于响应性依赖进行缓存的,只有在响应式依赖发生改变时它们才会重新求值,也就是说,当msg值没有发生改变时,reversedMsg计算属性会立即返回之前缓存的计算结果,而不会再执行computed中的函数,但是methods方法中是每次调用,都会执行函数的,methods不是响应式的
```

#### computed和watch的区别

```shell
相同点: 他们两个都是观察页面数据变化的
不同点: computed只有当依赖的数据变化时才会计算,当数据没有变化时,他会读取缓存数据
	   watch每次都会执行函数,watch更适合于数据变化时的异步操作			
```

#### 复用元素渲染问题

```shell
#引出: 当实现点击按钮切换input表单时,我们输入上的value,点击按钮切换表单时发现value值还存在,但是input元素切换了
<span v-if="isUser">
      <label for="user">用户名</label>
      <input type="text" placeholder="用户名" id="user" key="user">
</span>
<span v-else>
      <label for="email">邮箱</label>
      <input type="text" placeholder="邮箱" id="email" key="email">
</span>
<button @click="isUser=!isUser">切换类型</button>
<script> 
const app = new Vue({
  el: '#app',
  data: {
     isUser: true
  }
})
</script>
# 原因 
	1.vue在进行DOM渲染时,出于对性能的考虑,会尽可能复用已经存在的元素,而不是创建新的元素
	2.上面的案例,vue内部会对比发现两部分都相似只会替换属性,不会创建全新的元素
	3.上面的if的input不再使用,直接作为else的input使用
```

#### v-if与v-show

```shell
v-if 当条件为false时,不会有对应的元素存在DOM中
v-show当条件为false时,是将元素的display属性设置为none
在显示和隐藏之间切换很频繁时,使用v-show
当只有一次切换时,通过使用v-if
```

#### 子传父

```shell
# 子组件
<template>
	<button @click="sendNum">给父组件传值</button>
</template>
<script>
	export default {
		data(){
			return{
				num:3
			}
		},
		methods:{
			sendNum(){
				this.$emit("myEven", this.num)
			}
		}
	}
</script>

# 父组件
<template>
	<test @myEven="getNum"></test>
</template>
<script>
	export default {
		methods:{
			getNum(num){
				console.log(num)
			}
		}
	}
</script>
```

#### 兄弟组件传值(uniapp)

```shell
# 前提 a组件修改b组件的值

## a组件
<template>
	<view>
		<button @click="addNum"></button>
	</view>
</template>
<script>
	export default{
		methods:{
			addNum(){
				uni.$emit("updateNum",10)
			}
		}
	}
</script>
## b组件
<template>
</template>
<script>
	export default{
		data(){
			return{
				num:10
			}
		},
		created(){
			uni.$on("updateNum", num=>{
				this.num += num
			})
		}
	}
</script>
```

#### vuex(状态管理器)

```shell
https://segmentfault.com/a/1190000015782272
```

#### Promise的用法

```shell
# 概括
Promise对象用于表示一个异步操作的最终状态(完成或失败),以及该异步操作的结果值,一般通过new来实例化,接受一个构造函数有并带两个函数参数,分别表示成功或失败的输出
# 实例
function promise1(){
	# 这里模拟异步动作,一般可以放置ajax请求,'promise-result'为请求成功后的返回结果
	# Promise实例只能通过resolve或者reject函数来返回,并用then()或者catch()获取
	# 不能直接在里面return..这样是取不到Promise返回值的
	return new Promise((resolve,reject) => {
		setTimeout(() => resove('promise1-result'), 1000)
	})
}
promise1()
.then(response => console.log(response))
.catch(error => console.log(error))

```

#### vuex-action的{commit}

```shell
# vuex中使用action处理异步请求时,常规写法如下
getMenuAction:(context) => {
	context.commit('SET_MENU_LIST',['承保1','核保2'])
}
# 可以使用简化写法
action:{
	getMenuAction:({commit}) => {
		commit('SET_MENU_LIST',['承保1','核保2'])
	}
}
```

#### vue中的ref用法

```vue
# 基本用法 本页面获取dom元素
<template>
<div id="app">
    <div ref="testDom">1111</div>
    <button @click="getTest">获取test节点</button>
</div>
</template>

<script>
	export default{
        methods: {
            getTest(){
                console.log(this.$refs.testDom)
            }
        }
    }
</script>

# 打印结果  <div>1111</div>
```

```vue
# 调用子组件中的data
### 子组件
<template>
<div>
   {{ msg }}
</div>
</template>

<script>
	export default {
        data(){
            return {
                msg: "hello world"
            }
        }
    }
</script>

### 父组件
<template>
<div id="app">
    <HelloWorld ref="hello"></HelloWorld>
    <button @click="getHello">获取helloworld组件中的值</button>
</div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue"
export default{
    components: {
        HelloWorld
    },
    data(){
        return{}
    },
    methods(){
        getHello(){
            console.log(this.$ref.hello.msg)
        }
    }   
}
</script>

# 打印结果为  hello world
```

```vue
# 调用子组件中的方法
### 子组件
<template>
<div>
</div>
</template>
<script>
	export default {
        methods:{
            open(){
                console.log("调用到了")
            }
        }
    }
</script>

### 父组件
<template>
<div>
</div>
</template>
<template>
  <div id="app">
    <HelloWorld ref="hello"/>
    <button @click="getHello">获取helloworld组件中的值</button>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  components: {
    HelloWorld
  },
  data() {
    return {}
  },
  methods: {
    getHello() {
      this.$refs.hello.open();
    }
  }
};
</script>
## 打印的值   调用到了
```

#### .sync修饰符用法及原理

```shell
vue中我们经常会用v-bind(缩写为:)给子组件传入参数。
或者我们会给子组件传入一个函数，子组件通过调用传入的函数来改变父组件的状态。

//父组件给子组件传入一个函数
 <MyFooter :age="age" @setAge="(res)=> age = res">
 </MyFooter>
 //子组件通过调用这个函数来实现修改父组件的状态。
 mounted () {
      console.log(this.$emit('setAge',1234567));
 }
 
这时子组件触发了父组件的修改函数使父组件的age修改成了1234567
这种情况比较常见切写法比较复杂。于是我们引出今天的主角 .sync
这时我们可以直接这样写

//父组件将age传给子组件并使用.sync修饰符。
<MyFooter :age.sync="age">
</MyFooter>
//子组件触发事件
 mounted () {
    console.log(this.$emit('update:age',1234567));
 }
 
这里注意我们的事件名称被换成了update:age
update：是被固定的也就是vue为我们约定好的名称部分
age是我们要修改的状态的名称，是我们手动配置的，与传入的状态名字对应起来

这样就完成了，是不是感觉简单了很多。

注意事项：
这里我们必须在事件执行名称前加上update：的前缀才能正确触发事件。
 
```



