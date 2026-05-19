# .github

このリポジトリは、`yodo-test-org` の Organization 共通 GitHub 設定を管理するためのリポジトリです。

## 役割

- GitHub Organization のプロフィール表示
- Organization 共通の community health files
- Organization 共通の issue / pull request 案内
- Organization 共通の security policy

## ファイル一覧

- `profile/README.md`: Organization トップページに表示されるプロフィール
- `SECURITY.md`: 脆弱性報告ポリシー
- `CONTRIBUTING.md`: コントリビューション方針
- `CODE_OF_CONDUCT.md`: 行動規範
- `SUPPORT.md`: サポート方針
- `PULL_REQUEST_TEMPLATE.md`: pull request テンプレート
- `ISSUE_TEMPLATE/config.yml`: issue 作成時の案内

## 優先順位

各リポジトリに同じ種類のファイルが存在する場合は、リポジトリ側のファイルが優先されます。

たとえば、あるリポジトリに `SECURITY.md` がある場合、そのリポジトリではこの Organization 共通の `SECURITY.md` ではなく、リポジトリ固有の `SECURITY.md` が使われます。

## 検証用リポジトリ

`SECURITY.md` の継承や上書き挙動は、次の学習用リポジトリで確認できます。

https://github.com/yodo-test-org/org-security-md-lab

## 運用資料

企業向けの `.github` リポジトリ運用方針は、次の資料にまとめています。

https://github.com/yodo-test-org/.github/blob/main/docs/github-repository-operations.md
