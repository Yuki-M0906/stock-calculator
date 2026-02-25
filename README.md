# 理論株価 Calculator

PBR方式 + PER方式で理論株価を算出するシングルページWebアプリケーションです。
マネックス証券「銘柄スカウター」の理論株価計算方式に準拠しています。

**Live Demo:** https://yuki-m0906.github.io/stock-calculator/

## 機能

### PBR基準（過去75日PBR統計バンド方式）
- 直近75営業日の終値からPBR分布を統計分析
- PBR平均値による理論株価を算出
- +2σ（上値目途）/ -2σ（下値目途）のレンジを表示

### PER基準（EPS複利成長 × 想定PER方式）
- EPS × (1 + 成長率)^年数 × 想定PER で将来の理論株価を算出
- 成長率・予測年数はスライダーで調整可能
- EPS / PER / BPS の手動入力にも対応

### EPS成長率推定
- 過去3年 / 5年の EPS CAGR を自動計算
- アナリスト予想成長率の自動取得
- ワンクリックで成長率スライダーに反映

## 対応銘柄

| 市場 | 入力形式 | 例 |
|------|----------|------|
| 日本株（東証） | 4桁コード（自動で `.T` 付加） | `7203`, `2914`, `9984` |
| 米国株 | ティッカーシンボル | `AAPL`, `MSFT`, `NVDA` |

## 技術構成

```
index.html          … フロントエンド（単一HTMLファイル、依存ライブラリなし）
worker.js           … Cloudflare Worker（Yahoo Finance APIプロキシ）
```

- **フロントエンド**: Vanilla HTML/CSS/JS（単一ファイル、ビルド不要）
- **データソース**: Yahoo Finance API（`/v8/finance/chart`, `/v10/finance/quoteSummary`）
- **プロキシ**: Cloudflare Worker（CORS回避 + Crumb認証処理）
- **ホスティング**: GitHub Pages
- **オフライン**: 主要銘柄のモックデータ内蔵（API接続不可時のフォールバック）

### Cloudflare Worker（yahoo-proxy）

Yahoo Finance APIはCORS制限があるため、Cloudflare Workerをプロキシとして使用しています。

- セッションCookie取得（`fc.yahoo.com`）
- Crumbトークン取得（`/v1/test/getcrumb`）
- Cookie + Crumb付きでAPIリクエストを中継

## ローカル実行

```bash
# クローン
git clone https://github.com/Yuki-M0906/stock-calculator.git

# ブラウザで直接開く（ビルド不要）
open index.html
```

API接続できない環境では、以下の銘柄でモックデータが利用可能です:
`7203`（トヨタ）, `9984`（ソフトバンクG）, `6758`（ソニー）, `6861`（キーエンス）, `8306`（三菱UFJ）, `AAPL`, `MSFT`, `NVDA`

## 注意事項

- 本ツールは情報提供のみを目的としており、投資助言ではありません
- 投資判断はご自身の責任で行ってください
- Yahoo Finance APIの仕様変更により、データ取得に影響が出る場合があります

## バージョン履歴

| Version | Date | Changes |
|---------|------|---------|
| v2.1.0 | 2026-02-25 | 日本株EPS成長率修正（netIncomeフォールバック、earningsTrend参照パス追加） |
| v2.0.0 | 2026-02-25 | Crumb認証対応Worker、BPS手動入力、証券コード履歴保存 |
| v1.2.0 | 2026-02 | EPS成長率推定機能追加 |
| v1.1.0 | 2026-02 | Cloudflare Workerプロキシ導入 |
| v1.0.0 | 2026-02 | 初版（PBR/PER計算、モックデータ対応） |

## Author

Yuki
