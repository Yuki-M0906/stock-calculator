# 理論株価 Calculator / Theoretical Stock Price Calculator

PBR方式 + PER方式で理論株価を算出するシングルページWebアプリケーションです。
マネックス証券「銘柄スカウター」の理論株価計算方式に準拠しています。

A single-page web application that calculates theoretical stock prices using PBR (Price-to-Book Ratio) and PER (Price-to-Earnings Ratio) methods, based on the methodology used by Monex Securities' "Stock Scout" tool.

**Live Demo:** https://yuki-m0906.github.io/stock-calculator/

---

## 機能 / Features

### PBR基準 / PBR Method（過去75日PBR統計バンド方式 / 75-Day PBR Statistical Band）
- 直近75営業日の終値からPBR分布を統計分析
- PBR平均値による理論株価を算出
- +2σ（上値目途）/ -2σ（下値目途）のレンジを表示

- Statistically analyzes PBR distribution from the last 75 trading days' closing prices
- Calculates theoretical stock price based on mean PBR
- Displays range with +2σ (upper target) / -2σ (lower target)

### PER基準 / PER Method（EPS複利成長 × 想定PER方式 / EPS Compound Growth x Estimated PER）
- EPS × (1 + 成長率)^年数 × 想定PER で将来の理論株価を算出
- 成長率・予測年数はスライダーで調整可能
- EPS / PER / BPS の手動入力にも対応

- Calculates future theoretical price as EPS x (1 + growth rate)^years x estimated PER
- Growth rate and projection years adjustable via sliders
- Supports manual input for EPS / PER / BPS

### EPS成長率推定 / EPS Growth Rate Estimator
- 過去3年 / 5年の EPS CAGR を自動計算
- アナリスト予想成長率の自動取得
- ワンクリックで成長率スライダーに反映

- Automatically calculates 3-year / 5-year EPS CAGR
- Fetches analyst consensus growth estimates
- One-click application to the growth rate slider

---

## 対応銘柄 / Supported Stocks

| 市場 / Market | 入力形式 / Input Format | 例 / Example |
|---------------|------------------------|--------------|
| 日本株・東証 / Japan (TSE) | 4桁コード（自動で `.T` 付加）/ 4-digit code (`.T` auto-appended) | `7203`, `2914`, `9984` |
| 米国株 / US Stocks | ティッカーシンボル / Ticker symbol | `AAPL`, `MSFT`, `NVDA` |

---

## 技術構成 / Technical Architecture

```
index.html          … フロントエンド / Frontend（単一HTMLファイル / single HTML file, no dependencies）
worker.js           … Cloudflare Worker（Yahoo Finance APIプロキシ / API proxy）
```

- **フロントエンド / Frontend**: Vanilla HTML/CSS/JS（単一ファイル、ビルド不要 / single file, no build required）
- **データソース / Data Source**: Yahoo Finance API（`/v8/finance/chart`, `/v10/finance/quoteSummary`）
- **プロキシ / Proxy**: Cloudflare Worker（CORS回避 + Crumb認証処理 / CORS bypass + Crumb authentication）
- **ホスティング / Hosting**: GitHub Pages
- **オフライン / Offline**: 主要銘柄のモックデータ内蔵 / Built-in mock data for major stocks as fallback

### Cloudflare Worker（yahoo-proxy）

Yahoo Finance APIはCORS制限があるため、Cloudflare Workerをプロキシとして使用しています。

Yahoo Finance API has CORS restrictions, so a Cloudflare Worker is used as a proxy.

1. セッションCookie取得 / Fetch session cookies（`fc.yahoo.com`）
2. Crumbトークン取得 / Fetch crumb token（`/v1/test/getcrumb`）
3. Cookie + Crumb付きでAPIリクエストを中継 / Relay API requests with cookie + crumb

---

## ローカル実行 / Local Setup

```bash
# クローン / Clone
git clone https://github.com/Yuki-M0906/stock-calculator.git

# ブラウザで直接開く（ビルド不要）/ Open directly in browser (no build required)
open index.html
```

API接続できない環境では、以下の銘柄でモックデータが利用可能です。

Mock data is available for the following stocks when API connection is unavailable:

`7203`（トヨタ / Toyota）, `9984`（ソフトバンクG / SoftBank Group）, `6758`（ソニー / Sony）, `6861`（キーエンス / Keyence）, `8306`（三菱UFJ / MUFG）, `AAPL`, `MSFT`, `NVDA`

---

## 注意事項 / Disclaimer

- 本ツールは情報提供のみを目的としており、投資助言ではありません
- 投資判断はご自身の責任で行ってください
- Yahoo Finance APIの仕様変更により、データ取得に影響が出る場合があります

- This tool is for informational purposes only and does not constitute investment advice
- All investment decisions are made at your own risk
- Data availability may be affected by changes to the Yahoo Finance API

---

## バージョン履歴 / Version History

| Version | Date | Changes (JA) | Changes (EN) |
|---------|------|---------------|--------------|
| v2.1.0 | 2026-02-25 | 日本株EPS成長率修正（netIncomeフォールバック、earningsTrend参照パス追加） | Fixed EPS growth rate for JP stocks (netIncome fallback, earningsTrend path fix) |
| v2.0.0 | 2026-02-25 | Crumb認証対応Worker、BPS手動入力、証券コード履歴保存 | Crumb auth Worker, manual BPS input, ticker history (localStorage) |
| v1.2.0 | 2026-02 | EPS成長率推定機能追加 | Added EPS growth rate estimator |
| v1.1.0 | 2026-02 | Cloudflare Workerプロキシ導入 | Introduced Cloudflare Worker proxy |
| v1.0.0 | 2026-02 | 初版（PBR/PER計算、モックデータ対応） | Initial release (PBR/PER calculation, mock data) |

---

## Author

Yuki
