## 返回结果的含义

normalizr 的结果包含两项, 其分别包含如下含义

result: 用于表示原数据的结构

entities: 用于包含提取出来的数据

结构+数据, 信息量足够, 所以能denomalize还原出原数据

## id 的作用

normalizr 需要每一块数据有个key用来标识数据块, 默认为"id", 也可以自定义, 设置"idAttribute"

## Array的多 schema 情况

`new schema.Array`第一个参数有两种情况, 单个的 schema 或者多个 schema

前者的使用场景是数组中每个元素的提取方式都是相同的, 只需要一个 schema 就可以了

后者的使用场景是数组元素需要依据某个值作区分, 需要不同的 schema 来解析提取

数组元素多结构的情况需要配合`new schema.Array`第二个参数使用

这里拿官网示例说明

```js
const data = [{ id: 1, type: 'admin' }, { id: 2, type: 'user' }];

const userSchema = new schema.Entity('users');
const adminSchema = new schema.Entity('admins');
const myArray = new schema.Array(
  {
    admins: adminSchema,
    users: userSchema
  },
  (input, parent, key) => `${input.type}s`
);

const normalizedData = normalize(data, myArray);
```

原数据 data (下称"**数组**") 的元素依据"type"来作了区别, 这里`new schema.Array`引入了第二个参数, 一个函数

```js
(input, parent, key) => `${input.type}s`
```

这个函数的参数解析如下

input 即**数组**的每一个元素; parent 是**数组**的上一级对象, 这里没有; key 是**数组**在上一级对象中对应的 key, 这里也没有

这个函数应用于每一个数组元素, 返回的值**用于索引要使用的 schema** (在`new schema.Array`的第一个参数中定义)

输出如下

```js
{
  entities: {
    admins: { '1': { id: 1, type: 'admin' } },
    users: { '2': { id: 2, type: 'user' } }
  },
  result: [
    { id: 1, schema: 'admins' },
    { id: 2, schema: 'users' }
  ]
}
```

## Values

array 是针对一个数组提取, 而 values 是针对一个对象的值(可以看做是 Object.values(obj) 后用 array 提取)

# 为什么使用 normalizr

在使用 react 构建应用的过程中, 一份数据会传给很多组件, 这些组件会因为这份数据的结构而高度耦合.

一旦这份数据的结构改变, 所有的相关组件都需要作修改, 非常麻烦.

normalizr 的引入思想是, 一份数据使用 id 标识并传递给各个组件, 各个组件再使用 selector 从 state.entities 里去拿, 组件之间不会因为这份数据的结构而耦合.

