---
title: "Next: routingとlayout"
date: 2023-09-10T16:28:11+09:00
draft: false
---

デフォルトのファイル配置や命名を整理していたところ、routingが効かなくなったり、layoutが存在しないと怒られたりしたので調べてみたことのメモ。

参考: https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts

## NextのRouter

> Next.js uses a file-system based router

app(Pages Routerの場合はpages)ディレクトリ配下のディレクトリ構造がrouteになる。  
ディレクトリ名、ファイル名がrouteへ反映される。

これを知って以下の疑問が出てきた。  

ディレクトリ配下にページ以外のもの(ページ内で使うコンポーネントなど)を配置していると、意図しないrouteが作られたりするかもしれないからやめたほうがいい？

### appまたはpagesディレクトリ配下にページ以外のものを置かないほうがよいか
AppRouter, Pages Routerにより異なる。

App Routerの場合、意図しないrouteが作られるような事故は防がれる仕組みになっているので、置いてもよい。  
Pages Routerの場合はそうではないので、pagesディレクトリ配下にページ以外のものを置かないほうがよい。  

App Routerの場合、page.jsや、route.jsという名前のファイルに対してのみrouting可能。  
(僕が実装していた際は、この規則に違反していたためroutingが効かなくなり、404エラーが出た。)

参考: https://nextjs.org/docs/app/building-your-application/routing/colocation#safe-colocation-by-default

## Layout

ページへレイアウトを適用するためのファイル。  
App Routerの場合、appディレクトリ配下にRoot Layoutが必要。
page.jsに対応するLayout(Nesting Layouts)を個別定義することもできる。  
Nesting Layoutsは、上位ディレクトリに定義されたlayout.js内部にネストする形で適用される。  

以下、Root Layout, Nesting Layoutsを用意した場合のディレクトリ、ファイル配置の例

- app
    - layout.js (Root Layout)
    - dashboard
        - layout.js (dashboard用のNesting Layout)
        - page.js (app/layout.jsを適用した上で、app/dashboard/layout.jsを適用)
    - home
        - page.js (app/layout.jsを適用)

(僕が実装していた際は、Root Layoutがなく、Nesting Layoutsもなかったため、以下のエラーが出た。)  
> page.tsx doesn't have a root layout. To fix this error, make sure every page has a root layout.
