---
title: "Componentへパラメータを渡すとエラー"
date: 2023-08-29T02:45:20+09:00
draft: false
---


# Componentへパラメータを渡すとエラー

Componentへパラメータを渡せなかったときの原因と解決方法

## エラー発生時の実装
タスクの一覧を引数にとってタスクリストを作成するコンポーネントを定義し、タスクの一覧を渡そうとしていた。
taskItems.jsx
```ts
export const TaskItems = (tasks: Task[]) => {
    const taskElements = tasks.map(t => {
        return <div className="task" key={t.id}></div>
    })
    return (<div className="taskList">{taskElements}</div>)
}
```

home.jsx
```ts
export default function Home() {
  const tasks: Task[] = [
    // 略
  ]
  return (
    <main>
      <TaskItems tasks={tasks}></TaskItems>
    </main>
  )
}
```

## エラー
Typeエラーが出た。

> Type '{ tasks: Task[]; }' is not assignable to type 'IntrinsicAttributes & Task[]'.
> Property 'tasks' does not exist on type 'IntrinsicAttributes & Task[]'.ts(2322)

## 解決方法

必要なプロパティを単一のオブジェクトとして受け取る形に定義しなおした。

taskItems.jsx
```ts
export const TaskItems = (props: {tasks: Task[]}) => {
    const taskElements = props.tasks.map(t => {
        return <div className="task" key={t.id}></div>
    })
    return (<div className="taskList">{taskElements}</div>)
}
```

## なぜこうするのか
> in fact, props are the only argument to your component! React component functions accept a single argument, a props object:

Reactのコンポーネントは単一のオブジェクトを受け取るから。  
オブジェクトに含まれるプロパティは分割代入で小分けにされてる。

https://react.dev/learn/passing-props-to-a-component#step-2-read-props-inside-the-child-component

