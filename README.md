# Overview

这个概述页面包含了一个`typescript`所有发布说明的简短版本，因为这个页面太大，代码示例禁用了它们的交互元素。

## Typescript4.2

更智能的类型别名保留

TypeScript 有一种方法可以为称为类型别名的类型声明新名称，如果你正在编写一组适用于 `string | number | boolean` 的函数，你可以编写类型别名以避免一遍又一遍地重复自身

```ts
type BasicPrimitive = number | string | boolean;
```

在 `TypeScript 4.2`，我们的内部结构会更聪明一些，我们追踪它们最初是如何编写和构造的部分来跟踪类型是如何构造的，我们还跟踪并区分其他别名实例的类型别名！

能够根据您在代码中使用它们的方式打印回类型意味着作为 `TypeScript` 用户，您可以避免显示一些不幸的巨大类型，这通常转化为获得更好的 `.d.ts` 文件输出、错误消息、 和编辑器类型显示在快速信息和签名帮助中。 这可以帮助 `TypeScript` 对新手来说更加平易近人。

欲了解更多消息，查看这个[第一个PR,改进了围绕保留联合类型别名的各种情况](https://github.com/microsoft/TypeScript/pull/42149)，以及[第二个PR，保留间接别名](https://github.com/microsoft/TypeScript/pull/42284)


## 元组（Tuple Types）类型的前面、中间、以及剩余元素