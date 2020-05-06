# 引入外部的css文件

[官方库指南]( https://github.com/zeit/styled-jsx#styles-in-regular-css-files )

## webpack配置

[参考](https://github.com/zeit/styled-jsx/issues/498#issuecomment-427157914)

1. Making sure the the `styled-jsx/webpack` loader runs before `'babel-loader'` (positioned last in the `rules` array)(loader是自右向左应用的)
2. Adding the `css` extension the `babel-loader`'s `test` property: `test: /\.(jsx?|css)$,`.(这一步容易忽略, 因为官方库好像没有提到, 或者显示地说明)

## 全局样式

官方库文档描述的, 默认是"scoped", 所以`<style jsx global>`标签不会起效. 详细配置参考官方库文档.

# 常见报错

### 组件可以包含多个  <style jsx>  元素，但多个  <style jsx>  元素必须是兄弟元素，不可以是父子关系，否则会报错：

> SyntaxError: Detected nested style tag:
> styled-jsx only allows style tags to be direct descendants (children) of the outermost JSX element i.e. the subtree root.

这通常出现在这样一种情况: 

```tsx
return (
    <div className="main">
      <label className="file">
        <input></input>
      </label>
      <label className="level">
        主题色细粒度
        <input></input>
      </label>
      <label className="colorNum">
        主题色数量
        <input></input>
      </label>
      {themeColors.map((color) => {
        return (
          <div key={color.join(",")}>
            <style jsx>{`
              div {
                width: 100px;
                height: 50px;
                background-color: rgb(${color.join(",")});
              }
            `}</style>
          </div>
        );
      })}
      <style jsx>{`
        .main {
          display: grid;
          grid-template-columns: repeat(12, 1fr);
          grid-template-rows: repeat(6, 1fr);
        }
        .file {
          grid-area: 1/1/2/4;
        }
        .level {
          grid-area: 1/4/2/8;
        }
        .file {
          grid-area: 1/8/2/12;
        }
      `}</style>
    </div>
  );
```

一个元素内既有其本身的样式, 又有列表渲染的各个元素的样式.

这就会报错.

解决方法是将列表渲染提出来写成一个单独的组件, 再引入.

### 样式没有应用到元素上

`<style jsx>`元素好像只能接受简单的字符串或表达式

参考:  https://github.com/zeit/styled-jsx/issues/595 

```tsx
function GridTile({
  indexOfW,
  indexOfH,
}: {
  indexOfW: number;
  indexOfH: number;
}) {
  const color = `rgb(
  ${Math.random() * 100 + 100},
  ${Math.random() * 100 + 100},
  ${Math.random() * 100 + 100}
)`;
  return (
    <div className={`tile ${indexOfW}-${indexOfH}`}>
      {`${indexOfW}-${indexOfH}`}
      {/* style applied */}
      <style jsx>{`
        .tile {
          background-color: ${color};
          justify-self: stretch;
          align-self: stretch;
          grid-column: ${indexOfH + 1};
          grid-row: ${indexOfW + 1};
        }
      `}</style>
      {/* style not applied */}
      {/* <style jsx>{`
        .tile {
          background-color: rgb(
            ${Math.random() * 100 + 100},
            ${Math.random() * 100 + 100},
            ${Math.random() * 100 + 100}
          );
          justify-self: stretch;
          align-self: stretch;
          grid-column: ${indexOfH + 1};
          grid-row: ${indexOfW + 1};
        }
      `}</style> */}
    </div>
  );
}
```

