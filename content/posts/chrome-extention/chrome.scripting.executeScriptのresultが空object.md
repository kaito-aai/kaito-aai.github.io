---
title: "chrome拡張機能開発: chrome.scripting.executeScriptのresultが空objectになってしまう"
date: 2023-07-22T13:30:11+09:00
draft: true
---

## 現象

chrome.scripting.executeScriptで実行した関数の返り値が空objectになってしまう。

```ts
const getHeadingInfo = (): NodeListOf<Element> => {
    const elements = document.querySelectorAll("div");

    // ここでは値が存在する
    console.log(elements)

    return elements;
}

chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
    chrome.scripting.executeScript({
      target: { tabId: tabs[0].id },
      func: getHeadingInfo
    }, (injectionResult) => {
        // getHeadingInfoから返ってくる値が空objectになっている
        console.log(injectionResult.result)
    }),
})
```

## 原因

## 解決


