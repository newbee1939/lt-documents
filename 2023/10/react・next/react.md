---
marp: true
theme: gaia
footer: "2023/09/13 React/Next LT会"
size: 16:9
paginate: true
# ゴール
# - Reactとは何かが分かる
# - Next.jsとは何かが分かる
# - Reactの課題が分かる
# - RSCがどのようにReactの課題を解決するかが分かる
# - App Routerとは何かが分かる
---

<!--
_class: lead
_footer: ""
_paginate: false
-->

# React/Next.js の最新トレンドに

# 少しだけ触れる

---

# 本日のメニュー

- React とは何か
- Next.js とは何か
- React の課題について
- React Server Components が解決する React の課題
- Next.js の App Router について
- React Server Components と App Router のデモ

---

# React とは何か

- React
  - UI を簡単に構築するための JavaScript ライブラリ
  - コンポーネントという概念を使って宣言的に画面を構築できる

---

# Next.js とは何か

- Next.js
  - React のフレームワーク
  - React の機能を拡張してより使いやすくしたもの
    - ex. ルーティング機能など
  - リアーキでも採用している

---

# React の課題について

- クライアント側で全ての JavaScript をレンダリングする
  - クライアント側のパフォーマンス悪化が懸念

画像貼る
https://nextjs.org/learn/basics/data-fetching/pre-rendering
![w:500](jest.png)

---

# React Server Components が解決する React の課題

---

# Next.js の App Router について

- Next.js には二つのモードがある
- Pages Router と App Router

- SSR について。SSR の話する？
- フロント側に全ての JavaScript を送信して実行しないといけない部分は変わっていない
- 重い

---

# React Server Components と App Router のデモ

---

<!--
backgroundColor: black
paginate: false
footer: ""
-->
