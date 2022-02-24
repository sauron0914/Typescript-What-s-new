# 概述

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

在 `TypeScript` 中，元组类型旨在为具有特定长度和元素类型的数组建模。

```ts
// A tuple that stores a pair of numbers
let a: [number, number] = [1, 2];
// A tuple that stores a string, a number, and a boolean
let b: [string, number, boolean] = ["hello", 42, true];
```

在 `TypeScript 4.2` 中，其余元素的使用方式得到了专门扩展。在之前的版本中，`TypeScript` 只允许 `...rest` 元素位于元组类型的最后一个位置。

然而，现在剩余元素可以出现在元组中的任何地方——只有一些限制。

```ts
let foo: [...string[], number];
foo = [123];
foo = ["hello", 123];
foo = ["hello!", "hello!", "hello!", 123];
let bar: [boolean, ...string[], boolean];
bar = [true, false];
bar = [true, "some text", false];
bar = [true, "some", "separated", "text", false];
```