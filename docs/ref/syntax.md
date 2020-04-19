# スクリプトファイルの文法

スクリプトファイル文法について説明します。

## スクリプトファイルの例

```plain
# コメント行です。

# 行頭が ; から始まる行はコマンド行です。
;layopt { lay: 0, x: 100, y: 200 }
;layopt {
  lay: 0,
  x: 100,
  y: 200
}
# 一行で書くときは、{}を省略できます。
;layopt lay: 0, x: 100, y: 200

# 行頭が * から始まる行はラベル行です。
*jump_target_label

# 行頭が ~ から始まる行はセーブマーク行です。
~save_mark_01|セーブポイント01

# 行頭が - または = から始まる行はJavaScript行です。
- alert("JavaScript行です。");
= "100 + 200 = " + (100 + 200)

# --- と --- で囲まれた部分はJavaScript部です。
---
const a = 100;
const b = 200;
let sum = a + b;
alert("a + b = " + sum);
---

# 上記以外のすべての文字はテキストになります
こんにちは。
```

## コメント行

行の先頭が`#`から始まっている行は、コメント行です。
コメント行は、スクリプトファイルが実行されるときには無視されて、何も実行されません。

コメント行は、スクリプトファイル中に記載された注釈文です。
スクリプトファイルに書かれている内容について補足や解説を書くことができます。

## コマンド行

行の先頭が`;`から始まっている行は、コマンド行です。
コマンド行は、Ponkanの様々な機能を実行するための命令文です。

```plain
;layopt { lay: 0, x: 100, y: 200 }
```

コマンド行は、コマンド名とパラメータから成り立っています。
上記の例では、`layopt`がコマンド名、`{ lay: 0, x: 100, y: 200 }`がパラメータです。

パラメータは「パラメータ名: 値」という風に記述します。
パラメータが複数ある場合はカンマ(`,`)で区切って記載します。

パラメータは、コマンドの実行時に使われる具体的な値です。
たとえば上記の`layopt`というコマンドは、レイヤーに関する設定を行うコマンドです。
このコマンドに`lay: 0, x: 100, y: 200`の3つのパラメータが渡されているので、
この場合は「レイヤー0番のx座標を100、y座標を200に設定する」という命令になります。

パラメータは複数行にわたって記述することもできます。

```plain
;layopt {
  lay: 0,
  x: 100,
  y: 200
}
```

このように改行して書くことができるので、非常に見やすくなります。
パラメータの数が多い場合には活用してください。

コマンドを一行で書く場合は、パラメータの`{`と`}`を省略することができます。パラメータ中に改行したい場合は省略できません。
以下の2つは全く同じ意味となります。

```plain
# 以下の2行は同じ意味
;layopt { lay: 0, x: 100, y: 200 }
;layopt lay: 0, x: 100, y: 200
```

<div class="note">
パラメータの部分の実態はJavaScriptのObjectリテラルです。<br>
そのため、値は固定値だけではなく、式を記述することもできます。
<pre><code>;layopt { lay: 0, x: 1280-20, y: 720-20 }</code></pre>
</div>

コマンドの詳細については、[コマンドリファレンス](command_ref.md)に記載されています。
ただ、数が膨大ですので、まずはこのマニュアルのチュートリアルを一通り体験してから参照するとよいでしょう。

## ラベル行

行の先頭が`*`から始まっている行は、ラベル行です。

ラベルは`jump`コマンドと`call`コマンドと組み合わせて使用されます。
`jump`はスクリプトファイルの移動、`call`はサブルーチンの呼び出しを行うコマンドですが、
これらの呼び出し先を指定するためにラベルが使用されます。

たとえば以下の`jump`コマンドの例では、`title.pon`スクリプトの中の`*jump_target`が書かれた位置に移動します。

```plain
;jump file: "title.pon", label: "jump_target"
```

## セーブマーク行

行の先頭が`~`から始まっている行は、セーブマーク行です。

Ponkanでのセーブ機能は、このセーブマーク行を記述することで可能になります。

通常は`~セーブマーク名|見出しテキスト`と記述しますが、セーブマーク名、見出しテキストともに省略可能です。

- 見出しテキストを省略したとき：`~セーブマーク名|`
- セーブマーク名を省略したとき：`~|見出しテキスト` または `~見出しテキスト`
- 両方を省略したとき： `~|` または `~`

具体的な使い方については、[セーブ＆ロード機能](../basic/save_and_load.md)のページを参照してください。

## JavaScript行

行の先頭が`-`または`=`から始まっている行は、JavaScript行です。

記述したJavaScriptを実行することができます。

`-`の場合はJavaScriptを実行し、結果は捨てます。

`=`の場合は、実行した結果（評価値）を文字列としてメッセージレイヤーに出力します。

```
# 計算を実行しますが、計算結果は捨てられます。
- 100 + 200
# 計算を実行して、結果を文字列としてメッセージレイヤーに出力します。
# この場合は画面に300と表示されます。
= 100 + 200
```

## JavaScript部

`---`の行から`---`の行までの間は、JavaScript部です。


```plain
---
// 自由にJavaScriptを記述できます。
function getSum() {
  let sum = 0;
  for (let i = 0; i < 10; i++) {
    sum += i;
  }
  return sum;
}
tv.sum = getSum();
---
= tv.sum
```

`-`から始まるJavaScript行では、JavaScriptを1行しか書けませんでしたが、
JavaScript部では改行を含んだ複数行のJavaScriptを記述することができます。
長い処理を記載する場合はこちらを利用したほうが良いでしょう。

### 変数の扱いについて

JavaScript行やJavaScript部の内部で宣言した変数は、実行するたびにクリアされます。
そのため、以下のように書くとエラーになります。

```plain
- var a = 100;
= a + 200;  // 変数aは存在しないのでエラー

---
var b = 100;
---
---
var c = b + 100;  // 変数bは存在しないのでエラー
---
```

複数の計算結果を受け渡したい場合や、計算結果をコマンドで使用したい場合は、
一時変数用のオブジェクト`tv`を使用します。
変数の詳細については、[JavaScriptと変数](../basic/javascript.md)を参照してください。


## テキスト

コメント、ラベル、セーブマーク、JavaScript以外すべての文字はテキストとして扱われます。

テキストは、メッセージレイヤーに出力されます。

## インデントについて

Ponkanのスクリプトファイルでは、
行頭の半角スペースおよびタブ文字については無視されるため、
インデントして読みやすくすることができます。

```plain
# インデントで読みやすくすることができます
;macro name: "lbr"
  ;l
  ;br
;endmacro

;for loops: "2", indexvar: "i"
  ;for loops: "5", indexvar: "j"
    ---
    tv.a = tv.i + 1;
    tv.b = tv.j + 1;
    tv.c = tv.a * tv.b;
    ---
    = tv.a + "×" + tv.b + "=" + tv.c
    ;br
  ;endfor
  ;br
;endfor
```