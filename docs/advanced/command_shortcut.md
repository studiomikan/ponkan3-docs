# コマンドショートカット機能

コマンドショートカット機能は、
頻繁に使用するコマンドを簡単に記述するための機能です。

コマンドショートカットを設定すると、コマンドを１文字で呼び出すことができるようになります。

## 簡単に改行できるようにする

ここでは例として`br`コマンドのショートカットを設定して、簡単に改行できるようにしてみます。

コマンドショートカットを設定するには、`commandshortcut`コマンドを使用します。

```plain
;commandshortcut ch: "$", command:"br"
```

上記のように書くと、テキストの中に`$`という文字が出てきたとき、
`$`をメッセージとして出力する代わりに`br`コマンドを実行するようになります。

このように、コマンドショートカットとは、任意の一文字にコマンド実行を割り当てる機能になります。

`$`のショートカットを利用すると、記述量を減らすことができます。

```
# ショートカットを使わないとき
こんにちは。
;br
今日はいい天気ですね。
;br

# ショートカットを使うと、簡単に書けるようになります
こんにちは。$
今日はいい天気ですね。$
```

## マクロとの組み合わせ

コマンドショートカットはマクロにも設定することができるため、
複雑な処理も1文字で呼び出せるようになります。

```
;macro name: "lbr"
  ;linebreak
  ;br
;endmacro

# ただ改行するだけなら$、クリック待ちをするなら@
;commandshortcut ch: "$", command: "br"
;commandshortcut ch: "@", command: "lbr"
```

## 注意点

- コマンドショートカットではコマンドにパラメータを渡すことはできません。
- ショートカットが設定された文字は、メッセージとして出力されなくなります。
    - メッセージとして出したい場合は、`ch`コマンドを使用するか、`delcommandshortcut`でショートカットを削除します。