---
marp: true
theme: gaia
class: invert
size: 16:9
style: |
  img[alt='center'] {
      display: block;
      margin: 0 auto;
  }
  strong,b {
    color: red;
  }
  h2 strong,b {
    color: red;
  }
# この発表を見た人のゴール
# 目的
# - JSのシングルスレッドの仕組みを理解する
# - JSの非同期処理を理解する
# - JSの非同期処理で利用する関数やPromiseについて理解する
# TODO
# Zennとか技術記事としても投稿する
# 一石二鳥
# 究極のわかりやすさ
# プレゼンのスライド
# コピーライティングの本
# https://www.engilaboo.com/promise-async-await/
---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# JavaScriptの非同期処理を理解する

---

## 非同期処理とは何か？

- 裏で行われる処理
  - e.g. 通信、ファイルの読み書き、DBへのアクセスなどの時間がかかる処理

---

## JSでは時間がかかる処理は非同期（ノンブロッキング）で実行するのが推奨

- JSは実行モデルとしてシングルスレッドを採用
- シングルスレッドかつ処理が同期的（ブロッキング）だと非効率

---

## シングルスレッドとは

一つの実行環境内でプログラムの複数箇所が並列に実行されることはない

![w:980 center drop-shadow](4.png)

---

## シングルスレッドかつブロッキング（同期）

![w:940 center drop-shadow](2.jpg)

---

## シングルスレッドかつノンブロッキング（非同期）

![w:900 center drop-shadow](3.jpg)

---

## 非同期処理をJS・TSから扱う方法

- コールバック関数
- Promise

---

## コールバック関数

- 最も原始的な方法
- ES2015でPromiseが導入されるまでは、非同期処理はコールバック関数で表すのが普通だった

```ts
readFile('hoge.txt', (data) => {
  // 非同期処理が完了したときに呼び出される
  console.log(`読み込み完了！: ${data}`);
});
```

---

## Promise

- ES2015で追加された非同期処理のための機能
- 非同期処理を行う関数はPromiseオブジェクトを返す
- Promiseオブジェクトに対して、thenメソッドで終わった後に行う処理を表す関数を登録する

---

```ts
// 1. 非同期関数からPromiseを受け取る
// pの型はPromise<string>
const p = readFile("foo.txt", "utf8");

// 2. thenで非同期処理が完了したあとに呼び出す処理を登録する
// 実際に非同期処理が完了したら、非同期関数（readFile）がPromiseに対して結果を登録する（Promiseの解決）
p.then((data) => {
  // 3. 非同期処理が完了したあとに呼び出される
  console.log(`非同期処理が完了しました！: ${data}`);
})
```

--- 

## コールバック関数からの変化

- 従来は「非同期処理を行う関数にコールバック関数を直接渡す」というひとまとまりの処理だった
- 2つに分離された
  1. 非同期処理を行う関数はPromiseオブジェクトを返す
  2. 返されたPromiseオブジェクトにthenでコールバック関数を渡す
- より抽象的・統一的に非同期処理を表すことができるように

---

コールバック関数を直接渡す方式の場合、使いたいAPIごとにコールバック関数をどのように渡せばいいかを調べる必要がある

![w:800 center drop-shadow](5.png)

---

PromiseベースのAPIでは非同期処理を行う関数ならどんな関数でも「Promiseを返す」という点で共通している

![w:800 center drop-shadow](6.png)

---

Promiseの使い方さえ覚えていれば非同期処理の結果を受け取ることができる

![w:800 center drop-shadow](7.png)

---

## Promiseのメソッド1: Promise.all

- 複数のPromiseを合成するメソッド
- Promiseオブジェクトの配列を引数として受け取り、「それら全てが成功したら成功となるPromiseオブジェクト」を作って返す
- 複数の非同期処理を並行して行いたい場合に適している

```ts
Promise.all([
  prisma.users.deleteMany(),
  prisma.posts.deleteMany(),
  prisma.comments.deleteMany(),
])
```

---

## Promiseのメソッド2: Promise.race

- Promiseの配列を受け取る
- 最も早く成功または失敗したものの結果を、全体の（Promise.raceが返したPromiseの）結果とする

```ts
Promise.race([promise1, promise2]).then((value) => {
  // promise1, promise2のうち、先に成功した結果を返す
  console.log(value);
});
```

---

## async/await構文

- Promiseをベースとした非同期関数を扱うための便利な機能
- thenよりもasync/awaitの方がよく使う

---

## async関数

- async関数の返り値は必ずPromiseになる
- async関数内部でreturn文が実行された場合、return文で返された値が返り値のPromiseの結果となる

```ts
// Promiseオブジェクトを返す
async function get3(): Promise<number> {
  return 3;
}
```

---

## await式

- await式はasync関数の中で使える構文
- `await 式(Promiseオブジェクト)`という形式を取る
- 与えられたPromiseの結果が出るまで待つ
  - thenの代わりに使用することが多い

```ts
async function get3() {
  // Promiseの解決を待つ
  await sleep(1000);
  return 3;
}
```

--- 

## async/awaitのメリット

- readFile(...).then(...)のようにthenを使うよりも記述がシンプルになる
- 「ある非同期処理が終わってから次の非同期処理をする」というプログラムをまるで同期プログラムのように書くことができる

---

## まとめ

- JSで非同期関数を表現する方法
  - コールバック関数
  - Promise
- asycn/awaitを使うことで、よりPromiseを簡単に扱うことができる

---

<!--
backgroundColor: black
footer: ""
-->
