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

