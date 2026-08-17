# 📰 一人賛否ニュース Archive

ChatGPTで毎日19:00 JSTに生成している  
「一人賛否ニュース」の自動アーカイブ。

ニュース本文だけでなく、生成ルールもGitHub上で管理し、
過去記事との重複・継続ニュースの差分確認にも利用する。

## 📁 構成

- `daily/`
  - 日ごとのニュース本文
  - `YYYY/MM/YYYY-MM-DD.md`
- `docs/`
  - ニュース生成ルール
  - `daily_news_rules.md`

## 🤖 運用

毎日19:00 JSTにChatGPT Scheduled Taskが実行。

1. `docs/daily_news_rules.md` を読み込む
2. 直近の `daily/` を確認
3. 最新ニュースを調査・生成
4. ChatGPTに配信
5. 同じ本文をGitHubへ保存

## 📅 Archive

2026-07-15 から運用開始。

---

> ニュースは事実確認を重視しつつ、  
> 「賛」と「ただぁ！」の両面から見る。