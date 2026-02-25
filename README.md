# draw-io

[日本語版はこちら](#draw-io-1)

A Claude Code plugin for creating, reading, and editing draw.io (.drawio) diagrams.

## Features

Create, read, and edit diagrams in draw.io format (.drawio).

**Supported Diagram Types (examples):**
- Architecture diagrams
- ER diagrams
- Flowcharts
- Wireframes
- Sequence diagrams
- Class diagrams
- Domain model diagrams
- Object diagrams

## Prerequisites

### draw.io Desktop App

draw.io desktop app must be installed.

**macOS:**

```bash
brew install --cask drawio
```

**Windows:**

```powershell
winget install -e --id JGraph.Draw
```

### AI Coding Tool

The following tools have been tested and confirmed to work:

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- [Cursor](https://www.cursor.com/)

Any tool that supports Agent Skills should work, but has not been tested beyond the above.

## Installation

### Claude Code (Terminal)

Launch `claude` in your terminal and run the following command.

```
/plugin marketplace add little-hands/claude-drawio-skill
/plugin install draw-io@claude-drawio-skill
```

### For Cursor Users

Clone this repository and create a symbolic link in Cursor's global skills directory (`~/.cursor/skills/`).

1. Clone the repository (to any location):

   ```bash
   git clone https://github.com/little-hands/claude-drawio-skill.git
   ```

2. Create a symbolic link in the global skills directory:

   ```bash
   mkdir -p ~/.cursor/skills
   ln -s {claude-drawio-skill}/skills/draw-io ~/.cursor/skills/draw-io
   ```

   Replace `{claude-drawio-skill}` with the absolute path to the cloned repository.

   This makes the draw-io skill available across all projects.

## Usage Examples

```text
"Create an architecture diagram"
"Create an ER diagram: User(id, name, email) → Order(id, user_id, total)"
"Explain what's in the diagram"
"Add a user registration flow to the flowchart"
"Add a concrete example of this model to the same sheet"
```

## draw.io Tips & Recommended Settings

**Light Mode:**

- In the draw.io app settings, select "Extras → Appearance → Light".
- Dark mode causes color inversion issues, so Light mode is recommended.

**Autosave:**

- Enable "Extras → Autosave".
- When Claude Code edits a .drawio file, changes will be automatically reflected in the draw.io app.

**Google Drive Integration:**

- Save .drawio files to Google Drive and sync locally with [Google Drive for Desktop](https://www.google.com/drive/download/) for convenient team sharing and version management.

## Support & Contributing

- Bug reports and feature requests are welcome via [Issues](https://github.com/little-hands/claude-drawio-skill/issues)
- This plugin is provided as-is with no warranty (MIT License)

---

# draw-io

[English version is here](#draw-io)

Claude Codeで draw.io (.drawio) ダイアグラムを作成・読み取り・編集するためのプラグインです。

## 機能

draw.io形式（.drawio）のダイアグラムを作成・読み取り・編集できます。

**対応ダイアグラム種別（例）：**
- アーキテクチャ図
- ER図
- フローチャート
- ワイヤーフレーム
- シーケンス図
- クラス図
- ドメインモデル図
- オブジェクト図

## 前提条件

### draw.io デスクトップアプリ

draw.io デスクトップアプリのインストールが必要です。

**macOS:**

```bash
brew install --cask drawio
```

**Windows:**

```powershell
winget install -e --id JGraph.Draw
```

### AI コーディングツール

以下のツールで動作確認済みです：

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- [Cursor](https://www.cursor.com/)

Agent Skillsに対応しているツールであれば利用可能と想定されますが、上記以外は動作未確認です。

## インストール

### Claude Code（ターミナル）

ターミナルで `claude` を起動し、以下のコマンドを実行してください。

```
/plugin marketplace add little-hands/claude-drawio-skill
/plugin install draw-io@claude-drawio-skill
```

### Cursor ユーザー向け

このリポジトリをクローンし、Cursorのグローバルスキルディレクトリ（`~/.cursor/skills/`）にシンボリックリンクを作成してください。

1. リポジトリをクローン（任意の場所に）：

   ```bash
   git clone https://github.com/little-hands/claude-drawio-skill.git
   ```

2. グローバルスキルディレクトリにシンボリックリンクを作成：

   ```bash
   mkdir -p ~/.cursor/skills
   ln -s {claude-drawio-skill}/skills/draw-io ~/.cursor/skills/draw-io
   ```

   `{claude-drawio-skill}` はクローンしたリポジトリの絶対パスに置き換えてください。

   これにより、すべてのプロジェクトでdraw-ioスキルが利用可能になります。

## 使用例

```text
"アーキテクチャ図を作成して"
"ER図を作成して：User(id, name, email) → Order(id, user_id, total)"
"ダイアグラムの内容を説明して"
"フローチャートにユーザー登録のフローを追加して"
"このモデルの具体例を同じシートに追加して"
```

## draw.io Tips・推奨設定

**ライトモード：**

- draw.ioアプリの設定から、「Extras(その他) → Appearance(外観) → Light(ライト)」を選択してください。
- ダークモードは色の反転が発生して見づらくなるため、ライトモードを推奨します。

**自動保存：**

- 「Extras(その他) → Autosave(自動保存)」を有効にしてください。
- Claude Codeが.drawioファイルを編集すると、変更がdraw.ioアプリに自動的に反映されます。

**Google Drive 連携：**

- .drawioファイルをGoogle Driveに保存し、[Google Drive for Desktop](https://www.google.com/drive/download/)でローカルに同期することで、チームでの共有やバージョン管理が便利になります。

## サポート・貢献

- バグ報告・機能要望は [Issues](https://github.com/little-hands/claude-drawio-skill/issues) へ
- 本プラグインは無保証で提供されます（MITライセンス）
