# Serena MCP Setup Guide for Windows
# Windows環境でのSerena MCPセットアップ完全ガイド

## 📌 概要
このガイドは、Windows環境でClaude CodeにSerena MCPサーバーを設定するための汎用的な手順書です。
どなたでも自分の環境に合わせて設定できるよう設計されています。

## 🎯 このガイドの使い方
1. このドキュメント全体をClaude Codeにコピー＆ペースト
2. `[YOUR_PROJECT_PATH]`を実際のプロジェクトパスに置換
3. `[YOUR_USERNAME]`を自分のWindowsユーザー名に置換
4. AIアシスタントが自動的に設定を実行

## 📋 前提条件
- Windows 10/11
- Claude Code (Claude CLI) インストール済み
- 管理者権限は不要

## 🚀 セットアップ手順

### 準備: 必要な情報の確認
以下のコマンドで自分の環境情報を確認してください：

```powershell
# Windowsユーザー名の確認
echo %USERNAME%

# プロジェクトフォルダのパス確認
cd [設定したいプロジェクトフォルダ]
echo %CD%
```

**記入欄**（実際の値に置換してください）：
```
YOUR_USERNAME: [あなたのWindowsユーザー名]
YOUR_PROJECT_PATH: [プロジェクトのフルパス 例: C:\Projects\MyApp]
```

### ステップ1: Python 3.12のインストール

#### 1.1 インストール状況の確認
```bash
winget list | findstr Python.3.12
```

#### 1.2 Python 3.12のインストール（未インストールの場合）
```bash
winget install Python.Python.3.12
```

**注意**: Microsoft Store版のPythonでは正しく動作しません。必ずwinget版を使用してください。

#### 1.3 Pythonパスの確認
```bash
# 通常のインストール先
dir "C:\Users\%USERNAME%\AppData\Local\Programs\Python\Python312\python.exe"

# 確認できない場合は以下で検索
where python
```

### ステップ2: UV (Astral UV) ツールのインストール

```bash
# uvツールをインストール
"C:\Users\%USERNAME%\AppData\Local\Programs\Python\Python312\python.exe" -m pip install --user uv

# インストール確認
"C:\Users\%USERNAME%\AppData\Local\Programs\Python\Python312\python.exe" -m uv --version
```

### ステップ3: Serena起動用バッチファイルの作成

プロジェクトフォルダに`serena_mcp.bat`を作成します。

**ファイル名**: `serena_mcp.bat`
**保存場所**: `[YOUR_PROJECT_PATH]\serena_mcp.bat`

```batch
@echo off
:: Serena MCP Server Launcher for Windows
:: 自動的にPythonパスを検出して起動

set PYTHON_PATH=C:\Users\%USERNAME%\AppData\Local\Programs\Python\Python312\python.exe

if not exist "%PYTHON_PATH%" (
    echo Error: Python 3.12 not found at expected location
    echo Please check Python installation path
    pause
    exit /b 1
)

"%PYTHON_PATH%" -m uv tool run --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project %1
```

### ステップ4: プロジェクト用MCP設定ファイルの作成

**ファイル名**: `.mcp.json`
**保存場所**: `[YOUR_PROJECT_PATH]\.mcp.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "[YOUR_PROJECT_PATH]"
      ]
    },
    "serena": {
      "command": "[YOUR_PROJECT_PATH]\\serena_mcp.bat",
      "args": [
        "."
      ]
    }
  }
}
```

**重要**: 
- `[YOUR_PROJECT_PATH]`を実際のパスに置換
- パスにスペースが含まれる場合でも、JSONでは追加のエスケープは不要
- バックスラッシュは`\\`とエスケープする

### ステップ5: Claude CLIへの登録

```bash
# プロジェクトフォルダに移動
cd [YOUR_PROJECT_PATH]

# 既存のSerena設定がある場合は削除
claude mcp remove serena 2>nul

# Serenaを新規登録
claude mcp add serena "[YOUR_PROJECT_PATH]\serena_mcp.bat" "."

# 登録確認
claude mcp list
```

### ステップ6: Claude Codeの再起動と確認

1. **現在のClaude Codeセッションを終了**
   ```
   exitコマンドまたはCtrl+C
   ```

2. **プロジェクトフォルダで再起動**
   ```bash
   cd [YOUR_PROJECT_PATH]
   claude
   ```

3. **接続確認（Claude Code内で実行）**
   ```
   /mcp
   ```

## ✅ 動作確認

### 方法1: MCPコマンドで確認
Claude Code内で:
```
/mcp
```
出力例:
```
serena: ✓ Connected
filesystem: ✓ Connected
```

### 方法2: CLIで確認
```bash
claude mcp list
```
出力例:
```
serena: [YOUR_PROJECT_PATH]\serena_mcp.bat . - ✓ Connected
```

### 方法3: Webダッシュボード
ブラウザで開く:
```
http://127.0.0.1:24285/dashboard/index.html
```

## 🔧 トラブルシューティング

### エラー1: "Python not found"
**原因**: Pythonパスが異なる
**解決策**:
1. `where python`でPythonの場所を確認
2. `serena_mcp.bat`のPYTHON_PATHを修正

### エラー2: "uv: No module named uv"
**原因**: uvツールが未インストール
**解決策**:
```bash
python -m pip install --user uv
```

### エラー3: "Failed to connect"
**原因**: 複数の可能性
**解決策**:
1. Windows Defenderの通知を確認（許可が必要）
2. パスに日本語が含まれていないか確認
3. プロジェクトパスのスペースを確認
4. Claude Codeを完全に再起動

### エラー4: "Connection closed"
**原因**: バッチファイルの実行エラー
**解決策**:
1. バッチファイルを直接実行してエラーを確認:
   ```bash
   [YOUR_PROJECT_PATH]\serena_mcp.bat .
   ```
2. 出力されるエラーメッセージを確認

### エラー5: ポート競合
**原因**: ポート24285が使用中
**解決策**:
```bash
# 使用中のポートを確認
netstat -ano | findstr :24285

# プロセスを特定して終了
taskkill /PID [プロセスID] /F
```

## 📝 カスタマイズオプション

### 異なるPythonバージョンを使用
```batch
:: serena_mcp.batを編集
set PYTHON_PATH=C:\Python39\python.exe
```

### プロキシ環境での設定
```batch
:: 環境変数を追加
set HTTP_PROXY=http://proxy.example.com:8080
set HTTPS_PROXY=http://proxy.example.com:8080
```

### 複数プロジェクトの管理
各プロジェクトフォルダに独立した設定:
1. プロジェクトAに`.mcp.json`と`serena_mcp.bat`
2. プロジェクトBに別の`.mcp.json`と`serena_mcp.bat`
3. 各フォルダでclaudeを起動すると自動的に適切な設定を読み込み

## 🌟 ベストプラクティス

1. **プロジェクトごとの設定**
   - 各プロジェクトに独立した`.mcp.json`を配置
   - グローバル設定よりもプロジェクト設定を優先

2. **セキュリティ**
   - MCPサーバーはローカルのみでアクセス可能
   - 信頼できないMCPサーバーは追加しない

3. **パフォーマンス**
   - 不要なMCPサーバーは無効化
   - 大規模プロジェクトでは検索範囲を制限

## 📚 追加リソース

- **Serena公式リポジトリ**: https://github.com/oraios/serena
- **Claude Code ドキュメント**: https://docs.anthropic.com/claude-code
- **MCP仕様**: https://modelcontextprotocol.io/

## 🤝 コミュニティサポート

問題が解決しない場合:
1. このガイドのトラブルシューティングを再確認
2. Serena GitHubのIssuesを確認
3. Claude Codeのドキュメントを参照

## 📌 クイックコマンドリファレンス

```bash
# Python確認/インストール
winget list | findstr Python
winget install Python.Python.3.12

# UV確認/インストール  
python -m pip install --user uv
python -m uv --version

# MCP管理
claude mcp list                     # 一覧表示
claude mcp remove serena            # 削除
claude mcp add serena "path" "."    # 追加

# トラブルシューティング
where python                        # Pythonパス確認
echo %USERNAME%                     # ユーザー名確認
echo %CD%                          # 現在のパス確認
netstat -ano | findstr :24285      # ポート確認
```

## 📄 チェックリスト

設定完了の確認:
- [ ] Python 3.12 (winget版) インストール済み
- [ ] uvツールインストール済み
- [ ] `serena_mcp.bat`作成済み
- [ ] `.mcp.json`作成済み（パス置換済み）
- [ ] Claude CLIに登録済み
- [ ] Claude Code再起動済み
- [ ] `/mcp`で"Connected"表示確認
- [ ] ダッシュボード表示確認

---

**Document Version**: 1.0.0  
**Last Updated**: 2025-08-20  
**Compatible with**: Windows 10/11, Claude Code, Serena MCP Server  
**License**: Public Domain - コミュニティでの自由な使用・改変・配布が可能です

## このガイドへの貢献

改善案やエラー報告は以下へ:
- Serena Issues: https://github.com/oraios/serena/issues
- ガイドの改善: コミュニティフォーラムで共有

---

**💡 ヒント**: このガイドをClaude Codeに直接貼り付けて、`[YOUR_USERNAME]`と`[YOUR_PROJECT_PATH]`を置換すれば、AIが自動的にセットアップを実行します！