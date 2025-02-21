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
_paginate: false
-->

# Cloud Run を爆速で切り戻す

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 突然ですが

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# こんな経験はありませんか？

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 新機能をリリース🚀

![w:350](1.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 大量のエラーが発生

![w:350](2.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# とりあえず切り戻したい..

![w:350](3.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 作業工程が多い

```
1. 原因の特定
2. コードの修正 or revert
3. PR 作成
4. レビュー
5. 再度リリース
   - build
   - push
   - deploy
```

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 切り戻しに時間がかかる

![w:350](4.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# ユーザーに多大な影響が...

![w:350](5.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# その悩み、Cloud Run の リビジョン を使うことで解決できます

---

# Cloud Run のリビジョンとは？

- Cloud Run のデプロイ履歴を保持する機能
- 各デプロイで自動的に作成される
- **トラフィックの振り分けも可能**

![w:650](revision.png)

---

# トラフィックを以前のリビジョンに振り分けることで切り戻しが可能

- コードの revert や再デプロイは不要
- とりあえず切り戻してからコードの修正は焦らず実施

![](cof.png)

---

# 切り戻し方法

- コンソールから手動で実施
    - 権限の付与が必要
    - 操作ミスが生じる可能性
- GitHub Actions から実施
    - より素早くできる
    - 操作ミスの心配もない

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# リビジョンを切り戻す 

# Actions のモジュールを作りました

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# デモをします

---

# 想定される質問

リビジョンの名前を見てもどれに切り戻したらいいか分からない

---

# 考えていた案

タグ付きリビジョンで Cloud Run をデプロイして、切り戻しのときはタグの番号を指定する

---

# タグ付きリビジョンとは？

リビジョンにタグを付けることができる機能

![w:800](tag.png)

---

# タグ付きリビジョンの問題点

以下の条件の場合、Cloud Run の各リビジョンにもコストがかかる

- リビジョンにタグが付いている
- **リビジョン レベルの**最小インスタンス数が 1 以上

---

# Cloud Run の最小インスタンス数について

- Cloud Run の最小インスタンス数には以下の 2 種類がある
  - **サービスレベル**の最小インスタンス数
  - **リビジョンレベル**の最小インスタンス数
- それぞれ設定方法が違う

---

# 違いについて

- リビジョン レベルで最小インスタンス数を適用すると、リビジョンのデプロイ時に設定が有効になる
- サービスレベルで適用すると、新しいリビジョンをデプロイしなくても設定が有効になる

---

# Googleはサービスレベルでの設定を推奨

> 最小インスタンス数はサービスレベルで適用し、サービスレベルとリビジョン レベルの最小インスタンス数を組み合わせることは避けることをおすすめします

参考: [サービスレベルとリビジョン レベルでの最小インスタンス数の適用](https://cloud.google.com/run/docs/configuring/min-instances?hl=ja#service-vs-revision-level)

---

# 社内の状況は？

- 本番環境のサービスでは多くのサービスが最小インスタンス 1 以上になっている
- 社内ではほぼ`リビジョンレベルの最小インスタンス`が使用されている
- タグ付きリビジョンを用いた切り戻しだと、コストの肥大化が予想される

---

# どう対応したか？

- とりあえずデプロイ時にワークフローにリビジョン名を出すようにした
- 必要であればここからどのリリースでどのリビジョンにリリースしたかが分かる

![w:740](flow.png)

---

# 今後の方針

1. とりあえず1アプリに組み込んでみる
2. 実装方法を書いた社内Qiitaを執筆
3. 開発で必要に応じてモジュールを組み込んでもらう

---

# まとめ

- Cloud Run の切り戻しは リビジョン を使って実施したい
- コンソールからでも実施は可能だが、より簡単に切り戻したい場合は Actions のモジュールを使ってください

---

<!--
backgroundColor: black
footer: ""
-->


---

# 参考資料

- [サービスの最小インスタンス数を設定する](https://cloud.google.com/run/docs/configuring/min-instances?hl=ja#yaml_1)
- [テスト、トラフィックの移行、ロールバックにタグを使用する](https://cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration?hl=ja#tags)
