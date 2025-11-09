# Open CI 使用ガイド

このドキュメントでは、Open CIのself-hosted runnerでこのリポジトリを使用する方法を説明します。

## Self-hosted Runnerのセットアップ

### 1. ランナーの追加

1. GitHubリポジトリのページで **Settings** をクリック
2. 左側のメニューから **Actions** > **Runners** を選択
3. **New self-hosted runner** ボタンをクリック
4. OSを選択（Linux/macOS/Windowsなど）
5. 表示されるコマンドを実行してランナーをインストール

### 2. ランナーの起動

```bash
# ランナーディレクトリに移動
cd actions-runner

# ランナーを起動
./run.sh
```

### 3. 確認

ランナーが正常に起動すると、Settings > Actions > Runners のページにランナーが表示されます。

## ワークフローの実行方法

### 自動実行

以下のイベントで自動的にワークフローが実行されます：

- **Push**: `main`または`master`ブランチにコードをプッシュ
- **Pull Request**: `main`または`master`ブランチに対するプルリクエスト作成時

### 手動実行

1. GitHubリポジトリの **Actions** タブに移動
2. 実行したいワークフローを選択（Basic CI/Matrix Build/Simple Echo Test）
3. **Run workflow** ボタンをクリック
4. ブランチを選択して **Run workflow** をクリック

## ワークフローの説明

### Simple Echo Test（最も簡単）

**用途**: ランナーの基本動作確認

このワークフローは最もシンプルで、以下を実行します：
- メッセージの出力
- システム情報の表示
- 環境変数の確認

**実行時間**: 約10秒

### Basic CI（基本的なCI）

**用途**: Python アプリケーションの完全なテスト

実行内容：
1. リポジトリのチェックアウト
2. Python 3.10 のセットアップ
3. 依存関係のインストール
4. テストの実行（5つのテストケース）
5. アプリケーションの実行

**実行時間**: 約1-2分（初回実行時はPythonのセットアップに時間がかかる場合があります）

### Matrix Build（マトリックスビルド）

**用途**: 複数のPythonバージョンでのテスト

実行内容：
- Python 3.8, 3.9, 3.10, 3.11 の4つのバージョンで並列実行
- 各バージョンでテストを実行

**実行時間**: 約2-3分（並列実行のため）

## トラブルシューティング

### ワークフローが実行されない

1. ランナーが起動していることを確認
2. ランナーがオンライン状態であることを Settings > Actions > Runners で確認
3. ワークフローファイルの構文エラーをチェック

### Pythonがインストールされていない

ランナーのマシンに Python がインストールされていない場合、`actions/setup-python@v4` が自動的にインストールします。

### テストが失敗する

1. ローカルで `pytest -v test_app.py` を実行して確認
2. 依存関係が正しくインストールされているか確認（`pip list`）

### パーミッションエラー

ランナーに十分な権限があることを確認してください。特に：
- Python のインストール
- pip によるパッケージのインストール
- ファイルの読み書き

## ログの確認

ワークフローの実行ログは以下の手順で確認できます：

1. **Actions** タブに移動
2. 確認したいワークフローの実行を選択
3. ジョブ名をクリックしてログを表示
4. 各ステップを展開して詳細を確認

## カスタマイズ

### ワークフローの追加

`.github/workflows/` ディレクトリに新しい `.yml` ファイルを作成することで、独自のワークフローを追加できます。

### テストの追加

`test_app.py` にテストケースを追加できます：

```python
def test_my_function():
    """新しいテスト"""
    assert my_function() == expected_result
```

### Python バージョンの変更

`matrix-build.yml` の `python-version` を編集：

```yaml
matrix:
  python-version: ['3.9', '3.10', '3.11', '3.12']  # バージョンを追加/変更
```

## その他の設定

### ランナーのラベル

特定のランナーでのみワークフローを実行したい場合、ランナーにラベルを付けることができます：

```yaml
jobs:
  test:
    runs-on: [self-hosted, linux, x64]  # 複数のラベルを指定
```

### タイムアウトの設定

ワークフローのタイムアウトを設定：

```yaml
jobs:
  test:
    runs-on: self-hosted
    timeout-minutes: 30  # 30分でタイムアウト
```

## 参考リンク

- [GitHub Actions ドキュメント](https://docs.github.com/ja/actions)
- [Self-hosted runners について](https://docs.github.com/ja/actions/hosting-your-own-runners/about-self-hosted-runners)
- [pytest ドキュメント](https://docs.pytest.org/)
