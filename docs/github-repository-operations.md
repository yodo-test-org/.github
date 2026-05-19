# 企業向け `.github` リポジトリ運用ガイド

この資料は、企業 Organization で `.github` リポジトリを運用するための判断基準と具体的な運用手順をまとめたものです。

対象読者:

- GitHub Organization owner
- security manager
- platform / developer productivity team
- OSS 公開を管理するチーム
- 社内 GitHub 運用担当者

## `.github` リポジトリの役割

`.github` リポジトリは、Organization 全体に適用する GitHub 上の共通ファイルを管理するための特別なリポジトリです。

主な用途:

- Organization profile の表示
- `SECURITY.md` による脆弱性報告ポリシーの提示
- `CONTRIBUTING.md` による contribution 方針の提示
- `CODE_OF_CONDUCT.md` による行動規範の提示
- `SUPPORT.md` によるサポート方針の提示
- issue / pull request template の共通化

## 重要な前提

default community health files は、対象リポジトリに同種のファイルが存在しない場合にだけ使われます。

例:

- 各リポジトリに `SECURITY.md` がない場合、`.github/SECURITY.md` が使われる。
- 各リポジトリに独自の `SECURITY.md` がある場合、リポジトリ側が優先される。

つまり、`.github` リポジトリは「全社共通の最低ライン」を配布する場所です。個別プロダクトの事情がある場合は、各リポジトリ側で上書きします。

## 公開範囲の選択肢

企業運用では、次の 3 パターンを検討します。

| パターン | visibility | 主な用途 |
| --- | --- | --- |
| 外部公開 | public | OSS、外部利用者、脆弱性報告者向け |
| 内部公開 | internal | Enterprise 内の全社員・全メンバー向け |
| 非公開 | private | 限定チームだけの内部運用資料向け |

## 外部公開で運用する場合

`.github` リポジトリを public にします。

### 向いているケース

- public repository を持つ企業。
- OSS contributor から issue / pull request を受ける。
- 外部の security researcher から脆弱性報告を受けたい。
- Organization profile を外部向けに整えたい。
- `SECURITY.md` を外部から読める状態にしたい。

### メリット

- 外部利用者が security policy を確認できる。
- public repository に default community health files を適用しやすい。
- Organization profile を採用、技術広報、OSS 活動に使える。
- issue / pull request template を public repo に共通適用しやすい。
- 外部 contributor に対して、貢献ルールや行動規範を明示できる。

### デメリット

- 内容はインターネットに公開される。
- 社内の連絡先、担当者名、内部 SLA、詳細なインシデント対応手順は書きにくい。
- 運用が古くなった場合、外部からも古い情報が見える。
- 企業としての公式文書に近くなるため、レビュー責任が重くなる。

### 公開してよい内容

- 外部向けの脆弱性報告窓口
- public issue に脆弱性詳細を書かないでほしいという注意
- OSS contribution の基本方針
- code of conduct
- public repository の support policy
- Organization の概要

### 公開しないほうがよい内容

- 社内 escalation path の詳細
- 担当者個人名
- 内部 Slack channel
- 未公開プロダクト名
- 具体的な検知ルールや監視条件
- セキュリティ対応の内部 SLA
- 攻撃者に有利な構成情報

### 具体的な運用

1. `.github` リポジトリを public で作成する。
2. `SECURITY.md` には外部報告者向けの最小限の手順を書く。
3. `CONTRIBUTING.md` には OSS contribution の受け入れ条件を書く。
4. `CODE_OF_CONDUCT.md` を置く。
5. `SUPPORT.md` で support 対象と対象外を明確にする。
6. `profile/README.md` で Organization の公開プロフィールを整える。
7. 変更は pull request で行い、security / legal / developer relations の review を通す。
8. 四半期ごとにリンク切れ、連絡先、実態とのズレを確認する。

### 推奨構成

```text
.github/
  README.md
  SECURITY.md
  CONTRIBUTING.md
  CODE_OF_CONDUCT.md
  SUPPORT.md
  PULL_REQUEST_TEMPLATE.md
  ISSUE_TEMPLATE/
    config.yml
  profile/
    README.md
  docs/
    github-repository-operations.md
```

## 内部公開で運用する場合

Enterprise 環境では、internal repository を使える場合があります。internal repository は enterprise member から参照できます。

### 向いているケース

- GitHub Enterprise Cloud / Enterprise Server を利用している。
- 対象読者が社員や業務委託など enterprise member に限られる。
- 社内リポジトリ向けの共通運用ルールを置きたい。
- 外部公開できないが、社内全体には展開したい。

### メリット

- 社内向けの詳細な運用ルールを書きやすい。
- 社内リポジトリ向けの標準を一元管理できる。
- public に出せない連絡先や運用フローを含められる。
- 社員が新規 repo を作るときの標準資料として使える。
- 社内向け `SECURITY.md` として、脆弱性報告の一次受付、triage、escalation の流れを明確にできる。
- 社内の開発者が「どこに、何を、どの粒度で報告すればよいか」を統一できる。
- private / internal repository で起きた問題を public issue に出さず、社内の標準ルートへ誘導できる。
- security manager、SRE、platform team、法務、広報などの社内連携手順を書ける。
- severity 判定、初動期限、顧客影響確認、CVE 判断など、外部公開しにくい実務情報を扱える。

### デメリット

- 外部 contributor や security researcher には見えない。
- public OSS の `SECURITY.md` としては不十分。
- GitHub.com の通常 Organization では internal visibility が使えない場合がある。
- Enterprise Server では、internal `.github` から issue / pull request template が組織全体適用されない制約がある。

### 内部公開に向く内容

- 社内 escalation path
- セキュリティ連絡先
- チーム別 ownership
- repository 作成基準
- branch protection の標準
- ruleset 運用
- secret scanning / code scanning / Dependabot の標準
- private vulnerability reporting の有効化方針
- 社内向け vulnerability report template
- severity / priority の判定基準
- advisory 公開前の承認フロー
- 顧客影響がある場合の連絡判断

### 具体的な運用

1. Enterprise 配下の Organization に internal `.github` を作る。
2. 社内向け `SECURITY.md` を置く。
3. 社内向け `CONTRIBUTING.md` に開発プロセスを書く。
4. `docs/` に repository 作成、権限管理、security feature の運用資料を置く。
5. 外部公開用 `.github` が別に必要な場合は public `.github` と internal `.github` を分ける。
6. 社内リンク、Slack channel、当番表などは internal 側にのみ書く。
7. Organization owner / security manager / platform team を CODEOWNERS で review 必須にする。

### 注意点

internal `.github` は社内向けには便利ですが、外部に見せるべき security policy の代わりにはなりません。

public repository を運用する企業では、外部向け public `.github` と、社内向け internal / private docs を分ける構成が現実的です。

## 非公開で運用する場合

`.github` リポジトリを private にする運用です。

ただし、default community health files の配布元としては private `.github` は適していません。GitHub Docs でも private `.github` は default community health files の適用対象としてサポートされないと説明されています。

### 向いているケース

- `.github` の特殊機能を使いたいのではなく、単なる内部資料置き場として使いたい。
- アクセスできるメンバーを限定したい。
- セキュリティ運用の詳細、監査資料、内部手順を置きたい。

### メリット

- 閲覧者を明確に制限できる。
- 詳細な内部手順を書ける。
- 外部公開リスクを下げられる。
- セキュリティチームや platform team の作業資料置き場として使いやすい。
- `SECURITY.md` を「社内専用の脆弱性対応 Runbook」として運用できる。
- 外部には出せない報告先、当番、判断基準、緊急連絡手順を具体的に書ける。
- 未公開サービス、社内基盤、顧客別運用、契約上の対応条件などを含めた実務手順を管理できる。
- 報告受付から triage、修正、検証、リリース、事後レビューまでの流れを 1 か所に集約できる。
- 監査やインシデント後の振り返りで「その時点の手順」を Git history として追跡できる。

### デメリット

- Organization-wide default community health files としては期待どおり機能しない。
- 外部利用者に `SECURITY.md` が見えない。
- Organization profile には使えない。
- public repo の issue / PR 体験を改善できない。
- `.github` という名前により、特殊 repo としての期待と実態がズレやすい。
- 外部の security researcher は報告方法を見つけられないため、別途 public な案内が必要になる。
- public repository の Security タブや issue 作成導線で、非公開の `SECURITY.md` を外部向け案内として使うことはできない。

### 具体的な運用

1. private `.github` を default community health files 用には使わない。
2. private にしたい資料は、別名の private repo に置くことを検討する。
3. 例: `security-operations`, `github-governance`, `developer-platform-docs`
4. public `.github` には外部向け最小限の案内だけ置く。
5. private repo には詳細な運用資料、内部 escalation、監査手順を置く。

## `SECURITY.md` の公開範囲別メリット

`SECURITY.md` は「外部に見せる報告窓口」としても、「社内の対応手順」としても使えます。ただし、同じファイルに両方を書きすぎると、公開情報と内部情報が混ざります。

### public `SECURITY.md`

目的:

- 外部利用者、OSS contributor、security researcher に報告方法を知らせる。
- public issue に脆弱性詳細を書かないよう誘導する。
- Private vulnerability reporting や外部受付窓口へ案内する。

運営メリット:

- 外部報告の入口が明確になる。
- 脆弱性詳細が public issue に出るリスクを下げられる。
- OSS / public product の信頼性を高められる。
- 各 public repository に個別 `SECURITY.md` がなくても、最低限の案内を継承できる。

書く内容:

- 報告方法
- 報告に含めてほしい情報
- public issue に書かないでほしい内容
- 対象範囲
- 大まかな対応方針

書かない内容:

- 社内の詳細手順
- 個人名
- 内部 SLA
- 内部 escalation
- 具体的な監視・検知条件

### internal `SECURITY.md`

目的:

- enterprise member 向けに、社内共通の脆弱性報告・対応手順を示す。
- private / internal repository の開発者が迷わず報告できるようにする。

運営メリット:

- 社内の報告フォーマットを統一できる。
- security team への連絡経路を明確にできる。
- repository owner、security manager、platform team の役割分担を書ける。
- public には出せない社内システム名、連絡先、対応基準を書ける。
- 新規チームや新規 repository でも、最低限の security intake を揃えられる。

書く内容:

- 社内報告先
- severity 判定
- 初動期限
- escalation path
- 関係チーム
- advisory / CVE / 顧客連絡の判断基準

注意点:

- 外部報告者には見えない。
- public repository の外部向け security policy の代わりにはならない。
- issue / pull request template の組織全体適用には制約がある環境がある。

### private `SECURITY.md`

目的:

- 限定チーム向けの security operations runbook として使う。
- インシデント対応、監査、脆弱性 triage の詳細を管理する。

運営メリット:

- 最も詳細な内部手順を書ける。
- 当番、連絡先、判断基準、顧客別対応などを扱える。
- Git history で変更履歴を追跡できる。
- 権限を security team や platform leadership に限定できる。
- 外部公開事故のリスクを下げられる。

書く内容:

- triage checklist
- severity matrix
- escalation runbook
- incident response 連携
- patch release 手順
- advisory 公開手順
- 顧客影響調査
- 監査ログ確認手順

注意点:

- default community health files としての効果は期待しない。
- 外部・社内全体への案内には別資料が必要。
- `.github` という名前にこだわらず、`security-operations` のような明示的な repo 名にしたほうが誤解が少ない場合がある。

### 推奨分離

企業では、次の 3 層に分けるのが扱いやすいです。

| 層 | visibility | SECURITY.md の役割 |
| --- | --- | --- |
| 外部向け | public | 外部報告者向けの入口 |
| 社内標準 | internal | 社内開発者向けの報告・triage 標準 |
| 運用詳細 | private | security team 向け runbook |

この分離により、外部に必要な情報は公開し、社内に必要な情報は共有し、限定すべき情報は閉じることができます。

## 企業での推奨構成

public repository を持つ企業では、次の構成を推奨します。

| リポジトリ | visibility | 役割 |
| --- | --- | --- |
| `.github` | public | 外部向け profile / SECURITY / contribution / templates |
| `.github-private` | private | member-only Organization profile |
| `github-governance` | internal or private | 社内 GitHub 運用ルール |
| `security-operations` | private | セキュリティ対応の詳細手順 |

## `.github` に置くべきもの

public `.github`:

- 外部向け `SECURITY.md`
- 外部向け `CONTRIBUTING.md`
- `CODE_OF_CONDUCT.md`
- `SUPPORT.md`
- public repo 用 issue / PR template
- `profile/README.md`

internal / private docs:

- 詳細な triage 手順
- vulnerability response SLA
- 社内連絡先
- security manager の当番表
- incident response 手順
- secret scanning alert の対応手順
- GitHub Advanced Security の設定標準

## SECURITY.md の運用

### public `.github/SECURITY.md`

外部から読まれる前提で、次だけを書くのが安全です。

- public issue に脆弱性詳細を書かないこと
- Private vulnerability reporting が有効な repo ではフォームを使うこと
- 報告時に含めてほしい情報
- 対象外の範囲
- 返信の大まかな方針

### 内部資料

内部資料には、次を書くことができます。

- triage owner
- severity 判定基準
- 初動期限
- escalation path
- legal / PR / customer support 連携
- CVE 申請判断
- advisory 公開判断
- patch release 手順

## Private vulnerability reporting の運用

public repository では、可能な限り Private vulnerability reporting を有効にします。

理由:

- 報告者が public issue に詳細を書かずに済む。
- GitHub 上で非公開にやり取りできる。
- draft security advisory に移行しやすい。
- temporary private fork で修正作業を進められる。

運用手順:

1. public repository の Settings を開く。
2. Advanced Security を開く。
3. Private vulnerability reporting を有効化する。
4. repository admin / security manager が通知を受け取れるようにする。
5. `SECURITY.md` から Private vulnerability reporting の手順へ誘導する。

## 変更管理

`.github` は Organization 全体に影響するため、通常のドキュメント repo より厳しめに運用します。

推奨:

- default branch への直接 push を禁止する。
- pull request 必須にする。
- CODEOWNERS で review 必須にする。
- security policy 変更は security team review 必須にする。
- issue / PR template 変更は developer productivity team review 必須にする。
- profile 変更は developer relations / communications review を通す。

## レビュー観点

公開 `.github` の review では、次を確認します。

- 外部公開してよい情報だけか。
- 内部 URL、個人名、Slack channel が含まれていないか。
- security report の導線が明確か。
- public issue に脆弱性詳細を書かない注意があるか。
- リンク切れがないか。
- 各 repo の固有 policy と矛盾しないか。

内部 `.github` / governance docs の review では、次を確認します。

- 現実の運用と一致しているか。
- owner が明確か。
- escalation path が古くないか。
- 退職者や異動者の名前が残っていないか。
- audit / compliance で参照できる粒度か。

## 定期点検

最低でも四半期に 1 回、次を確認します。

- `SECURITY.md` の報告導線
- Private vulnerability reporting の有効化状況
- issue / PR template の利用状況
- CODEOWNERS の owner
- docs 内のリンク
- Organization profile の内容
- public に出してはいけない情報の混入

## 判断基準

迷った場合は、次の基準で判断します。

| 質問 | yes の場合 |
| --- | --- |
| 外部利用者や OSS contributor が読む必要があるか | public `.github` に置く |
| 脆弱性報告者が読む必要があるか | public `.github/SECURITY.md` に置く |
| 社員全員が読む必要があるか | internal repo に置く |
| 限定チームだけが読むべきか | private repo に置く |
| 攻撃者に有利な詳細を含むか | public には置かない |
| 個人名や内部連絡先を含むか | public には置かない |

## 推奨結論

企業運用では、単一の `.github` リポジトリにすべてを入れないほうが安全です。

推奨は次の分離です。

1. public `.github`: 外部向けの最低限の共通ポリシー。
2. internal governance repo: 社内向け GitHub 標準。
3. private security operations repo: セキュリティ対応の詳細手順。

この分離により、外部から必要な情報は見える状態にしつつ、内部運用の詳細は守れます。

## 参考資料

- Creating a default community health file
  https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file
- Customizing your organization's profile
  https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile
- About repositories
  https://docs.github.com/enterprise-cloud@latest/repositories/creating-and-managing-repositories/about-repositories
- Setting repository visibility
  https://docs.github.com/en/enterprise-cloud@latest/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/setting-repository-visibility
