# tips


### hide win
```
go build -ldflags "-H windowsgui"
``` 
### 
```
go build -ldflags "-s -w" 
```
`-s` 去掉符号表，然后 `panic` 的时候 `stack trace` 就没有任何文件名/行号信息了。
`-w` 去掉 `DWARF` 调试信息，得到的程序就不能用 `gdb`调试。

### 查看汇编
```
go tool compile -S main.go
```

### static file

```
// 根访问
http.Handle("/", http.FileServer(http.Dir(".")))
// 指定 路径访问
http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static/"))))
```

### cookie
```
	http.HandleFunc("/one", func(w http.ResponseWriter, r *http.Request) {
		cookie := &http.Cookie{Name: "cookie", Value: "zxysilent", Path: "/", HttpOnly: true,MaxAge: 7200}
		http.SetCookie(w, cookie)
	})
	http.HandleFunc("/two", func(w http.ResponseWriter, r *http.Request) {
		cookie, _ := r.Cookie("cookie")
		w.Write([]byte(cookie.String()))
	})
```

### 跨域
```
w.Header().Set(`Access-Control-Allow-Origin`, `*`)
```
### 中间件
1
```
http.Handle(`/`, mid(http.HandlerFunc(fn)))
func mid(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		//中间件的逻辑
		w.Header().Set("Content-Type", "application/json")
		w.Header().Set(`Access-Control-Allow-Origin`, `*`)
		next.ServeHTTP(w, r)
	})
}
```
2
```
http.HandleFunc(`/`, mid(fn))
func mid(next http.HandlerFunc) http.HandlerFunc{
	return func(w http.ResponseWriter, r *http.Request) {
		//中间件的逻辑
		w.Header().Set("Content-Type", "application/json")
		w.Header().Set(`Access-Control-Allow-Origin`, `*`)
		next.ServeHTTP(w, r)
	}
}
```
### npm 
```
prefix = D:\Program Files\nodejs\node_global
cache = D:\Program Files\nodejs\node_cache
```
### cnpm 
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```
// --history-api-fallback 解决本地开发 vue-router-history模式 404 错误
"scripts": {
    "dev": "webpack-dev-server --content-base ./ --open --inline --hot  --port=82 --history-api-fallback --compress --config build/webpack.dev.config.js",
    "build": "webpack --progress --hide-modules --config build/webpack.prod.config.js"
  },
```
install git 
```
sudo apt-get install git git-core git-doc
```
## node

```
npm install --registry=https://registry.npm.taobao.org
```
prod
> npm run webpack.build.production

<!--more-->
dev
> npm run webpack

thinkjs

> 前执行 `npm run compile `命令，将 src/ 目录编译到 app/ 目录，然后将 app/ 目录下的文件上线

## ssh

```
// 不加入邮箱 自定义git 服务部署会失败
ssh-keygen -t rsa -C "zxysilent@outlook.com"
```
### linux环境
- 临时设置
`export PATH=$PATH:/usr/local/go/bin`
- 当前用户的全局设置
 打开`~/.bashrc`，添加 `export PATH=$PATH:/usr/local/go/bin`
 使生效：`source .bashrc`
- 所有用户的全局设置 `/etc/profile`
在里面加入：`export PATH=$PATH:/usr/local/go/bin`
使生效 `source profile`


### mysql 
 `uid:pass@tcp(host:port)/dbname?charset=utf8&parseTime=true`

### jquery
1. `$('#id').siblings()` 当前元素所有的兄弟节点
2. `$('#id').prev()`当前元素前一个兄弟节点
3. `$('#id').prevaAll()`当前元素之前所有的兄弟节点
4. `$('#id').next()`当前元素之后第一个兄弟节点
5. `$('#id').nextAll()` 当前元素之后所有的兄弟节点\

### caddy 
`-service install -name caddy -agree -log "E:\Caddy\logs\caddy.log" -conf "E:\Caddy\conf"  -email "zxysilent@foxmail.com"`

### mysql
```sql
update 数据表名 SET 字段名 = replace(字段名, '要替换的字符串', '替换为') where 设定条件;
```

## npm

```
npm cache clean --force
```

### vue 

render 

``` js
{
  // 和`v-bind:class`一样的 API
  'class': {
    foo: true,
    bar: false
  },
  // 和`v-bind:style`一样的 API
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 正常的 HTML 特性
  attrs: {
    id: 'foo'
  },
  // 组件 props
  props: {
    myProp: 'bar'
  },
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器基于 `on`
  // 所以不再支持如 `v-on:keyup.enter` 修饰器
  // 需要手动匹配 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅对于组件，用于监听原生事件，而不是组件内部使用 `vm.$emit` 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令。注意事项：不能对绑定的旧值设值
  // Vue 会为您持续追踪
  directives: [
    {
      name: 'my-custom-directive',
      value: '2',
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // Scoped slots in the form of
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => createElement('span', props.text)
  },
  // 如果组件是其他组件的子组件，需为插槽指定名称
  slot: 'name-of-slot',
  // 其他特殊顶层属性
  key: 'myKey',
  ref: 'myRef'
}

```
### win10链接vpn后wifi无法上网
- 文件路径 C:\Users\用户名\AppData\Roaming\Microsoft\Network\Connections\Pbk\
- 用记事本打开rasphone.pbk找到IpPrioritizeRemote=1
>改成0就取消"从远程网络上使用默认网关"

### golang代理

- GOPROXY
- https://goproxy.cn,direct