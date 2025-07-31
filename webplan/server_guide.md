# Flask Webサーバー構築ガイド（初心者向け）

## 1. 開発環境の準備

### 1.1 必要なソフトウェア
- **Python 3.8以上**をインストール
  - [Python公式サイト](https://www.python.org/)からダウンロード
  - インストール時に「Add Python to PATH」にチェックを入れる
- **VSCode**をインストール
  - [VSCode公式サイト](https://code.visualstudio.com/)からダウンロード
  - Python拡張機能をインストール

### 1.2 VSCodeの設定
1. **Python拡張機能のインストール**
   - VSCodeを開く
   - 左側の拡張機能タブ（四角いアイコン）をクリック
   - 検索で「Python」を入力
   - MicrosoftのPython拡張機能をインストール

2. **フォルダを開く**
   - VSCodeで「ファイル」→「フォルダを開く」
   - 新しいプロジェクト用のフォルダを作成して選択

## 2. 仮想環境の作成と設定

### 2.1 仮想環境とは
- **仮想環境**は、プロジェクト専用のPython環境
- 他のプロジェクトとライブラリの競合を防ぐ
- プロジェクトごとに独立した環境を作れる

### 2.2 仮想環境の作成手順

#### 手順1: ターミナルを開く
- VSCodeで「表示」→「ターミナル」を選択
- または `Ctrl + `` （バッククォート）でターミナルを開く

#### 手順2: 仮想環境を作成
```bash
# 仮想環境を作成（名前は「venv_slide_maker」, 「venv」が一般的ですが、混乱するのでこちらを利用）
python -m venv venv_slide_maker
```

#### 手順3: 仮想環境を有効化
**Windowsの場合:**
```bash
venv_slide_maker\Scripts\activate
```

**Mac/Linuxの場合:**
```bash
source venv_slide_maker/bin/activate
```

#### 手順4: 有効化の確認
- ターミナルの先頭に `(venv_slide_maker)` が表示されれば成功
- 例: `(venv_slide_maker) C:\project>`

### 2.3 仮想環境の管理
- **有効化**: 上記のactivateコマンド
- **無効化**: `deactivate` コマンド
- **削除**: フォルダごと削除（`venv_slide_maker`フォルダを削除）

## 3. Flaskのインストールと基本設定

### 3.1 Flaskのインストール
仮想環境が有効化された状態で：
```bash
pip install flask
```

### 3.2 依存関係の管理
```bash
# 現在インストールされているライブラリを確認
pip list

# 依存関係をファイルに保存
pip freeze > requirements.txt

# 他の環境で同じライブラリをインストールする場合
pip install -r requirements.txt
```

### 3.3 プロジェクト構造の作成
```
my_flask_app/
├── app.py              # メインアプリケーション
├── requirements.txt    # 依存関係
├── uploads/           # アップロードファイル保存先
├── downloads/         # ダウンロードファイル保存先
├── templates/         # HTMLテンプレート
├── static/           # CSS、JS、画像ファイル
└── logs/             # ログファイル
```

## 4. ポート番号の理解

### 4.1 ポート番号とは
- **ポート番号**は、コンピュータ内の通信の出入口
- Webサーバーは通常 **5000番ポート** を使用
- ローカル環境では `http://localhost:5000` でアクセス

### 4.2 よく使うポート番号
- **80**: HTTP（Webサイト）
- **443**: HTTPS（暗号化されたWebサイト）
- **5000**: Flask開発サーバー（デフォルト）
- **8000**: 代替ポート（5000が使われている場合）

### 4.3 ポートの確認方法
```bash
# Windowsでポートの使用状況を確認
netstat -an | findstr :5000

# Mac/Linuxでポートの使用状況を確認
lsof -i :5000
```

## 5. 基本的なFlaskアプリケーションの作成

### 5.1 最小限のアプリケーション
`app.py` ファイルを作成：
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```

### 5.2 アプリケーションの実行
```bash
python app.py
```

### 5.3 ブラウザでの確認
- ブラウザを開く
- アドレスバーに `http://localhost:5000` を入力
- 「Hello, World!」が表示されれば成功

## 6. ローカルでの試験方法

### 6.1 開発サーバーの起動
```bash
# 基本的な起動方法
python app.py

# または
flask run

# 特定のポートで起動
flask run --port=8000

# 外部からのアクセスを許可
flask run --host=0.0.0.0
```

### 6.2 デバッグモードの活用
- **デバッグモード**を有効にすると：
  - コードを変更すると自動でサーバーが再起動
  - エラーが発生した時に詳細な情報が表示
  - ブラウザでデバッグコンソールが使える

### 6.3 よくある問題と解決方法

#### 問題1: ポートが既に使用されている
```bash
# 別のポートで起動
flask run --port=8000
```

#### 問題2: 仮想環境が有効化されていない
```bash
# 仮想環境を有効化
venv_slide_maker\Scripts\activate  # Windows
source venv_slide_maker/bin/activate  # Mac/Linux
```

#### 問題3: Flaskがインストールされていない
```bash
# Flaskをインストール
pip install flask
```

### 6.4 ファイルアップロード機能のテスト

#### 基本的なアップロード機能
```python
from flask import Flask, request, render_template
import os

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = 'uploads'

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        if 'file' not in request.files:
            return 'ファイルが選択されていません'
        
        file = request.files['file']
        if file.filename == '':
            return 'ファイルが選択されていません'
        
        if file:
            filename = file.filename
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
            return f'ファイル {filename} がアップロードされました'
    
    return '''
    <!doctype html>
    <html>
    <head><title>ファイルアップロード</title></head>
    <body>
        <h1>ファイルをアップロード</h1>
        <form method="post" enctype="multipart/form-data">
            <input type="file" name="file">
            <input type="submit" value="アップロード">
        </form>
    </body>
    </html>
    '''

if __name__ == '__main__':
    # uploadsフォルダが存在しない場合は作成
    os.makedirs('uploads', exist_ok=True)
    app.run(debug=True)
```

## 7. トラブルシューティング

### 7.1 よくあるエラーと解決方法

#### エラー1: ModuleNotFoundError
```bash
# 仮想環境が有効化されているか確認
# プロンプトに (venv) が表示されているかチェック
```

#### エラー2: Address already in use
```bash
# 別のポートを使用
flask run --port=8000
```

#### エラー3: Permission denied
```bash
# 管理者権限でVSCodeを実行
# または、別のポートを使用
```

### 7.2 デバッグのコツ
1. **エラーメッセージをよく読む**
2. **ターミナルの出力を確認**
3. **ブラウザの開発者ツールを活用**
4. **print文でデバッグ情報を出力**

## 8. 次のステップ

### 8.1 学習の順序
1. **基本のFlaskアプリケーション**を作成
2. **HTMLテンプレート**を使用
3. **フォーム処理**を実装
4. **ファイルアップロード**機能を追加
5. **データベース**との連携
6. **セキュリティ**対策を実装

### 8.2 参考資料
- [Flask公式ドキュメント](https://flask.palletsprojects.com/)
- [Python公式チュートリアル](https://docs.python.org/ja/3/tutorial/)
- [VSCode Python拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

### 8.3 実践的な練習
1. **TODOリスト**アプリの作成
2. **ブログ**システムの構築
3. **ファイル管理**システムの開発
4. **API**の作成

## 9. セキュリティの基本

### 9.1 開発時の注意点
- **デバッグモード**は開発時のみ使用
- **機密情報**は環境変数で管理
- **ファイルアップロード**は適切に制限
- **入力値**は必ず検証する

### 9.2 APIキーの管理方法（.envファイル推奨）

#### 9.2.1 .envファイルの設定
1. **python-dotenvのインストール**
```bash
pip install python-dotenv
```

2. **.envファイルの作成**
```
# .envファイル（プロジェクトのルートに作成）
API_KEY=your_api_key_here
DATABASE_URL=your_database_url_here
SECRET_KEY=your_secret_key_here
WEATHER_API_KEY=your_weather_api_key_here
```

3. **.gitignoreファイルの作成・編集**
```bash
# .gitignoreファイルに以下を追加
.env
*.env
```

4. **.env.exampleファイルの作成**
```
# .env.exampleファイル（APIキーは含めない）
API_KEY=your_api_key_here
DATABASE_URL=your_database_url_here
SECRET_KEY=your_secret_key_here
WEATHER_API_KEY=your_weather_api_key_here
```

#### 9.2.2 Flaskアプリケーションでの使用
```python
from flask import Flask
from dotenv import load_dotenv
import os

# .envファイルを読み込み
load_dotenv()

app = Flask(__name__)

# 環境変数からAPIキーを取得
api_key = os.getenv('API_KEY')
database_url = os.getenv('DATABASE_URL')

@app.route('/')
def hello():
    return f'API Key: {api_key[:10]}...'  # 最初の10文字のみ表示
```

#### 9.2.3 依存関係の管理
```bash
# requirements.txtに追加
pip install python-dotenv
pip freeze > requirements.txt
```

#### 9.2.4 外部APIの使用例
```python
import requests
import os
from flask import Flask, jsonify
from dotenv import load_dotenv

# .envファイルを読み込み
load_dotenv()

app = Flask(__name__)

@app.route('/weather/<city>')
def get_weather(city):
    api_key = os.getenv('WEATHER_API_KEY')
    if not api_key:
        return jsonify({'error': 'API key not found'}), 500
    
    url = f"http://api.weatherapi.com/v1/current.json?key={api_key}&q={city}"
    
    try:
        response = requests.get(url)
        response.raise_for_status()
        return jsonify(response.json())
    except requests.RequestException as e:
        return jsonify({'error': str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True)
```

#### 9.2.5 セキュリティのベストプラクティス
- **.envファイルは絶対にGitにコミットしない**
- **.env.exampleファイルで必要な環境変数を明示**
- **本番環境では環境変数を直接設定**
- **APIキーは定期的に更新**
- **不要なAPIキーは削除**

### 9.3 本番環境への移行
- **デバッグモード**を無効化
- **HTTPS**を使用
- **適切なエラーハンドリング**を実装
- **ログ**を記録
- **環境変数**を本番サーバーで設定
