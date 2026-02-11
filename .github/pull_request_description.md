**TypeScript + Electron + Volta プロジェクトの初期化と CI/CD 環境、テスト、パッケージング機能の追加**

### 概要
当PRでは、プロジェクトの初期化から CI/CD 環境、テスト環境、パッケージング機能までを包括的に追加しました。

### プロジェクト初期化
当PRでプロジェクトをゼロから構築しました：

**1. コア技術スタック**
- **TypeScript**: `tsconfig.json` で ES2020 target、CommonJS modules 設定
- **Electron**: v40.2.1、main process (`src/main.ts`)、preload script (`src/preload.ts`)、HTML entry point (`src/index.html`)
- **Volta**: Node.js v22.12.0 を `package.json` で管理

**2. ソースファイル構成**
```
src/
├── main.ts      (Electron main process - 31行)
├── preload.ts   (Preload script - 5行)
├── index.html   (HTML entry point - 15行)
└── sample.test.ts (Jest テスト - 5行)
```

### 追加された機能

**1. CI/CD ワークフロー (`.github/workflows/build-test.yml`)**
- pull_request イベントで自動実行
- Node.js v22.12.0 で依存関係をインストール
- `npm run build` で TypeScript をビルド
- `npm test` で Jest テストを実行

**2. テスト環境**
- Jest + ts-jest を導入 (`jest.config.js`)
- `src/sample.test.ts` にサンプルテストを追加

**3. パッケージング**
- electron-builder を導入
- `npm run build:package` でデスクトップアプリをビルド
- 出力先: `dist-electron/` ディレクトリ
- 対応プラットフォーム: Windows (.exe), macOS (.dmg), Linux

### 技術的決定事項

**Node.js バージョンについて**
当初 v24.13.1 を使用しましたが、Electron v40.2.1 との互換性問題により **v22.12.0** に変更しました。Electron v40.2.1 は Node v22.22.0 に基づいており、v24 では一部の依存関係が正常に動作しませんでした。

**ワークフローの構成**
`opencode.yml` (コメントトリガーのみ) と `build-test.yml` (build/test用) を分離しました。機能分離により、メンテンナンス性と可読性が向上します。

### 使用方法

```bash
npm install          # 依存関係インストール
npm run build        # TypeScript ビルド
npm test             # テスト実行
npm run build:package # アプリケーションをパッケージ化
```

### 変更ファイル (10ファイル、+8655行)
| ファイル | 変更内容 |
|---------|---------|
| `.github/workflows/build-test.yml` | CI/CD ワークフロー (+27行) |
| `package.json` | 依存関係とスクリプト設定 (+46行) |
| `jest.config.js` | Jest 設定 (+7行) |
| `src/sample.test.ts` | サンプルテスト (+5行) |
| `README.md` | ドキュメント (+57行) |
| `src/index.html` | HTML entry point (+15行) |
| `src/main.ts` | Electron main process (+31行) |
| `src/preload.ts` | Preload script (+5行) |
| `tsconfig.json` | TypeScript 設定 (+48行) |
| `package-lock.json` | 依存関係ロックファイル (+8414行) |

---

Run `npm start` to launch the Electron app once Node.js v22.12.0 is installed via Volta.

Closes #1
