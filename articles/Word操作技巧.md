# 快速查找替换

快速查找: `ctrl-f`

向后查找:  `ctrl-pagedown`

向前查找: `ctrl-pageup`

# 在下一页编辑(插入空白页)

`ctrl-enter`

# 撤销, 逆撤销

`ctrl-z`, `ctrl-y`

# 光标选择到行首/尾

`shift-home`,`shift-end`

# 跳至整个文档首/尾

`ctrl-home`,`ctrl-end`

# 拼写检查

`f7`

# 更新域和目录

`ctrl-a`, `f9`

# 转换大小写

`shift-f3`

# 尾注标号格式自定义

默认的尾注是自增的, 不需要手动维护, 很方便

但"便签选项"=>"编号格式"中自增类型样式就那么几样, 无法自定义, 比如带中括号的"[1], [2], [3]...", 而默认只有"1, 2, 3..."

这里有个曲线救国的路子, 使用"替换"功能

"替换"=>"特殊格式"=>"尾注标记", 此时"查找内容"变成"^e", 在"替换内容"中输入"\[^&\]"即可, 其中"^&"代表匹配的内容, 即尾注编号

**注意, 文档和尾注是两个搜索范围, 所以对文档内的尾注标号改完后需要到尾注区域再改一下**

*推荐对尾注格式的替换最后进行, 即文档完成后*

# 题注全部居中

万能的查找替换

1. 激活"查找"输入框

2. "替换">"更多">"替换/格式">"样式">"题注
3. 激活"替换"输入框
4. "替换/格式">"段落">"对齐方式/居中"

# 图片缩放和居中

贴一段VB代码

```vb
Sub 批量设置图片大小并居中()
  maxLength = 200
  On Error Resume Next '忽略错误
  For Each iShape In ActiveDocument.InlineShapes
    setImg iShape, maxLength
    centerImg iShape
  Next
  For Each Shape In ActiveDocument.Shapes
    setImg iShape, maxLength
    centerImg Shape
  Next
End Sub

Sub setImg(img, maxLength)
  maxRatio = 1.6
  ratio = img.Width / img.Height
  If ratio > maxRatio Then
    img.Width = maxLength * ratio
  ElseIf ratio < 1 / maxRatio Then
    img.Height = maxLength / ratio
  Else
    img.Width = maxLength
  End If
  '防止过宽过长图片
  max = maxLength * maxRatio
  If img.Width > max Then
    img.Width = max
  End If
  If img.Height > max Then
    img.Height = max
  End If
End Sub

Sub centerImg(img)
  img.Select
  Selection.ParagraphFormat.Alignment = wdAlignParagraphCenter
End Sub
```

"居中"只能对段落实现, 所以使用`Selection.ParagraphFormat.Alignment = wdAlignParagraphCenter`, 当然, 得先选择.

缩放的逻辑是这样的

1. 设置一个归一化的值`maxLength`, 和一个宽高比(默认1.6)`maxRatio`.
2. 超出`maxRatio`的以实际`ratio`乘以`maxLength`, 并最后使用`max`限制过宽过高
3. 不超出`maxRatio`默认将宽度设为`maxLength`

# 标题列表格式前缀自定义

"多级列表">"定义新的多级列表"

"编号">"更改列表级别"

"编号">"设置编号值"

# 公式标号

[参考](https://zhuanlan.zhihu.com/p/38300903)

# 不同页码类型插入

核心是使用"布局">"分隔符">"分节符">"下一页"来分隔文章各个部分页码的作用域, 区别于"分页符"只是用来分页.

然后再设置页码

首页不显示页码点选"首页不同", 然后删除.

# 删除页眉横线

`ctrl`+`shift`+`n`

# 给所有图片添加题注

1. 将域的代码复制的到剪贴板, 选中一张图片, "引用">"插入题注", 选择插入的题注, 右击"切换域代码", 选择域代码比如: `图 { STYLEREF 1 \s}-{ SEQ 图 \* ARABIC \s 1 }`(表现为`图1-1`), 复制
2. 查找替换, 查找`^g`匹配图片, 替换`^&^p^c`, 解释: `^&`代表匹配的内容, `^p`代表回车, `^c`代表剪贴板内容

# 键值对文本转换为表格

word有文字转换成表格的功能, 可以基于字符分隔

"插入">"表格">"文本转换为表格", 设置列宽和分隔符就可以了

# 三线表

[参考]( https://zhuanlan.zhihu.com/p/33000931)

随便选择一个表格, 在"设计"(可能有两个"设计", 选右边的)里"新建表格样式"自定义出一个三线表的格式, 然后应用于需要这种格式的表格.

# 表格转置

转置表格需要excel辅助, 粘贴到excel里转置后再粘贴回word

