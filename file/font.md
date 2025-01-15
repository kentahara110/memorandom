# font

## Google Fonts

### Noto

基本的なフォントの一つ。Google Fonts から `Noto Sans` または `Noto Serif` の日本語フォントを読み込む際、約物（句読点など）を半角幅としたい場合、後述する `Yaku Han JP` を参照。

## その他

### Yaku Han JP

https://github.com/qrac/yakuhanjp
https://yakuhanjp.qranoko.jp/

CSSでは、`Noto Sans JP` または `Noto Serif JP` CSSの `font-family` で先（左側）に読み込む事で全角の約物（句読点などの）文字が半角幅となる。

```css
font-family: "YakuHanMP", "Noto Serif JP", serif;
```

## フォントに関連するもの

### webfontloader

https://github.com/typekit/webfontloader
https://www.jsdelivr.com/package/npm/webfontloader

webフォントの読み込みや、読み込み完了を待ちたい時に便利なJSライブラリ。