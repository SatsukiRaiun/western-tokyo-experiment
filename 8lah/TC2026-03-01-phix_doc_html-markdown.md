# 令和8年3月1日
**Phix.1.0.5.KB0000001.ja_jp**: Phix 独自ドキュメントソース (カスタム HTML) 型式から汎用 Markdown 型式への変換方法

TBA (執筆中・草稿案)

## おおまかな流れ

1. makephix.exw 形式の英語版ドキュメントソースコードをテキストファイルへ変換 (pandoc/sed/grep)
2. テキストファイルの手動整形
3. スペルチェッカーで誤字脱字を修正します。
4. Markdown ファイルへの変換とマークアップ復元
5. CHM/ビルド済み HTML版とMarkdown ファイルの比較照合
6. 作業後の英語版 Markdown ファイルをリポジトリへのアップロード
7. 英語版のファイルを TexTra で機械翻訳
8. 1 から 6 までの手順を二千回繰り返します。
9. 数ヶ月後に完了したら mkdocs か HuGo用の設定ファイルを書いて各種修正をした後で英語版を完成させます。
10. 同様に設定ファイルを書き終えて日本語版のポストエディットを行います。
11. ひと通り追えたら公開作業です。英語版はPhix公式リポジトリへプルリクエストを出してマージしていただききます。リジェクトされた場合は仕方ないのでほかの方法を考えます。
12. 日本語版は Github Pages で公開します。
13. これで日本の皆さんがスタート地点に立ってPhixを使えるようになるでしょう。
14. 以後は誤訳の修正や年単位での Phix の最新版対応作業になります。また、一般からの修正などの提案も受け入れる予定です。

ここまで一年から二年です。大変な思いをしますが、お賃金はでません。

## 判明している不具合 (v.1.0.5)

* pandoc 3.7.0.2 では a32Colour.htm などは正常に変換できない(ウェブブラウザでは正常に表示できる)。Another-Lint, Tidy で調べる限りと HTML の書き方がおかしいのが理由です。もしかすると table タグまわりがおかしいのかもしれません。ちゃんと規格準拠させてPandocで通るようにするには大規模改修になります。

これがあるせいで変換失敗するようです。
``` html
        <col style="width: 5%"/>
```

現在、このタグを使っているファイルは[こちら](https://github.com/search?q=repo%3Apetelomax%2FPhix+<col&type=code&p=1)です。削除すれば通るようにはなりますが、テーブルは残ったままになるので考えものです(対策は後ほど)。このタグ以外にも追加修正しないと正常に変換できないコトがあります。Pandoc変換後は必ず内容の照合をしましょう。

* Latin-1 文字や絵文字などの異種文字コードの混入
iconv や nkf で UTF-8へ変換しても対処しましょう。

``` bash
~/.../phix/html $ for f in *.htm; do pandoc -f html "$f" -t plain -o "${f%.htm}.txt"; done
[WARNING] IupCells.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupColorBrowser.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupColorbar.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupHide.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupImageLibOpen.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupListDialog.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupMatrixEx.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupPlot.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupSetAttribute.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] IupTree.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] a32dgeom.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] bigatom.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] callbacks.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] enter_cs.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] glFrustum.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] glRotate.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] glScale.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] glTranslate.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] gospec.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] regex.e.htm is not UTF-8 encoded: falling back to latin1.
[WARNING] regex_syntax.htm is not UTF-8 encoded: falling back to latin1.
```

* Markdown に変換すると元の HTML ファイルと同じ禁則処理にならない

pandoc では --wrap=preserve オプションを指定することで HTML ファイルの禁則処理のまま変換します。

このプロジェクトでは英語版 html との整合性を保つために、このオプションの指定をしてください。整合性のないものをMarkdownに変換してプルリクエストで出されてもリジェクトします。今のところ日本語版は特に指定はありませんが、同じルールを適用するかもしれません。

## 関連

## 改訂履歴
