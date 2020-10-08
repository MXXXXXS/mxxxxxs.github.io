# action

是一个对象, 必须有一个"type"属性(个人理解为这个"action"的名字, 应该叫"name"), 其余属性为附带的数据.

比如

```js
{
	type: 'NAME',
	text: 'sofy'
}
```

一般"type"使用大写 snake_case, 并且为了避免"hard coding"一般使用同名变量, 即`const NAME = 'NAME'`, `{type: NAME, text: 'sofy'}`

## action creator

很多时候为了方便获取"action", 使用与其 "type"同名的一个"camelCase"函数返回. 函数接受数据作为属性. 这种返回一个 action 的函数称为 **action creator**

比如

```js
function name(text) {
	return {type: NAME, text}
}
```

"action"是 redux 里的数据源, 也是一个信号, 用于触发"state"变化. 

"state"的数据更新, 来自"dispach"的触发, 其接受一个"action"作为参数, `dispatch(action)`.

# reducer

一个纯函数, `(preState, action) => nextState`.

用于状态的转移.

由于和 Array.prototype.reduce的第一个参数的函数签名很像所以叫"reducer".

*注意: 由于是纯函数, 不要在里面做一些副作用的事情比如说 dispatch别的 action, 导致一连串的动作触发*

## combineReducer

`createStore`只接受一个 reducer, 作为整个应用的状态转移函数. 输入整个状态和一个 action, 输出整个应用的下一个状态. 要处理整个状态的 reducer显然太大了,可以使用 `combineReducer`拆分 reducer, 也定义了 store 的形状

`combineReducer`接受一个对象, 每个 key 对应了 `state`的 key, 每个 value 为会接受输入这个 state 的一个 reducer

拆分出的 reducer 的第一个参数只关注其对应的部分`state`, 而不是整个应用的`state`, 依据第二个参数 action 判断是否返回新值

在 dispatch 操作时所有 reducer 都会执行一遍, 各自接受对应的 state, 依据 action 的 type 是否匹配决定是否返回新值或是返回原本的 state

# store

从名字上可以得知其是用来存放状态的, 但数据源并不是来自于此, 而是"action".

"store"显然可以用来获取状态, 使用其方法`getState`.

"store"有个`dispatch`来实现状态变化.

"store"使用`createStore`来创建, `createStore(reducer, [preloadedState], [enhancer])`, 后两个参数可选, 分别为指定一个初始状态和一个"Middleware"或"enhancer".

"Middleware"比较常见, "enhancer"比较少见.

"Middleware"给予"dispatch"额外的功能, "enhancer"给予"store"额外的功能.

常见的一个"middleware""redux-thunk"用于实现异步"dispatch", 默认的是同步触发的.

redux的逻辑是一个应用只有一个"store", 而`createStore`只接受一个"reducer". 各种状态有各种不同的转移方式, 所以redux提供了一个函数`combineReducers`来合并多个"reducer"为一个.

"store"还有一个`subscribe`方法实现状态变更后触发一些函数.

# selector

从名字上会使人联想到 css 选择器, 这里是一个从大的 "state" 中提取出部分状态或状态组合的一个函数.

由于 "state" 的哲学是只保存最小的状态, 很多数据是需要提取, 合并, 排序等各种处理的. 这会带来一个问题, "state"的每次变化都会导致重新计算, 有性能问题, 所以需要一层缓存, 比较新旧状态再决定是否重新计算.

这是一种优化的思想, 而不是和 redux 绑定的, 所以由一些第三方的库实现, 或者自己写. 常见的流行的工具是 [reselect](https://github.com/reduxjs/reselect). 项目中常见的`createSelector`就是这个库里的一个方法.

# dispatch

触发"state"改变的唯一方法, 是"store"的一个方法

"dispath"接受唯一参数: 一个"action"

"store"会的"reducer"会接受当前的"state"(`getState()`)和这个"action", 并执行

# react redux

redux 是一种基于单一 state 的应用逻辑管理思想. 脱离了具体的某个前端框架和库, 通常和 react 一起用会使用 react readux. React redux 提供了一些工具或者说代码组织范式.

## Provider

类似 react 里的 `Context.Provider`的, 被包含在其中的组件会接受到"state", 所以一般在项目组件树的最外层.

## mapStateToProps和mapDispatchToProps

推荐只在class 组件使用

react redux 的一个逻辑是组件用于不直接接触到 store, 而是使用 connect 来绑定, 这是做了一个解耦, 就需要 "mapxxxToProps"来使得组件能够获得一些方法操作 store

从名字上可知其二分别将"state"和"dispatch"映射到"props", 即"数据"和"行为", 前者自外而内使组件接受数据渲染视图, 后者触发行为来更改状态数据.

核心思想是组件只负责视图, 不包含功能逻辑.

## mapDispatchToProps

有两种配置方式, 官方推荐第一种对象形式, 比较简洁

###  一个对象

这种情况下组件不会接收到"dispatch"这个 prop

```js
{
...actionCreateors
}
```

react redux 自动使用 bindActionCreators 对这些 action creator 作转换

```js
// React Redux does this for you automatically:
dispatch => bindActionCreators(mapDispatchToProps, dispatch)
```

### 或一个函数

```
(dispatch, ownProps?) => {
  return {
    action0: () => dispatch(actionCreator(0)),
    action1: () => dispatch(actionCreator(1))
  }
}
```

配置好的 dispatch prop 会传给组件, 被调用时会 dispatch 一个 action

# useSelector和 useDispatch

在 function 组件使用

对应于原来在 class 组件使用的 mapStateToProps和mapDispatchToProps

## connect()

这是一个将"mapStateToProps", "mapDispatchToProps"和 "某个react 组件"绑定到一起的函数.

代码中某个组件文件最后通常有类似

```react
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoApp)
```

# redux-thunk

用于异步 dispatch

异步操作要 dispath 如果不借助中间件, 可以在`.then`中获得结果并调用`store.dispatch`来, 但这会导致 api 的不一致和更高的耦合, 

redux-thunk 的思路类似延迟求值

```js
const thunkedLogin = () =>     
  dispatch =>                  
    axios.get('/api/auth/me')  
    .then(res => res.data)
    .then(user => {
      dispatch(simpleLogin(user)) //这里就是原版的 dispatch 一个 action
    })
```

dispatch 原版只接受一个 action, 在引入redux-thunk后可以接受一个函数, 像上面这样的, 返回一个函数(第一个参数是 dispatch)

调用方式和普通 action 一样, `dispatch(thunkedLogin())`, 这使得 api 的使用方式保持一致, 解耦了 store 和具体的逻辑

`dispatch(thunkedLogin())`可以使用`.then`依据异步结果进一步操作

```js
store.dispatch(thunkedLogin())
.then(() => console.log('async from component A fulfilled'))
```

# 异步操作

参考[这里](https://redux.js.org/tutorials/essentials/part-5-async-logic#writing-async-thunks), 请求的发送往往有三个阶段, 即 action 的 type 是一组三个的

# redux toolkit

`createReducer`里的 builder 模式(另一个是 map 模式)处理 action 时, 必须遵循顺序`addCase => addMatcher => addDefaultCase`. 其中`addMatcher`类似于写 Switch case 语句时的[多 case 匹配](https://stackoverflow.com/questions/13207927/switch-statement-multiple-cases-in-javascript)(fall-through)