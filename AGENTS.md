# AGENTS.md - OpenCode 運用ガイド

## 目的
このリポジトリで OpenCode エージェントが作業する際の、共通ルールと最短の作業手順をまとめる。

## 対象範囲
- 対象: 本リポジトリ内のコード/ドキュメント/CI設定
- 対象外: 外部サービスの運用、プロダクションの操作、機密情報の取り扱い

## 事実（リポジトリに存在する情報）
### 構成（主要ディレクトリ）
| パス | 説明 |
| --- | --- |
| `src/` | Electron アプリのソース |
| `specs/functional/` | 機能仕様 |
| `specs/technical/` | 技術仕様 |
| `specs/api/` | API 仕様 |
| `.github/workflows/` | CI/CD |

### 主要コマンド（package.json と一致）
```bash
npm install
npm start
npm run build
npm test
npm run build:package
```

## 方針（作業スタイル）
- 変更理由が不明確な場合は、まず既存の実装/README/仕様を確認する
- 仕様は「事実」と「想定」を混在させない（根拠がない記述は “想定” と明記）
- コード変更時は関連する README / specs の整合性を確認する
- 自動生成物（`dist/`、`dist-electron/`）の修正は行わない

## 作業時の注意
- 機密情報（APIキー等）をコミットしない
- 破壊的操作（強制push、reset/checkout -- など）は行わない
- テストやビルドは必要時にのみ実行（CIで失敗した場合は再現優先）

## 更新ルール
- 仕様/構成/コマンドに変更があった場合は AGENTS.md も更新する
- レビュー時に「AGENTS.md の更新要否」を確認する
