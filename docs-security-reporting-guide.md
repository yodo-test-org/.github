# Security 報告のやり方

`yodo-test-org` で Security 報告の流れを確認するための資料です。

詳しい検証用資料は、学習用リポジトリにも置いています。

https://github.com/yodo-test-org/org-security-md-lab/blob/main/docs/security-reporting-guide.md

## 報告経路

GitHub での Security 報告には、主に 2 つの経路があります。

1. `SECURITY.md` に書かれた手順に従う。
2. Private vulnerability reporting が有効なリポジトリで、GitHub の非公開フォームから報告する。

## Private vulnerability reporting が有効な場合

1. 対象リポジトリを開く。
2. **Security** タブを開く。
3. **Advisories** を開く。
4. **Report a vulnerability** を押す。
5. フォームに内容を入力する。
6. **Submit report** を押す。

この報告は public issue ではなく、GitHub 上でメンテナに非公開で通知されます。

## Private vulnerability reporting が無効な場合

1. 対象リポジトリの `SECURITY.md` を確認する。
2. 書かれている連絡先や手順に従う。
3. 連絡方法がない場合は、public issue に詳細を書かず、セキュリティ連絡先だけを確認する。

## 書くべき内容

- 対象リポジトリ
- 影響範囲
- 再現手順
- 想定される影響
- proof of concept
- ログやスクリーンショット
- 回避策や修正案

## 書いてはいけない場所

public issue、public discussion、SNS などに、未修正の脆弱性の詳細を書かないでください。
