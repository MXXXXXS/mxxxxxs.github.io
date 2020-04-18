# 引入外部的css文件

[官方库指南]( https://github.com/zeit/styled-jsx#styles-in-regular-css-files )

## webpack配置

[参考](https://github.com/zeit/styled-jsx/issues/498#issuecomment-427157914)

1. Making sure the the `styled-jsx/webpack` loader runs before `'babel-loader'` (positioned last in the `rules` array)(loader是自右向左应用的)
2. Adding the `css` extension the `babel-loader`'s `test` property: `test: /\.(jsx?|css)$,`.(这一步容易忽略, 因为官方库好像没有提到, 或者显示地说明)

## 全局样式

官方库文档描述的, 默认是"scoped", 所以`<style jsx global>`标签不会起效. 详细配置参考官方库文档.