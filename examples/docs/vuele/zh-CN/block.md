## Block容器

通过Block容器进行页面的分区域布局

### 无title模式
:::demo
```html
<tc-block>
test
</tc-block>
```
:::

### title模式
:::demo
```html
<tc-block title="我是一个block">
test
</tc-block>
```
:::

### 自定义title模式
:::demo
```html
<tc-block>
  <div slot="title" style="color:red;">我是自定义title</div>
  <div style="font-size:18px;">test</div>
</tc-block>
```
:::

### Slots
| name | 说明 |
|------|--------|
| default | 默认标签正文内容 |
| title | 标题栏 |

### Attributes

| 参数          | 说明            | 类型            | 可选值                 | 默认值   |
|-------------  |---------------- |---------------- |---------------------- |-------- |
| title   | 标题文本   | string          | — | — |