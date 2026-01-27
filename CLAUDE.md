# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

AnsibleベースのmacOS開発環境セットアップ自動化リポジトリ。Homebrew、Homebrew Cask、Mac App Store (mas) を使用して、CLIツール、GUIアプリケーション、開発環境をプロビジョニングする。

## コマンド

### 初期セットアップ（新しいMacで1回実行）
```bash
sh ./setting.sh
```
Xcode CLIツール、Homebrew、OpenJDK、Ansible、community.generalコレクションをインストールする。

### Playbook実行
```bash
cd ./setup
ansible-playbook setup.yml -i inventory
```

### 構文チェック
```bash
cd ./setup
ansible-playbook --syntax-check setup.yml -i inventory
```

### 特定のRoleのみ実行（タグ指定）
```bash
cd ./setup
ansible-playbook setup.yml -i inventory --tags homebrew
ansible-playbook setup.yml -i inventory --tags homebrew_cask
ansible-playbook setup.yml -i inventory --tags mas
```

## アーキテクチャ

```
setup/
├── setup.yml          # メインPlaybook（全Roleを順番に実行）
├── inventory          # localhost対象
└── roles/
    ├── homebrew/      # CLIツール（brew install）
    │   ├── tasks/main.yml
    │   └── vars/main.yml   # パッケージリスト（name/state）
    ├── homebrew_cask/ # GUIアプリ（brew install --cask）
    │   ├── tasks/main.yml
    │   └── vars/main.yml   # パッケージリスト（name/state）
    ├── mas/           # Mac App Storeアプリ（mas install）
    │   ├── tasks/main.yml
    │   └── vars/main.yml   # パッケージリスト（id/name）
    └── nvm/           # Node.js LTS（nvm経由）
        └── tasks/main.yml
```

## パッケージの追加方法

- **Homebrewパッケージ**: `setup/roles/homebrew/vars/main.yml` に `{ name: package-name, state: present }` を追加
- **Homebrew Caskアプリ**: `setup/roles/homebrew_cask/vars/main.yml` に `{ name: app-name, state: present }` を追加
- **Mac App Storeアプリ**: `setup/roles/mas/vars/main.yml` に `{ id: APP_ID, name: App Name }` を追加（IDは `mas search "app name"` で検索）
