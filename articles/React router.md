*不明确指明主语的, 主语都是指react router*

# 各个标签结构

使用`<Router>`包裹, 限定作用范围, 类似react 的"Provider"

`<Switch>`用于包裹`<Route>`或`<Redirect>`, 用于在`<Switch>`位置渲染哪些组件. `<Switch>`只渲染其内部匹配到的第一个`<Route>`

而`<Route>`或`<Redirect>`包裹具体的组件, 并设置匹配规则

`<Route>`里也可以包含`<Switch>`, 构成嵌套的路由, 甚至是递归的路由

`<Link>`标签用来设置 url, 触发`<Switch>`匹配. `<Link>`的样式可以自定义, 通过其包含一个元素或在其外面包裹一层.



# 匹配的逻辑是这样的

`<Route>`的路径转为一个正则(使用"Path-to-RegExp"), 调用"test"方法, 参数为`<Link>`中的 url

当某个`<Link>`被点击时, `<Switch>`中每个`<Route>`的路径都会执行上述逻辑进行匹配

`exact`用于是否要求`<Route>`的路径和`<Link>`中的 url完全一样, 忽略末尾"/"

`strict`用于是否严格匹配路径末尾的"/", 不设置就无论路径末尾是否有"/"

*在`<Route>`上同时添加`exact`和`strict`实现完全精确匹配*

[参考](https://stackoverflow.com/a/52275337)

# 占位符

`<Route>`路径内使用":"开头的一个标识, 类似正则匹配时使用括号捕获匹配结果

# route props

## match

和 react router 的其他内建对象一样, 是一个对象

包含以下属性`params, isExact, path, url`, 后两个属性`path, url`在**构建嵌套的路由**时很实用

- `path` - (string) The path pattern used to match. Useful for building nested `<Route>`s
- `url` - (string) The matched portion(部分) of the URL. Useful for building nested `<Link>`s

可以通过很多途径获得, 比如使用 hook `useRouteMatch`

path 代表上一层的路径, url 代表上一层的整个 url

在嵌套路由时, 一般 path 会用于`<Route>`的 path 属性, url 用于`<Link>`的 to 属性

可以参考官方示例里的[Nesting](https://reactrouter.com/web/example/nesting)这章

## location

内建对象, 可以使用`useLocation`获得

结构类似

```js
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere',
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```

用来获得当前路径的信息

key作为一个类似"id"的存在, 用于标识这块数据

pathname, search, hash 是一个 url 的几个部分

state 可以携带一些用户自定义的数据

## history

# \<Route>的一些 props

## component 和 render

component 赋值为一个组件，路由变化时，每次会卸载再挂载这个组件，因为会使用 React.createElement 来创建

render 就不会有上述情况

赋值给二者的组件都会获得相同的 route props (match, location and history)

## children

拥有比以上二者更高的优先级，工作方式和 render 相同，但无论是否匹配路由都会被调用

直接赋值到该 prop 或用`<route>`包裹，等同，这其实是 react 的特性。

# withRouter

没有被`<Route>`包裹的组件没法响应到路由的变化，但有些组件不需要依据路由被渲染但也想要获知路由变化，就可以用`withRouter`包裹。

比如一个侧边栏内某个选项（组件）要高亮以匹配当前路由，但这个组件又不是被`<Route>`包裹渲染的（因为其一直存在），为了感知到路由变化需要使用`withRouter`包裹。