# 令和8年3月1日
**Phix.1.0.5.KB0000001.ja_jp**: Phix 独自ドキュメントソース (カスタム HTML) 型式から汎用 Markdown 型式への変換方法

TBA

## おおまかな流れ

1. makephix.exw 形式の英語版ドキュメントソースコードをテキストファイルへ変換 (sed)
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

削除すれば通るようにはなりますが、テーブルは残ったままになるので考えものです。

* Latin-1 文字の混入
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

## 関連

## 改訂履歴
