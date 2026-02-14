# AGENTS.md - OpenCode 用設定ファイル

## プロジェクト構成図

```mermaid
graph TD
    Root[opencode-sample] --> src[src/]
    Root --> specs[specs/]
    Root --> github[.github/]
    Root --> config[設定ファイル]

    src --> main[main.ts<br/>Electron Main Process]
    src --> preload[preload.ts<br/>Security Bridge]
    src --> html[index.html<br/>Renderer]
    src --> test[sample.test.ts<br/>Jest Test]

    specs --> functional[functional/<br/>機能仕様]
    specs --> technical[technical/<br/>技術仕様]
    specs --> api[api/<br/>API仕様]

    functional --> func1[system_functional.md]
    technical --> tech1[architecture.md]
    api --> api1[references.md]

    github --> workflows[workflows/]
    workflows --> opencode[opencode.yml]
    workflows --> buildtest[build-test.yml]
```

## フォルダ構成

| フォルダ | 説明 |
|---------|------|
| `src/` | ソースコード（Electron Main/Preload/Renderer） |
| `specs/` | OpenSpec 仕様に基づく仕様書群 |
| `specs/functional/` | 機能仕様書 |
| `specs/technical/` | 技術仕様書 |
| `specs/api/` | API仕様書 |
| `.github/workflows/` | CI/CD ワークフロー |
| `dist/` | TypeScript コンパイル出力 |
| `dist-electron/` | パッケージング済みアプリ |

## タスク実行コマンド

```bash
# 依存関係インストール
npm install

# 開発サーバー起動
npm start

# ビルド
npm run build

# テスト実行
npm test

# パッケージング
npm run build:package
```
