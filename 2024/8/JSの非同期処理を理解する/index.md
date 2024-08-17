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

## 本日のメニュー

---

## 非同期処理とは何か？

- 裏で行われる処理
- 時間がかかる処理
    - e.g. 通信、ファイルの読み書き、DBへのアクセス

---

## JavaScriptにおける非同期処理

- JS・TSでは時間がかかる処理は非同期（ノンブロッキング）で実行するのが推奨されている
- なぜなら、JSでは、実行モデルとしてシングルスレッド（single-threaded）が採用されているから
- シングルスレッドかつ通信処理が同期的（ブロッキング）だと、あるクライアントと通信している際にプログラムの実行がストップしてしまって非効率

---

## シングルスレッドとは

- 一つの実行環境（ node index.jsなどの形で起動されるプロセス）内でプログラムの複数箇所が同時に（並列に）実行されることはない
- TypeScriptプログラムが実行される場合、常にプログラムのある1箇所のみが実行される

![w:980 center drop-shadow](4.png)

---

## シングルスレッドかつブロッキング

![w:940 center drop-shadow](2.jpg)

---

## シングルスレッドかつノンブロッキング

![w:940 center drop-shadow](3.jpg)

---

## 非同期処理をJS・TSから扱う方法

- コールバック関数
- Promise

---

## コールバック関数

- 最も原始的な方法
- ES2015でPromiseが導入されるまでは、非同期処理はコールバック関数で表すのが普通だった
- 非同期処理が終わった後に呼び出される関数

```ts
console.log('1. 処理開始');
readFile('hoge.txt', (data) => {
  console.log('3. 読み込み完了');
});
console.log('3. 読み込み開始');
```

---

## Promise

- ES2015で追加された非同期処理のための機能
- 非同期処理そのものを表すオブジェクト
- Promiseを使うことで、より便利かつ分かりやすい形で非同意処理を扱うことができる
- 非同期処理においては「終わったあとに何をするか」を表す関数が不可欠
- コールバック関数ベースの非同期処理の場合は非同期処理を開始する関数（e.g. setTimeout, readFile）に直接これをコールバック関数として渡していた
- 非同期処理を行う関数はPromiseオブジェクトを返す
- Promiseオブジェクトに対して、thenメソッドで終わった後に行う処理を表す関数を登録する
- 関数は、当該Promiseが表す非同期処理が完了した時点で呼び出される

```ts
import { readFile } from "fs/promises";

// pの型はPromise<string>。非同期関数がPromiseを返す
const p = readFile("foo.txt", "utf8");

p.then((data) => {
  // 非同期処理が完了したときに呼び出される
  console.log(data);
})
```

--- 

## コールバック関数からの変化

- 従来は「非同期処理を行う関数にコールバック関数を直接渡す」という人まとまりの処理だった
- 「非同期処理を行う関数はPromiseオブジェクトを返す」「返されたPromiseオブジェクトにthenでコールバック関数を渡す」という2段階の処理に分離された
- より抽象的・統一的い非同期処理を表すことができる

---

## Promiseの動作の流れを図解

```ts
import { readFile } from "fs/promises";

// 1. 非同期関数からPromiseを受け取る
// pの型はPromise<string>。非同期関数がPromiseを返す
const p = readFile("foo.txt", "utf8");

// 2. 非同期処理が完了したら非同期関数がPromiseに対して結果を登録する（Promiseの解決）
p.then((data) => {
  // 非同期処理が完了したときに呼び出される
  console.log(data);
})
```


---

## Promiseのメリット

- コールバック関数を直接渡す方式の場合、使いたいAPIごとにコールバック関数をどのように渡せばいいかを調べる必要がある
- PromiseベースのAPIでは非同期処理を行う関数ならどんな関数でも「PromiseおBっジェクトを返す」という点で共通しており、結果も「Promiseの解決」という共通の機構をと通して伝えられる
- これにより、利用する側はPromiseの使い方さえ覚えていれば非同期処理の結果を無事に受け取ることができる
- Promiseオブジェクトのthenに渡すコールバック関数は常に1引数であり、どんな非同期処理であろうと共通

---

## Promiseの使い方

非同期処理が成功した場合の処理には.thenを使い、失敗した場合の処理の登録には`catch`を使う

---

## Promiseのメソッド1

複数のPromiseオブジェクトを組み合わせて新しいPromiseオブジェクトを作る

Promise.allは複数のPromiseを合成するメソッド。
Promiseオブジェクトの配列を引数として受け取り、「それら全てが成功したら成功となるPromiseオブジェクト」を作って返す。
複数の非同期処理を並行して行いたい場合に適している。

どれか一つでも失敗したらその時点でPromise.allが返したPromiseも失敗する。

```ts
Promise.all([
  prisma.users.deleteMany(),
  prisma.posts.deleteMany(),
  prisma.comments.deleteMany(),
])
```

---

## Promiseのメソッド2

Promise.race

Promiseの配列を受け取る。そして、そのうち最も早く成功または失敗したものの結果を、全体の（Promise.raceが返したPromiseの）結果とする。

---

## async/await構文

Promiseをベースとした非同期関数を扱うための便利な機能

thenよりもasync/awaitの方がよく使う。

```ts
async function get3(): Promise<number> {
  return 3;
}
```

async関数の返り値は必ずPromiseになる。
async関数内部でreturn文が実行された場合、return文で返された値が、返り値のPromiseの結果となる。（async関数内でreturn文が実行された時点で、return文に渡された値を結果として、返り値のPromiseが成功裡に解決される）

await式はasync関数の中で使える構文。

`await 式`という形式を取る。
普通awaitに与える式はPromiseオブジェクト。
与えられたPromiseの結果が出るまで待つ。

```ts
const sleep = (duratioin: number) => {
  return new Promise<void>((resolve) => {
    setTimeout(resolve, duration)
  });
};

async function get3() {
  await sleep(1000);
  return 3;
}

const p = get3();
p.then(num => {
  console.log(`num is ${num}`);
});
```

awaitの返り値。
式として与えられたPromiseの結果。

Promiseの結果は本来thenメソッドで得るものですが、async関数の中ではこのようにawait式を使うことでPromiseの結果を得ることができる。
awaitが「Promiseを待つ」という働きを持ちthenの代わりとなっているため。

await式を使うと「ある非同期処理が終わってから次の非同期処理をする」というプログラムをまるで同期プログラムのように（上から下に進むという流れに則った形で）書くことができる

---

## なぜawaitはasyncの中でしか使えないのか？

---

<!--
backgroundColor: black
footer: ""
-->
