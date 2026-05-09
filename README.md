# VRCUdonSkills for Claude Code

VRChat UdonSharp 開発向けに整理した、Claude Code Plugin Marketplace 用 Skill 集と公開ガイド集です。

この公開版は、`VRCUdonSkills` で育てた知見のうち、外部配布に向く self-contained な Skill、テンプレート、汎用ガイドだけを切り出しています。内部の案件整理メモや参照元プロジェクト一覧、ローカル環境前提の知識ベース本体は含めていません。

## やること / やらないこと

- やること: VRChat UdonSharp 開発で使い回せる Skill 群を、Claude Code の Plugin Marketplace 形式で配布する
- やる内容: `vrc-udon-skills` plugin として 10 個の Skill と `guides/` を提供する。`/plugin marketplace add` で取り込める形に揃える
- やらないこと: 案件固有の分析資料、参照元プロジェクトのローカル絶対パス一覧、内部知識ベース本体、Codex (OpenAI) 専用ツール (apply_patch、VS Code Codex 拡張のチャット規約等) は配布しない

## インストール

Claude Code v2.x 以降が必要です。

```shell
/plugin marketplace add tyunta/VRCUdonSkills-for-Claude
/plugin install vrc-udon-skills@vrc-udon-skills
```

ローカルで試す場合:

```shell
/plugin marketplace add /path/to/VRCUdonSkills-for-Claude
/plugin install vrc-udon-skills@vrc-udon-skills
```

開発中の plugin として直接ロードしたい場合:

```bash
claude --plugin-dir /path/to/VRCUdonSkills-for-Claude/plugins/vrc-udon-skills
```

インストール後、`/help` で `vrc-udon-skills:*` の Skill 一覧が見えます。Claude は description を見て自動的に呼び出します。手動で呼ぶ場合は `/vrc-udon-skills:vrc-udon-core-module` のように namespace 付きで指定します。

## 収録 Skill

すべて `vrc-udon-skills` plugin 内で `/vrc-udon-skills:<skill-name>` として呼べます。

- `vrc-udon-project-docs`: project-owned な文書土台、環境再現文書、公式参照マップ、補助テンプレート
- `vrc-udon-core-module`: Builder First、Runtime / Editor 分離、再利用モジュールの基本骨格
- `vrc-udon-ui-menu`: ワールド空間 UI、画面遷移、Pickup と UI の干渉整理
- `vrc-udon-networking-rules`: Sync、Ownership、Network Event、Late Joiner のルール固定
- `vrc-udon-persistence-sync`: local / synced / Player Data / Player Object / Persistence の選定
- `vrc-udon-heavy-runtime`: 高負荷 runtime、段階実行、replay / rebuild の整理
- `vrc-udon-unity-mcp`: Unity MCP を Editor 支援として扱うための運用ルール
- `vrc-udon-fukuro-udon`: Fukuro Udon 導入時の既定差分
- `vrc-udon-data-lists`: DataList / Data Containers を明示 opt-in で扱う補助
- `vrc-udon-knowhow-transfer`: import / export ベースの know-how 受け渡し整理

## 収録 Guides

- `plugins/vrc-udon-skills/guides/texture-and-vram-optimization.md`: VRAM / ダウンロードデータ量最適化ガイド

## リポジトリ構造

```
.
├── .claude-plugin/
│   └── marketplace.json          # Plugin Marketplace 定義
├── plugins/
│   └── vrc-udon-skills/
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin manifest
│       ├── skills/
│       │   ├── vrc-udon-project-docs/
│       │   │   ├── SKILL.md
│       │   │   ├── references/
│       │   │   └── assets/
│       │   ├── vrc-udon-core-module/
│       │   ├── ... (全 10 skill)
│       └── guides/
│           └── texture-and-vram-optimization.md
├── README.md
└── LICENSE
```

各 skill の `references/` は SKILL.md から相対パスで参照される追加資料、`assets/` は導入先プロジェクトへ配置するテンプレート (`docs/NETWORKING_RULES.md` 雛形、`Editor/UdonSharpProgramAssetAutoGenerator.cs` 等) です。

## 公開方針

- 公開対象は、他プロジェクトへ持ち出せる Skill 本体、references、assets、汎用 guide に限定する
- 案件固有の分析ドキュメント、参照元プロジェクトのローカル絶対パス一覧、内部知識ベースの運用資料は含めない
- 各 Skill は単体でも読めるようにし、公開リポジトリ外の `docs/` を前提にしない
- `guides/` 配下の文書も、公開リポジトリ内だけで読めるようにする
- project-owned な仕様は Skill に固定せず、導入先プロジェクトの `CLAUDE.md` や `docs/` へ移す前提を維持する

## バージョン管理

`plugins/vrc-udon-skills/.claude-plugin/plugin.json` の `version` を更新したときだけ、ユーザー側に更新通知が届きます。日々の細かい改善で版を切らず、ある程度まとめて更新したい運用に合っています。`version` を消すと git commit SHA が版になり、毎 commit が新版扱いになります。

## 解説記事

この Skill セットの背景や設計意図は、次の note 記事で紹介しています。

- [VRChatのワールドギミックをCodexで開発するための基本Skills、公開しました](https://note.com/sechiro_vrc/n/n1705a7330377)

note 記事は元の Codex 版 (`VRCUdonSkills-for-Codex`) を前提に書いていますが、Skill 本体の設計意図は Claude Code 版でも同じです。

## ライセンス

このリポジトリ全体は MIT License です。

## 外部由来コンポーネント

この公開物には、外部由来の MIT ライセンスベースのテンプレートを一部含みます。

- `UdonSharpProgramAsset_AutoGeneration`
  - 利用箇所: `plugins/vrc-udon-skills/skills/vrc-udon-project-docs/assets/templates/Editor/UdonSharpProgramAssetAutoGenerator.cs`
  - 作者: `nemurigi`
  - 参照元: `https://gist.github.com/nemurigi/dea7c0a1fb94f7b9cf1c36481a459ded`
  - ライセンス: `MIT License`
  - 同梱ライセンス文面: `plugins/vrc-udon-skills/skills/vrc-udon-project-docs/assets/templates/LICENSES/UdonSharpProgramAssetAutoGenerator.MIT.txt`

外部由来部分を再配布するときは、上記の参照元 URL、著作権表示、MIT 許諾文を一緒に維持してください。
