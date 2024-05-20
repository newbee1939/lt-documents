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
# 聞き手の状態ゴール
# -
---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# Bunに入門してみた

---

## GitHub Actions とは？

自動テスト・自動リリース（CI/CD）を行うためのツール

<br>

![w:600 center drop-shadow](actions.webp)

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

### 【発生した事象】

## **Dependabot が作成する プルリクエスト で**

## **dev 環境にリリースできない...**

---

## Dependabot とは？

自動的にパッケージを更新してプルリクエストを発行してくれる GitHub の機能

<br>
<br>

![w:600 center drop-shadow](dependa.svg)

---

## dev 環境とは？

各プルリクエストごとにリリースしている動作確認用の環境

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

## なぜ Dependabot の プルリクエスト で

## dev 環境にリリースできなかったのか？

---

## 原因: **Dependabot から Secret の値を読めない**から

- Secret = GitHub Actions で秘匿情報を保存する機能
- dev 環境をリリースするワークフロー上で Secret を参照
- Dependabot は Secret を読むことができないのでエラーになった

```yml
steps:
  - id: generate-token
    uses: actions/create-github-app-token@f2acddfb5195534d487896a656232b016a682f3c # v1.9.0
    with:
      app-id: 386721
      private-key: ${{ secrets.KEY }} <--- ここでSecretを参照
      owner: ${{ github.repository_owner }}
```

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# どう対応したか？

---

## トリガーを pull_request_target に変更した

- トリガー = ワークフローを動かす条件
- `pull_request`だと Dependabot から Secret を読めない
- `pull_request_target`だと読める

<br>

```yml
name: release dev

on:
  # pull_request: <--- 変更前
  pull_request_target: <--- 変更後
```

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# dev 環境にリリース
# できるようになった！

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# 解決！

![w:350 drop-shadow](happy.png)

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# してなかった...

![w:350 drop-shadow](zt.png)

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

### 【発生した事象 2】

## **変更を push しても dev 環境が更新されない...**

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

## なぜ dev 環境が更新されなくなったのか？

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

### 【原因】

## トリガーを変更したことで 
## github.sha の値が変化しなくなったから

---

## 前提: デプロイimageの値に github.sha の値を使用している

pushの度に`github.sha`が変化しないとdev環境も更新されない

<br>

```yml
on:
  pull_request:
jobs:
  push-image:
    steps:
      - name: Build docker image
          tags: ${{ env.IMAGE_URI }}:${{ github.sha }}-${{ github.run_attempt }}
```

---

## **github.shaの値はトリガー毎に異なる**

<br>

- pull_requestトリガー
  - PRのブランチの最後のコミット
- pull_request_targetトリガー
  - PRのベースブランチの直近のコミット

---

トリガーを pull_request_target に変えたことで、変更内容を push してもイメージが更新されなくなった

---

## 現状を整理

- pull_request トリガーだと Dependabot から Secret を読めない
- pull_request_target トリガーだと dev 環境が更新されない

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

# どうする？

![w:300 drop-shadow](think.png)

---

## 解決策: Dependabot Secret を利用する

- Dependabot Secret = Dependabot 用の Secret
- Dependabot Secret を使うと、pull_request トリガーを利用しても**Dependabot から Secret の値を参照できる**

<br>

![center drop-shadow](secret.png)

---

## 最終的にはこうなった

※関係ない部分は省略しています

```yml
on:
  pull_request: <--- 変更をpushするたびにgithub.shaの値を更新させる
jobs:
  push-image:
    steps:
      - name: Build docker image
          tags: ${{ env.IMAGE_URI }}:${{ github.sha }}-${{ github.run_attempt }}

  dispatch-release-envoy-gateway:
    steps:
      - id: generate-token
        with:
          private-key: |
            ${{ secrets.KEY }} <--- Dependabotの場合はDependabot Secretから値を参照する
```

---

<!--
_class:
  - lead
  - invert
_footer: ""
-->

## 全てのプルリクエストで dev 環境が

## リリース&更新されるようになった！

---

## まとめ

- github.sha はトリガー毎に取得されるハッシュ値が異なる
- Dependabot から Secret の値を参照したいときは Dependabot Secretを使う

---

<!--
backgroundColor: black
footer: ""
-->
