# GitHub Write Transport

> **AI / Automation 向け。`yuya228/news-archive` へのGitHub write手段だけを定義する正本。**

この文書はwrite transportのみを定義する。
一人賛否ニュース本文は `docs/daily_news_rules.md`、新聞は `docs/newspaper_rules.md` を正本とし、この文書で内容ルールを再定義しない。

## AI / Scheduled Task からのUTF-8 write

- AI / Scheduled Task からGitHubへ書き込む対象は、Markdown / HTML / JSONなどの**UTF-8テキスト**とする。
- Contents API の `create_file` / `update_file` / `delete_file` を自動writeの標準経路にしない。
- low-level Git Data APIを使い、**`blob -> tree -> commit -> ref update`** の順で書く。
- write前に必ず最新 `main` のref / commit / treeを取得する。
- 対象ファイルに関係する正本と最新stateを読み、staleな内容を上書きしないことを確認する。
- UTF-8内容をblob化し、最新mainのtreeをbaseに新treeを作る。
- 最新main HEADを親とするcommitを作成する。
- `main` refは `force=false` でfast-forward更新する。
- 同一変更で複数テキストファイルの整合が必要な場合は、**同じtree / commitへまとめて原子的に反映**する。
- ref更新直前に最新mainを再確認する。HEADが変化していた場合はblind retryせず、最新mainを取り直して再構成・再判定する。
- force updateは禁止。

## 新聞HTML

- `newspaper/YYYY/MM/YYYY-MM-DD.html` はUTF-8テキストなので、上記transportでwriteしてよい。
- daily本文を書き換えず、新聞用HTML・新聞ルールの変更だけを同一commitへまとめてよい。
- 外部写真を使う場合、HTMLには画像URLと出典を保持する。

## PDF / PNG / 写真などのバイナリ

- AI / Scheduled TaskはPDF / PNG / JPEG等のローカルバイト列を、上記UTF-8 transportへ混ぜない。
- バイナリをGitHubへ自動publishする必要が生じた場合は、DAILY-AI-BRIEFと同様に、UTF-8のpublish requestを作成し、GitHub Actions側が生成・検証・commitするhandoff方式を採用する。
- request作成はpublish完了ではない。workflow後にmainの成果物整合を確認して初めて成功と扱う。
