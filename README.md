# Slide Maker

## 説明

このプロンプトは、卒業論文をわかりやすくまとめたスライドを作るためのものです。

論文はTeX（テフ）という形式のファイルで書かれています。

このプロンプトでは、そのTeXファイルから「概要」「目的」「証明」など、
論文の大事な部分を自動で取り出し、スライドの形に整理します。

## プロジェクト構成

```
SlideMaker/
├── webapp/                 # Flask Webアプリケーション
│   ├── static/            # 静的ファイル（CSS、JS、画像）
│   ├── templates/         # HTMLテンプレート
│   ├── app.py             # Flaskメインアプリケーション
│   ├── requirements.txt   # Python依存関係
│   ├── config.py          # 設定ファイル
│   ├── run.py             # サーバー起動スクリプト
│   └── README.md          # Webアプリケーション説明書
├── matome/                # TeXファイル集
├── slide3/                # スライドサンプル3
├── slide4/                # スライドサンプル4
├── slide7/                # スライドサンプル7
├── slide8/                # スライドサンプル8
├── webplan/               # Webサービス計画書
├── 概要/                  # 概要資料
└── README.md              # このファイル
```

## Linuxサーバー上でのFlask運用

### 前提条件

1. **Python 3.8以上** がインストールされていること
2. **pip** がインストールされていること
3. **Linuxサーバー** にアクセス権限があること

### セットアップ手順

1. **Python仮想環境の作成**
   ```bash
   cd webapp
   python3 -m venv venv-slide-maker
   source venv-slide-maker/bin/activate
   ```

2. **依存関係のインストール**
   ```bash
   pip install -r requirements.txt
   ```

3. **環境変数の設定**
   ```bash
   export FLASK_APP=app.py
   export FLASK_ENV=production
   ```

### サーバーの起動

#### 開発モード
```bash
cd webapp
source venv-slide-maker/bin/activate
python run.py
```

#### 本番モード（Gunicorn使用）
```bash
cd webapp
source venv-slide-maker/bin/activate
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

### アクセス方法

1. サーバーを起動すると、以下のURLでアクセスできます：
   - ローカル: http://localhost:5000
   - 外部: http://your-server-ip:5000

2. ブラウザでSlide Makerアプリケーションが表示されます。

### システムサービスとして登録

```bash
# systemdサービスファイルを作成
sudo nano /etc/systemd/system/slide-maker.service
```

サービスファイルの内容：
```ini
[Unit]
Description=Slide Maker Flask Application
After=network.target

[Service]
User=your-username
WorkingDirectory=/path/to/SlideMaker/webapp
Environment="PATH=/path/to/SlideMaker/webapp/venv-slide-maker/bin"
ExecStart=/path/to/SlideMaker/webapp/venv-slide-maker/bin/gunicorn -w 4 -b 0.0.0.0:5000 app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

サービスを有効化：
```bash
sudo systemctl daemon-reload
sudo systemctl enable slide-maker
sudo systemctl start slide-maker
```

## 開発者向け情報

### 技術スタック
- **バックエンド**: Python Flask
- **フロントエンド**: HTML5 + CSS3 + JavaScript
- **テンプレートエンジン**: Jinja2
- **Webサーバー**: Gunicorn (本番環境)
- **開発サーバー**: Flask Development Server

### Webアプリケーションの詳細
詳細な使用方法や開発情報については、[webapp/README.md](webapp/README.md)を参照してください。

## TEXマーカー説明書

[TEXマーカー説明書](https://github.com/KazumasaFujiwaraSeminar2024/SlideMaker/blob/develop/TEX%E3%83%9E%E3%83%BC%E3%82%AB%E3%83%BC%E8%AA%AC%E6%98%8E%E6%9B%B8.md)