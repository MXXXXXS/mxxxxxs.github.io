## 常见报错

- 选择元素, 元素可能为`null`
  - 使用`!`非空断言
  - 关闭`strictNullChecks`(不推荐)
  - 使用`if`包裹后续逻辑
  
- 类型`EventTarget`上不存在属性`value`
  
  - 方法1:
    - 需要指明`event`对象的类型
    - `(<HTMLInputElement>event.target).value`
    - `(event.target as HTMLInputElement).value`
  - 方法2:
    - `e.currentTarget` 它不香吗?
  
  补充知识:
  
  `target` is the element that triggered the event (e.g., the user clicked on)
  `currentTarget` is the element that the event listener is attached to.
  
- (TS7053)当用`string`索引一个对象的属性时
  - 需要定义对象`key`的类型
    `const myObj: {[index: string]:any} = {}`
  
- JSX 元素类型“Element[]”不是 JSX 元素的构造函数
  - 组件返回了一个数组, 而不是单个元素
  - 将数组用单个元素包裹, 或使用fragment
  
- 在node环境里, 使用webpack时, 源文件`__dirname`和`__filename`指向与传统node环境的行为不同(因为经过了编译, 输出到别的地方了).
需要先设置`context`选项
  
  ```json
  module.exports = {
    //...
    context: path.resolve(__dirname, 'app')
  }
  ```

  并设置
  
  ```json
  target: 'node',
  node: {
    __dirname: true,
    __filename: true,
  }
  ```
  
  此时`__dirname`即为`contex`的路径
  
  [webpack文档的解释]( https://webpack.js.org/configuration/node/#node__dirname )
  
- 使用 [MediaStream Recording API](https://devdocs.io/dom/mediastream_recording_api) 时, 需要安装"@types/dom-mediacapture-record"依赖以提供类型支持

- 导出默认enmu的方式

  ```typescript
  enum Foo {
    Bar, Baz
  }
  
  export default Foo;
  ```

  需要分开来写, 因为ES6的设计只有 `export default class ...` and `export default function ...`, [参考](https://github.com/microsoft/TypeScript/issues/3320#issuecomment-107241679)

## 常见需求

- 在对象中声明属性类型
**Interface:**
  
  ```typescript
  interface myObj {
      property: string;
  }
  
  var obj: myObj = { property: "My string" }
  ```
  
  **Custom type:**
  
  ```typescript
  type myObjType = {
      property: string
  };
    
  var obj: myObjType = { property: "My string" };
  ```

  **Cast operator:**

  ```js
  var obj = {
      myString: <string> 'hi',
      myNumber: <number> 132,
  };
  ```

  **Object type literal:**

  ```js
  var obj: { property: string; } = { property: "Mystring" };
  ```

  <https://stackoverflow.com/questions/12787781/type-definition-in-object-literal-in-typescript>
  
- 函数参数解构, 声明类型的写法

  ```typescript
  {emitDate}: {emitDate: ((_: Date | null) => void)}
  ```
  
- 箭头函数的类型声明写法

  ```typescript
  const fn = (src: string): string[] => {
      return src.split()
  }
  ```
  
- 定义函数类型

  ```typescript
  type Work = (cost: number, time: number) => number
  ```
  
- 创建tsconfig.json

  ```bash
  tsc --init
  ```

- 导入.svg文件, 需添加一个`custom.d.ts`, 填入

  ```json
  declare module "*.svg" {
    const content: any;
    export default content;
  }
  ```

  [参考]( https://webpack.js.org/guides/typescript/#importing-other-assets )

## 常见疑问

- 抽象类和接口的区别
  - 抽象类在运行时存在, 接口只在编译时存在
  - 抽象类可以包含具体的方法实现, 用于被继承实现.
    抽象类里的`实现细节`实现了某些功能, 而需要实现的`抽象方法`通过继承实现, 以**自定义**`实现细节`完成的功能.
- 检测到了source map但打不了断点
  解决方式: 
  打开方式不对, 不是在'localhost'或'域名'下的文件里, 而是在'webpack://'下

## 相关知识

- 使用npm包带上`@types`的scope, 源于 http://definitelytyped.org/ 项目, 为npm中大多数的包的类型支持版本

- 在使用各种包时, 每种包都有各种接口类型什么的, 导入进代码以获得语法提示, 比如

  ```typescript
  import express, {
    Request,
    Response
  } from 'express'
  ```

  