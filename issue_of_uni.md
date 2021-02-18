### 页面跳转携带参数

```shell
let detail = {
	_id: item.cid,
    addname: item.addname,
    addtime: item.addtime,
    name: item.name,
    reasons: item.reasons
}
uni.navigateTo({
					url: "./delayindex?detail=" + encodeURIComponent(JSON.stringify(detail)) ,
				})
				
# delayindex页面
onLoad(options){
	const item = JSON.parse(decodeURIComponent(options.detail))
	    this.addname = item.addname,
        this.name = item.name,
        this.reasons = item.reasons,
        this._id = item._id
}
```



