# open-ci-example

Open CI（GitHub Actions self-hosted runner）の動作検証用リポジトリ

## 概要

このリポジトリは、Open CIのself-hosted runnerの動作を検証するための簡単なサンプルアプリケーションとCI/CDパイプラインを提供します。

## 構成

### アプリケーション

- `app.py` - 簡単な計算機アプリケーション（加算、減算、乗算、除算）
- `test_app.py` - アプリケーションのテストコード
- `requirements.txt` - Python依存関係

### GitHub Actions ワークフロー

このリポジトリには3つのワークフローが含まれています：

1. **Basic CI** (`.github/workflows/basic-ci.yml`)
   - 基本的なCI/CDパイプライン
   - コードのチェックアウト、依存関係のインストール、テスト実行、アプリケーション実行
   - Python 3.10を使用

2. **Matrix Build** (`.github/workflows/matrix-build.yml`)
   - マトリックスビルドの例
   - Python 3.8, 3.9, 3.10, 3.11の複数バージョンでテスト
   - 並列実行の動作確認

3. **Simple Echo Test** (`.github/workflows/simple-echo.yml`)
   - 最もシンプルなワークフロー
   - システム情報の表示のみ
   - ランナーの基本動作確認用

## セットアップ

### ローカルでの実行

```bash
# 依存関係のインストール
pip install -r requirements.txt

# テストの実行
pytest -v test_app.py

# アプリケーションの実行
python app.py
```

### Self-hosted Runnerの設定

1. リポジトリの Settings > Actions > Runners に移動
2. "New self-hosted runner" をクリック
3. 表示される手順に従ってランナーをセットアップ
4. ランナーが起動したら、ワークフローが自動的に実行されます

## ワークフローのトリガー

ワークフローは以下のイベントでトリガーされます：

- `push`: main/master ブランチへのプッシュ
- `pull_request`: main/master ブランチへのプルリクエスト
- `workflow_dispatch`: 手動実行（Actions タブから実行可能）

## 検証項目

このリポジトリで検証できる項目：

- ✅ Self-hosted runnerの基本動作
- ✅ ワークフローの実行
- ✅ 依存関係のインストール
- ✅ テストの実行
- ✅ マトリックスビルド
- ✅ 並列ジョブの実行
- ✅ システム情報の取得

## ライセンス

MIT License
