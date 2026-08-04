# ig-posts

Instagram 自動投稿（IG Harness）で使う画像の置き場。GitHub Pages で配信する。

## なぜ必要か

Instagram の Content Publishing API は画像ファイルを直接受け取らない。
**外部から取得できる公開 URL** しか受け付けないため、投稿する画像をどこかに
公開しておく必要がある。Cloudflare R2 を使わない構成にしたので、その代わりが
このリポジトリ。

## 公開 URL

```
https://yoshisgarden.github.io/ig-posts/images/day001.jpg
```

## 運用

1. nano banana で画像を生成し、`ClaudeWork\ig-harness-setup\inbox\` に保存
2. スクリプトが JPEG 変換 + 1080x1350 (4:5) に整形して `images/` へ配置
3. commit & push → Pages に反映
4. IG Harness の `POST /api/media-posts` で予約登録
5. Worker の cron（5分毎）が指定日時に投稿

## 注意

- **予約投稿が公開されるまで画像を削除しないこと。** cron が投稿する瞬間に
  URL が生きている必要がある。
- 投稿が完了した後は削除してよい。Instagram は取得時に自分のサーバーへ
  コピーするため、過去の投稿は消えない。
- 画像形式は **JPEG のみ**。PNG は API に拒否される。
- 比率は 4:5〜1.91:1。4:5 (1080x1350) がスマホ表示で最大になる。
