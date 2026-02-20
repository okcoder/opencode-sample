# 技術仕様書

---

## 1. システム構成

| コンポーネント | 技術 | バージョン |
|--------------|------|-----------|
| フロントエンド | Electron | v40.2.1 |
| メイン言語 | TypeScript | ES2020 |
| テスト | Jest | - |
| ビルドツール | electron-builder | - |
| Node.js管理 | Volta | v22.12.0 |

---

## 2. アーキテクチャ

### 2.1 プロセス構成

- **Main Process**: Electron メインプロセス（ウィンドウ管理、Native API呼び出し）
- **Preload Script**: セキュリティ境界のbridge（contextBridge使用）
- **Renderer Process**: HTML/CSS/JSによるUI描画

### 2.2 モジュール構成

```
src/
├── main.ts        # メインプロセスエントリ
├── preload.ts     # Preload script
└── index.html    # Rendererエントリ
```

---

## 3. 技術制約

- Google Calendar API・OAuth認証に依存しないクライアントサイド実装
- タイムゾーン: Asia/Tokyo（デフォルト）
- ターゲット: Windows/macOS/Linux

---

## 4. カレンダー取得方式

### 4.1 概要

Google Calendar 公開URLの埋め込み機能を利用し、裏で呼び出されるevents APIのレスポンスからイベント情報を取得する。

```
https://calendar.google.com/calendar/embed?src=${email}&ctz=Asia%2FTokyo
```

### 4.2 実装手法

1. 埋め込みiframeとして公開URLを Electron WebView で読み込み
2. WebView の devtools protocol を使用し、events API のレスポンスを横取り
3. 取得したJSONからイベント情報を抽出・正規化

### 4.3 カレンダーAdapter設計

```
┌─────────────────────┐
│   CalendarService   │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  CalendarAdapter    │  ← Interface定義
│  (Abstract Class)  │
└──────────┬──────────┘
           │
    ┌──────┴──────┐
    │             │
┌───▼───┐    ┌────▼────┐
│Google │    │ Outlook │
│Adapter│    │ Adapter │
└───────┘    └─────────┘
```

### 4.4 Adapter Interface

```typescript
interface CalendarAdapter {
  getEvents(start: Date, end: Date): Promise<CalendarEvent[]>;
  validateConfig(config: CalendarConfig): boolean;
}
```

### 4.5 対応予定Provider

| 優先度 | Provider | 方式 |
|--------|----------|------|
| P0 | Google Calendar | 埋め込みAPI横取り |
| P1 | Outlook Calendar | 予定 |

---

## 5. モジュール構成

```
src/
├── main.ts              # メインプロセスエントリ
├── preload.ts          # Preload script
├── index.html          # Rendererエントリ
├── core/
│   └── CalendarService.ts    # カレンダー操作の中核
├── adapters/
│   ├── CalendarAdapter.ts    # 基底抽象クラス
│   ├── GoogleAdapter.ts       # Google Calendar実装
│   └── OutlookAdapter.ts     # Outlook Calendar実装
└── types/
    └── calendar.ts           # 型定義
```
