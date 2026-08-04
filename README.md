# JANコード読み取りテスト(静的HTML版)

スマホブラウザでJANコード(EAN-13/EAN-8)を読み取れるかどうかだけを確認する最小テストです。
GAS版(`jan_scanner_test/`)でカメラが`NotAllowedError`になった原因を切り分けるため、
GASのiframe実行環境を経由しない、ふつうのHTTPS静的ページとして作成しました。

## 構成

- `index.html` — 単一ファイルの静的ページ。サーバーサイドの処理は一切ありません。

## 使用ライブラリ

- [`@zxing/library`](https://github.com/zxing-js/library) v0.20.0(CDN: unpkg経由で読み込み)

## できること

- スマホブラウザでページを開く
- 「カメラを起動」でカメラを起動(背面カメラを優先)
- EAN-13 / EAN-8を読み取り、13桁または8桁の数字を画面に表示
- 読み取り成功後は自動でカメラ停止
- 「カメラを停止」で手動停止
- 再度「カメラを起動」で再読み取り

## セキュリティ・制約

- 読み取ったJANコードはブラウザ画面に表示するだけで、どこにも送信・保存しません。
- ログにJANコードを出力する処理もありません。

## ローカルでの動作確認

カメラ機能はHTTPS(またはlocalhost)でのみ動作します。ローカルで見た目を確認する場合:

```bash
cd jan_scanner_static_test
python3 -m http.server 8000
```

`http://localhost:8000/` をPCブラウザで開けば、レイアウトと(PCのカメラがあれば)動作を確認できます。
`localhost`はセキュアコンテキスト扱いのためカメラAPIが動作しますが、スマホの実機テストは
GitHub Pagesなど実際のHTTPS URLで行ってください。

## GitHub Pagesで公開する手順

1. このフォルダの内容をGitHubリポジトリにpushする
2. リポジトリの Settings → Pages を開く
3. Source を「Deploy from a branch」にし、対象ブランチとフォルダ(例: `main` / `/jan_scanner_static_test` または
   ルート直下に配置する場合は `/ (root)`)を選択して保存
4. 発行されたURL(例: `https://<username>.github.io/<repo>/`)にスマホでアクセス

## スマホでのテスト手順

1. GitHub PagesのURLをスマホブラウザで開く
2. 「カメラを起動」をタップ→カメラ許可を求められたら許可
3. JANコードを画面中央に合わせる
4. 読み取り成功で13桁/8桁の数字が大きく表示され、自動でカメラ停止
5. 「カメラを起動」を再度押せば再読み取り可能
