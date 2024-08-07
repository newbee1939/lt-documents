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

## コールバックによる非同期処理の扱い

//

---

<!--
backgroundColor: black
footer: ""
-->
