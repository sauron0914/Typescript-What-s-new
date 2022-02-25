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

即使 `JavaScript` 没有任何语法来建模前导剩余参数，我们仍然能够通过使用使用前导剩余元素的元组类型声明 `...args` 剩余参数来将 `doStuff` 声明为接受前导参数的函数。这可以帮助对大量现有的 `JavaScript` 进行建模！

有关更多详细信息，请参阅[原始PR](https://github.com/microsoft/TypeScript/pull/41544)

### 在操作中更为严格的检查

在 `JavaScript` 中，在 `in` 运算符右侧使用非对象类型是运行时错误。 `TypeScript 4.2` 确保可以在设计时捕捉到这一点。

```ts
// @errors: 2361
"foo" in 42;
```
此检查在大多数情况下是相当保守的，因此如果您收到有关此的错误，则可能是代码中的问题。

非常感谢我们的外部贡献者 [Jonas Hübotter](https://github.com/jonhue) 的[ pull request](https://github.com/microsoft/TypeScript/pull/41928)!

### --noPropertyAccessFromIndexSignature(没有来自索引签名的属性访问)

早在 `TypeScript` 首次引入索引签名时，您只能使用诸如 `person["name"]` 之类的“括号”元素访问语法来获取它们声明的属性。

```ts
interface SomeType {
  /** This is an index signature. */
  [propName: string]: any;
}
function doStuff(value: SomeType) {
  let x = value["someProperty"];
}
```

在我们需要处理具有任意属性的对象的情况下，这最终变得很麻烦。例如，想象一个 API，其中通过在末尾添加额外的 s 字符来拼写属性名称是很常见的。

```ts
interface Options {
  /** File patterns to be excluded. */
  exclude?: string[];
  /**
   * It handles any extra properties that we haven't declared as type 'any'.
   */
  [x: string]: any;
}
function processOptions(opts: Options) {
  // Notice we're *intentionally* accessing `excludes`, not `exclude`
  if (opts.excludes) {
    console.error(
      "The option `excludes` is not valid. Did you mean `exclude`?"
    );
  }
}
```

为了使这些类型的情况更容易，不久前，当类型具有字符串索引签名时，`TypeScript` 使得使用像 `person.name` 这样的`点`属性访问语法成为可能。这也使得将现有 `JavaScript` 代码转换为 `TypeScript` 变得更加容易。

在某些情况下，用户更愿意明确选择加入索引签名——当点属性访问与特定属性声明不对应时，他们更愿意收到错误消息。

这就是 `TypeScript` 引入一个名为 [noPropertyAccessFromIndexSignature](https://www.typescriptlang.org/tsconfig#noPropertyAccessFromIndexSignature) 的新标志的原因。在这种模式下，您将选择使用 `TypeScript` 的旧行为，该行为会发出错误。这个新设置不在 [strict](https://www.typescriptlang.org/tsconfig#strict) 的标志系列之下，因为我们相信用户会发现它在某些代码库上比其他代码库更有用。

您可以通过阅读相应的[pull request](https://github.com/microsoft/TypeScript/pull/40171/)来更详细地了解此功能。我们还要非常感谢向我们发送此 pull request 的 [Wenlu Wang](https://github.com/Kingwl)！


### abstract Construct Signatures (抽象的构造函数)