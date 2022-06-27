---
title: "Widening"
date: 2022-06-27T23:58:38+09:00
draft: false
---

# Widening

ReactでコンポーネントをスタイリングしていたらTypescript関連のエラーがでたのでメモ。

以下のようなコードを書いた
``` js
const Component = () => {
    const divStyle = {
        display: "flex",
        flexDirection: "row",
    };

    return (
        <div style={divStyle}></div>
    );
}
```

出てきたエラー
> Type '{ display: string; flexDirection: string; }' is not assignable to type 'Properties<string | number, string & {}>'.
  Types of property 'flexDirection' are incompatible.
    Type 'string' is not assignable to type 'FlexDirection | undefined'.ts(2322)

FlexDirection型を設定すべきところ、"row"がstring型になってしまっていた (TypescriptのWideningによるもの)  
const assertionでWideningを抑制して解決

以下のコードに修正
``` js
const Component = () => {
    const divStyle = {
        display: "flex",
        flexDirection: "row" as const, // const assertion
    };

    return (
        <div style={divStyle}></div>
    );
}
```
