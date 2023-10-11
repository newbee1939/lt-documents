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

# 少しだけ触れてみる

---

# 本日のメニュー

- React とは何か
- Next.js とは何か
- React の課題について
- React Server Components が解決する React の課題
- Next.js の App Router について
- React Server Components と App Router のデモ

---

# 本日のゴール

React Server Components と App Router をふんわり理解している

![w:350](obake.png)

---

# React とは何か

- React
  - UI を簡単に構築するための JavaScript ライブラリ
  - コンポーネントという概念を使ってパーツを組み立てる感覚で画面を構築できる

![w:400](react.png)

---

# Next.js とは何か

- Next.js
  - React のフレームワーク
  - React の機能を拡張してより使いやすくしたもの
    - ex. ルーティング機能など

![w:400](next.png)

---

<!--
_footer: ""
_paginate: false
-->

# React の課題について

- クライアント側でアプリの全ての JavaScript をレンダリングする
  - クライアントに送信される JS のサイズが大きい
  - 実行にコストがかかる
- クライアント側のパフォーマンス悪化が懸念

![w:540](app.png)

---

# そこで

### React Server Components が登場！

---

# React Server Components(RSC)とは

- コンポーネントを「クライアント側でレンダリングされるコンポーネント」と「サーバー側でレンダリングされるコンポーネント」に分ける React の新機能
- クライアントに送信される JS の量（クライアント上でレンダリングされるコード量）が減るため、パフォーマンスの向上が期待されている

---

# 要は

**今までクライアント側でしか実行できなかった React が、サーバー側でも実行できるようになった**ということ

---

# Next.js の App Router について

- Next.js には二つのモードがある
  - Pages Router と App Router
- App Router が現在推奨されているモード
- App Router では、React Server Components が採用されている
- デフォルトだと、実装したコンポーネントは「サーバーコンポーネント」になる
- クライアントコンポーネントにする場合は、`use client`を記述する必要がある

---

# React Server Components と App Router のデモ

- サーバー側で実行されるコンポーネントと、クライアント側で実行されるコンポーネント

---

# まとめ

- React では、クライアント側の負担の増加が課題だった
  - アプリを丸ごとクライアント側で構築するのは、パフォーマンス的に厳しい
- そこで登場したのが React Server Components
  - コンポーネントを「サーバーコンポーネント」と「クライアントコンポーネント」に分ける
  - クライアント側に送信する JS の量を減らすことに成功
- Next.js の App Router は React Server Components がベースになっている

---

# 資料

- [一言で理解する React Server Components](https://zenn.dev/uhyo/articles/react-server-components-multi-stage)
- [Next.js 公式ドキュメント](https://nextjs.org/docs)
- [Next.js から学ぶ Web レンダリング ~React 誕生以前から App Router with RSC までの流れ~](https://zenn.dev/suzu_4/articles/2e6dbb25c12ee5)
- [What's "Next" JS Meetup](https://www.youtube.com/watch?v=WHMm6w41_WI&ab_channel=TimeeEngineering)
- [Nextjs で理解する React Server Components 徹底解説【React18】](https://youtu.be/A78v05JSyqg?si=EJiKhE35K11TbcGe)

---

<!--
backgroundColor: black
paginate: false
footer: ""
-->
