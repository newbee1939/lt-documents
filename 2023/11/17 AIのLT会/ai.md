---
marp: true
theme: gaia
class: invert
size: 16:9
---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# Cloud Run の切り戻しについて

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

# 新機能をリリース

![](1.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 大量のエラーが発生

![](2.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# とりあえず切り戻したい..

![](3.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 作業工程が多い

1. 原因の特定
2. コードの修正・revert
3. PR 作成
4. 再度リリース
   - build
   - push
   - deploy

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# 切り戻しに時間がかかる

![](4.png)

---

<!--
_class:
    - lead
    - invert
_footer: ""
_paginate: false
-->

# ユーザーにも多大な影響が..

![](5.png)

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
- トラフィックの振り分けも可能

![](revision.png)

---

# トラフィックを以前のリビジョンに振り分けることで切り戻しが可能

- コードの revert や再デプロイは不要
- とりあえず切り戻してからコードの修正は（ゆっくり）すればいい

コーヒーイラスト

---

# 切り戻し方法

- コンソールから手動で実施
- GitHub Actions から実施

---

# Actions のモジュールを作りました

---

# デモをします

---

# 想定される質問

- リビジョンの名前を見てもどれに切り戻したらいいか分からない

---

# 考えていた案

- タグ付きリビジョンで Cloud Run をデプロイして、切り戻しのときはその番号を指定すればいいのでは？

---

# タグ付きリビジョンとは？

---

# タグ付きリビジョンの問題点

- 以下の条件の場合、Cloud Run の各リビジョンにもコストがかかる
  - リビジョンにタグが付いている（タグ付きリビジョン）
  - **リビジョン レベルの**最小インスタンスが 1 以上

---

# Cloud Run の最小インスタンス数の仕様について

- Cloud Run の最小インスタンス数には以下の 2 種類がある
  - サービスレベルの最小インスタンス数
  - リビジョンレベルの最小インスタンス数
- それぞれ設定方法が違う
- リビジョン レベルで最小インスタンス数を適用すると、リビジョンのデプロイ時に設定が有効になる
- サービスレベルで適用すると、新しいリビジョンをデプロイしなくても設定が有効になる
- Google の推奨はサービスレベルでの設定

> 最小インスタンス数はサービスレベルで適用し、サービスレベルとリビジョン レベルの最小インスタンス数を組み合わせることは避けることをおすすめします。

参考: https://cloud.google.com/run/docs/configuring/min-instances?hl=ja#service-vs-revision-level

---

# タグ付きリビジョンを用いた切り戻しは難しい

- 本番環境のサービスでは多くのサービスが最小インスタンス 1 以上になっている
- コード検索したところ、社内ではほぼ`リビジョンレベルの最小インスタンス`が指定されていた..
- タグ付きリビジョンを用いた切り戻しただと、コストの肥大化が予想される..

---

# 今回どうしたか？

- とりあえずデプロイ時にワークフローにリビジョンの番号を出すようにした
- 必要であればここからどのリリースでどのリビジョンにリリースしたかが分かる

---

# 今後の方針

- リビジョン管理の社内ベストプラクティスを確立
  - 切り戻し手順のドキュメント化
  - 切り戻し判断基準の明確化
- GitHub Actions モジュールの継続的改善
  - よりユーザーフレンドリーな UI
  - エラーハンドリングの強化
- モニタリングとの連携強化
  - リビジョン切り替え時の影響を可視化
  - 自動アラート設定の検討

---

# まとめ

- せっかく標準化に伴い Cloud Run が使用されるようになったので、「リビジョンを使った切り戻し」にしていきたい
- コンソールから実施してもいいですが、より簡単に切り戻したい場合は Actions のモジュールを使ってください

---

# 参考資料

- [サービスの最小インスタンス数を設定する](https://cloud.google.com/run/docs/configuring/min-instances?hl=ja#yaml_1)
- [テスト、トラフィックの移行、ロールバックにタグを使用する](https://cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration?hl=ja#tags)
