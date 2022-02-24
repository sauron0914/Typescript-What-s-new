# Overview

这个概述页面包含了一个`typescript`所有发布说明的简短版本，因为这个页面太大，代码示例禁用了它们的交互元素。

## Typescript4.2

更智能的类型别名保留

TypeScript 有一种方法可以为称为类型别名的类型声明新名称，如果你正在编写一组适用于 `string | number | boolean` 的函数，你可以编写类型别名以避免一遍又一遍地重复自身

```ts
type BasicPrimitive = number | string | boolean;
```
