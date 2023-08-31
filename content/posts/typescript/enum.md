---
title: "TypeScriptのenumがちょっと改善されていた"
date: 2023-08-30T03:11:38+09:00
draft: true
---

TypeScriptのEnumが型安全になっていた。  
TypeScriptでEnumを使うのが良くないといわれている理由の確認中に見つけたのでメモ。

ここを使って動作確認をした。  
transpile結果も確認できるので便利。  
https://www.typescriptlang.org/play

### エラー
以下のコードを書いてみたところtypeエラーが出た。  
バージョンアップでエラーが出るように修正された？

Enumの型安全を検証するコード
```ts
enum FruitEnum {
    Apple = 0,
    Orange = 1
}

const fruit: FruitEnum = 6;
```

エラーメッセージ
> Type '6' is not assignable to type 'FruitEnum'.(2322)

### いつからこうなった？
TypeScript 5.0で修正された。  
https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#all-enums-are-union-enums  

### 備忘: transpile前後の比較
以下、Enum, Const Enum, Unionのtranspile前後の比較  

Enumは即時関数を使った形にtranspileされるので、Tree Shakingで振り落とせないデメリットがある。  
Const Enumか、Unionを使うのが良いかも。  
Const EnumにはBabelでtranspileできない等のデメリットがあるらしいが未検証。

#### Enum
transpile前
```ts
enum FruitEnum {
    Apple,
    Orange
}

const fruit: FruitEnum = FruitEnum.Apple;
```

transpile後
```js
var FruitEnum;
(function (FruitEnum) {
    FruitEnum[FruitEnum["Apple"] = 0] = "Apple";
    FruitEnum[FruitEnum["Orange"] = 1] = "Orange";
})(FruitEnum || (FruitEnum = {}));
const fruit = FruitEnum.Apple;
```

#### Const Enum
transpile前
```ts
const enum FruitEnum {
    Apple,
    Orange
}

const fruit: FruitEnum = FruitEnum.Apple;
```

transpile後
```js
const fruit = 0 /* FruitEnum.Apple */;
```

#### Union
transpile前
```ts
const FruitsConst = {
    Apple: 0,
    Orange: 1,
} as const;
type FruitType = (typeof FruitsConst)[keyof typeof FruitsConst];
const fruit: FruitType = FruitsConst.Apple;
```

transpile後
```js
const FruitsConst = {
    Apple: 0,
    Orange: 1,
};
const fruit = FruitsConst.Apple;
```