# atlas-arm

[network detection / monitoring service Atlas](https://github.com/karam-ajaj/atlas) の arm64 対応 Docker イメージ。

## 概要

- 上流リポジトリ: `https://github.com/karam-ajaj/atlas`
- 対応アーキテクチャ: `linux/arm64`（公式イメージは arm64 が存在しないため自前ビルド）
- `patch.diff` により React UI をビルドして `/usr/share/nginx/html/` へ配置する修正を適用

GitHub Actions により毎日 1 回、上流リポジトリの最新ソースを clone → patch 適用 → 自動ビルドして GHCR へ push される。

## イメージ

```
ghcr.io/igarashi58426/atlas-arm:latest
```

`YYYY-MM-DD` 形式の日付タグも付与され、過去のビルドが蓄積されていきます（例: `2026-10-08`）。

## 使い方

```yaml
services:
  atlas:
    image: ghcr.io/igarashi58426/atlas-arm:latest
    container_name: atlas.lan
    restart: always
    expose: [80]
    environment:
      ATLAS_UI_PORT: "80"
```

## CI/CD

`.github/workflows/build.yml` のスケジュール（毎日 21:00 UTC / JST 06:00）で自動ビルド・push されます。
手動実行も `workflow_dispatch` に対応しています。

## パッチ

`patch.diff` は上流の `Dockerfile` に対する以下の修正です。

- React UI (`data/`) を `node:20` でビルドする `ui-builder` ステージを追加
- ビルド成果物 (`react-ui/dist/`) を `/usr/share/nginx/html/` へコピー

## ライセンス

MIT