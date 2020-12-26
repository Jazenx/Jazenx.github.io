---
title: TypeScript 实现一个 Pick
date: 2019-06-25 15:22:33
tags: TypeScript
---
Pick 是 TypeScript 中提供的 utility types 之一。它的主要功能就是从一个类型中挑选一组属性来构造一个新的类型。

具体使用方法如下：

```ts
interface User {
  name: string;
  age: number
  sex: string;
}

type SingleDog = Pick<User, "name" | "age">;

const Billy: SingleDog = {
  name: "Billy Wang",
  age: 18
};
```

那么如果通过泛型来实现一个 Pick 呢?

首先我们要用到 extend 操作符。extend 操作符在泛型中的用处来添加泛型约束。我们知道 K 是 T 里面的类型属性。keyof 操作符可以获取目标类型的所有键，返回其联合类型。我们用 keyof 和 extend 结合起来使用来检查所要挑选的属性是否合规：

```ts
 K extends keyof T
```

然后我们使用 in 操作符来遍历枚举目标属性，并把对应的类型提出：

```ts
[P in K]: T[P]
```

然后结合起来，得出 Pick 的最终实现如下：

```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P]
}
```

## 参考链接

[4 - 实现 Pick](https://www.typescriptlang.org/play/?#code/PQKgUABBAsELQUHnagG5wgBQJYGMDWl5yFH4BGAnhAIIB2ALgBYD21FAYgK4QAUAAgIZ0AZuwCUEAMSAA70CqyhPbUMzCSXYYANrTgZq+fOP0RAGRmA7t11QAfBECMroHYYwDLWgK8DAFUqBv-0CL0YBh-wETWgOfj0AJQhATlNAbfj3QApYwHnEwCAGMwhAA9NAAHTAf3lACldAUMVAO39AELcAA0xcAB4AFQAaCABpc1zAbZtAaPVAZ2VAe+VYqHxALATAcfjAOblcitzAWjlASATAS6NAPR1AcgNc4tzAU3NAEPNAAgTAbx86uMBo+UAgzTjc3doAZ3xtWgBTACdBPiwTiGLGABNGCABvfChaDFo1E4AuCH3aGdtABzN4Qe4nfZYIEABw+zD+AKB1FBUCgWEYAFsYd9Tvc-iRGIxvgJ8ABfOK0Mgwm53R5oM4nABuGBOAHcIABeCAAWTIBRwJQejHKAHIPl8TqKIAAfCCijHY3Ene6i8xxDHUAEQWjCv50xgM5msjnc15otES75-UUAYRJ1AgZyJmNFpTB6KxOJOeL+lzU+xO7qgFKgu1ycUsgAp1CAAcU+9HYJAggCg5QCn5oBod0AWP-0Wi0GH7H7AYAHLD0AB0ACt9uXGGdgcBoMAAF70OC2gByYBAwDAfdAEAA+sOR6ORxBAAby2UAx3KAQA8h2PF4OID2+1Sabz+dhBWVKpZuQIyH2wAOl4uIIBpW0Aq9GpdYLs-j1cYbF12gvCAAUQAjuw+Gpyh+AAeNJYG+ZIQIIzqYvKPDriccBln+3wopCwDsB8Aaimu1I3FgfCBvsXIQAA2vgQEgbQhTfr+ahUcBJygSqACM5R8gKQqPGKVpSuYvHBp+9GgVRP5-nRFEqgATKxW5FAaXGfN80pygqXrKqqvHmPxxYQLB+xwCcgmaOczpnPgbHbhxIrytxSnyoq3p4rZoraEyf4YOp7oALp9sc5yXNctzCi8+DcYigIgvgEJQrC8LUGFyKohA9lqQSRIOmAFJgL5FxXDc5EMXiTHBe8Cm-P84UohlPl0H5uUCeJ9wScVOqlfFEWekqPoqqlxInKSmX9iA94PsugDQcoAAHKAKbWw0PiuvagPgliAGBKgDVclOgDHkYAKt45nmBZFiWUIVtWtb1o2wACPsbLnC2badotECZltO35oWxalkdNZ1g2Tb7MS6GKFq92AC9mgBYmiYz17W9h1Vp9p2tu2XarmAQA)
