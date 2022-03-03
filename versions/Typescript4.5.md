# 自 Beta 版和 RC 版以来有什么新变化？

自从我们的 [beta 发布](https://devblogs.microsoft.com/typescript/announcing-typescript-4-5-beta/)和 [RC](https://devblogs.microsoft.com/typescript/announcing-typescript-4-5-rc/) 发布以来，4.5 经历了一些变化。

自 Beta 版以来我们所做的最大更改是对 Node.js 12 的 ECMAScript 模块支持已推迟到未来版本，现在仅在夜间版本中作为实验标志提供。这不是一个容易的决定，但我们的团队对生态系统的准备情况和如何/何时使用该功能的一般指导有共同的担忧。我们认为最好是平滑用户体验，而不是发布最终对大多数人来说太令人沮丧的东西。但与此同时，您仍然可以使用对` --module nodenext` 和 `--moduleResolution nodenext` 的新支持作为[ TypeScript 每晚构建](https://www.typescriptlang.org/docs/handbook/nightly-builds.html)中的实验性功能。如果您尝试在 TypeScript 4.5 中使用这些设置，您将收到一条错误消息，指示您改用每晚构建。

自我们的 RC 发布以来，我们添加了有关`新 JSDoc 功能的注释`。虽然这些功能实际上包含在 RC 中，但它们并未包含在我们之前的发行说明中。

在语言编辑方面，自 TypeScript 4.5 beta 以来，我们引入了更多的代码片段完成——`特别是用于方法实现和覆盖`。

由于对 package.json 文件的过多 realpath 调用，我们还解决了 [--build 模式下的性能回归问题](https://github.com/microsoft/TypeScript/pull/46209)。这些变更将会在 `Typescript4.5` 实现，但也被向后移植到 `TypeScript 4.4.4`, 如果这种回归阻止您尝试 TypeScript 4.4，那么与过去的版本相比，您应该会在 `--build` 模式下看到相当或更好的速度。