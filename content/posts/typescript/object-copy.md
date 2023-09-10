---
title: "Objectのコピー方法"
date: 2023-09-01T10:08:38+09:00
draft: true
---

Objectのコピーについてまとめ

## コピーの種類
Deep CopyとShallow Copyの二種類  

### Deep Copy

### Shallow Copy
CopyしたObjectとそのプロパティがCopy元と参照を共有する。  

#### プロパティすべての参照を共有している場合

#### 参照型のプロパティの参照を共有している場合
以下を使った場合
- Object.assign
    - オブジェクトに対するスプレッド構文
      - { ...object }

```ts
const obj = {
    a: "1",
    array: ["z", "y", "z"],
};

const copy = { ...obj };

console.log("コピーしたオブジェクト: copy");
console.log(copy);

copy.a = "100";
copy.array[0] = "q"

console.log("コピーしたオブジェクト: copy");
console.log(copy);

console.log("コピー元のオブジェクト: obj");
console.log(obj);
```
