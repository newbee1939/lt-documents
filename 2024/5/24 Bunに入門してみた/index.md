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
---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# Bunに入門してみた

---

## Bunとは何か？ 

オールインワンの JavaScript ランタイム＆ツールキット

<br>

![w:400 center drop-shadow](bun.png)

---

## JavaScriptランタイムとは？

- JavaScriptを実行する環境のこと
- 様々な種類がある
  - Node.js
  - Deno
  - WinterJS  
- Bunは最近特に伸びてきている

---

![w:1100 center drop-shadow](bun-trend.png)

---

## Bunのポイント

- 速い
  - Node.jsの20倍以上
- TypeScriptとJSXのサポート
  - .jsx、.ts、.tsxファイルを直接実行可能
- ESM & CommonJS、Node.jsとの互換性を担保
- 様々なJS開発ツールが搭載
  - テストランナー、パッケージマネージャなど

---

## BunはただのJSランタイムではない

<br>

>Bunは単なるランタイムではない。長期的な目標は、パッケージマネージャ、トランスパイラ、バンドル、スクリプトランナー、テストランナーなどを含む、JavaScript/TypeScriptでアプリを構築するための、まとまりのあるインフラツールキットとなることだ。

<br>

引用: [What is Bun?](https://bun.sh/docs#design-goals)

---

## 実際にBunを使ってアプリを作ってみた！

<br>
<br>

![w:400 center drop-shadow](draw.png)

---

@[times_hide](https://aa-jp.slack.com/archives/C011TPLB2E9)

![w:1030 center drop-shadow](tec.png)

---

## アプリを作る中で学んだこと・感じたこと

- TypeScriptの実行が簡単
- 環境変数の利用が簡単
- bun.lockbでパッケージを管理する
- Node.jsからの移行は簡単そう

---

## TypeScriptの実行が簡単

- Node.jsの場合
  - ts-nodeなどの利用が必要
- Bunの場合
  - `bun index.ts`で実行可能
  - 内部的にトランスパイルを行う

---

## 環境変数の利用が簡単

- Node.jsの場合
  - Dotenvなど別のパッケージを利用する必要がある
- Bunの場合
  - 設定不要
  - .envを用意するだけ
  - `Bun.env.HOGE`のように読み込める

---

## bun.lockbでパッケージを管理する

- Node.jsの場合
  - package.lock.json
- Bunの場合
  - bun.lockbで管理
  - バイナリ形式
  - パフォーマンスの向上が目的らしい

---

## Node.jsからの移行は簡単そう

- Node.jsとの違いが少なかったので、すんなり開発をすることができた
- Node.jsの開発に比べてタスクの数が減ることはあっても増えることはない気がする

---

## 感想

- Bunを触ってみて、以下は特に大きなメリットだなと感じた
  - TSを直接実行できる
  - Node.jsとの互換性がある
  - 依存パッケージを減らせる
- まだ触れていない機能も多いので、色々触って試してみたい

---

<!--
backgroundColor: black
footer: ""
-->
