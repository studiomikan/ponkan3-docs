# 開発の準備（セットアップ）

Ponkanでゲーム開発を始めるための準備をします。

## Ponkanの入手

Ponkanの最新版はGitHubで公開しています。

以下のリンクから、最新版のzipファイルをダウンロードしてください。

[Ponkan最新版](https://github.com/studiomikan/ponkan/archive/master.zip)

Gitが使える方は`git clone`したほうが最新版へ更新しやすいのでそちらのほうが良いです。

```sh
$ git clone https://github.com/studiomikan/ponkan.git
```

## Node.jsの導入

開発環境として、Node.jsが必要です。

以下の公式サイトからダウンロードし、インストーラにしたがって導入してください。

LTS版（長期サポートの安定板）と最新版がありますが、どちらを導入しても構いません。
こだわりがなければLTS版が良いかと思います。

[Node.js 日本語サイト](https://nodejs.org/ja/)

インストール自体は、基本的にはデフォルト設定のまま進めたので問題ありませんが、
詳細に知りたい方は以下のページを参照してください。

* [Node.js導入（Windows）](./setup_nodejs_win.md)

## 新しいゲームを作成する

新しいゲーム用に、Ponkan関連のファイルをコピーします。

あらかじめ、コピーをするためのスクリプトが用意されていますので、それを利用します。

### Windowの場合

`tools/new_game.bat`をダブルクリックして実行してください。
途中、`Game folder name:`と表示されたら、新しいゲーム用のフォルダ名を入力してください。
（アルファベットを推奨します。できれば日本語は避けたほうが良いです。）

### Mac, Linuxの場合

`tools/new_game.sh`をターミナルから実行してください。
途中、`Game directory name:`と表示されたら、新しいゲーム用のフォルダ名を入力してください。
（アルファベットを推奨します。できれば日本語は避けたほうが良いです。）

## ディレクトリを移動する

正常に生成できれば、`games`に指定した名前でゲーム用ファイルがコピーされています。

このまま`games`ディレクトリで作業してもよいですが、どこかわかりやすい位置にディレクトリごと移動させておいたほうが良いでしょう。

フォルダごとどこか好きな場所に移動して、作業を開始しましょう。

## ゲームの起動

`setup.bat`または`setup.sh`を実行すると、ゲームがブラウザで起動します。

Windowsであれば`start.bat`をダブルクリック、Mac/Linuxであれば`setup.sh`をターミナルから実行してください。

環境によってブラウザが開かれないことがあります。
その場合は、ブラウザで`http://localhost:8080/index.html`を開いてください。

## ディレクトリの構成

作成した新しいゲームディレクトリの中で、は、以下のような構成になっています。

---
- <i class="md-icon">folder</i> node_modules/
    - 生成直後には存在せず、初回起動時に生成されます。Node.jsのライブラリなどが格納されます。
- <i class="md-icon">folder</i> public/
    - ゲーム本体のファイルが格納されています。
- <i class="md-icon">insert_drive_file</i> package-lock.json
    - Node.js用のファイルです。
- <i class="md-icon">insert_drive_file</i> package.json
    - Node.js用のファイルです。
- <i class="md-icon">insert_drive_file</i> start.bat
    - Windows用の起動スクリプトです。
- <i class="md-icon">insert_drive_file</i> start.sh
    - Mac、Linux用の起動スクリプトです。
---

`public`の下は、以下のような構成になっており、Ponkanのゲームに関係するファイルがすべて含まれます。

---
- <i class="md-icon">folder</i> public/
    - <i class="md-icon">folder</i> fonts/
        - ゲームで使うフォントファイルが格納されています。
    - <i class="md-icon">folder</i>  gamedata/
        - ゲームのスクリプト、画像、音声などのリソースファイルがここに格納されています。
          フォント以外のファイルはすべてここに格納します。
    - <i class="md-icon">insert_drive_file</i>  index.html
        - Ponkanのゲームを表示するためのhtmlファイルです。
    - <i class="md-icon">insert_drive_file</i>  ponkan.js
        - Ponkanの本体です。
    - <i class="md-icon">insert_drive_file</i>  settings.js
        - ゲームの設定ファイルです。
---

## 最初の設定

最初にいくつか新しいゲームの設定をしましょう。

### `settings.js`の編集

`public/settings.js`では、ゲーム全体の設定を行います。
テキストエディタで`settings.js`を開いて編集してください。

以下の設定は、必ず変更するようにしてください。

```javascript
  // ゲームバージョン
  // ゲームのバージョン番号を設定します。
  // バージョンアップして再公開する場合は、必ずここの値を変更してください。
  gameVersion: "0.0.0",

  // セーブデータ名のプレフィックス
  // セーブデータを保存する際の名前として使用されます。
  // 必ず初期値から変更するようにしてください。
  // もしこの値が同じだと、同じゲームのセーブデータだと判断されてしまいます。
  saveDataPrefix: "ponkan-sample",
```

### `index.html`の編集

`public/index.html`では、ブラウザ表示に関する設定を行います。
テキストエディタで`settings.js`を開いて編集してください。

#### ページタイトルを変更する

Webページのタイトルとして表示される文字列を変更します。
初期値は`"Ponkan"`となっているので、ゲームタイトルなどに変更してください。

```html
  <title>Ponkan</title>
```

### `init_system.pon`の編集

`public/gamedata/script/init_system.pon`では、メッセージのフォントやグリフなどの設定を行います。
テキストエディタで`init_system.pon`を開いて編集してください。

`init_system.pon`は、今すぐ設定しておくところはありません。
ゲームを作っていく中で、フォントを変更したり、レイヤー数を変更したくなった時などに参照してください。
