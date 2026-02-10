# OpenCode Sample

Electron + TypeScript + Volta プロジェクトのサンプルアプリケーションです。

## 必要条件

- Node.js v24.13.1 (Volta により自動管理)
- npm

## インストール

```bash
npm install
```

## 開発

アプリケーションを起動:

```bash
npm start
```

## ビルド

TypeScript をコンパイル:

```bash
npm run build
```

## テスト

テストを実行:

```bash
npm test
```

## パッケージング

 Electron アプリケーションをパッケージ化:

```bash
npm run build
npm run build:package
```

生成されたパッケージは `dist-electron` ディレクトリに出力されます。

- **macOS**: `.dmg` ファイル
- **Windows**: `.exe` ファイル (NSIS)
- **Linux**: `.AppImage` ファイル

## ライセンス

ISC
