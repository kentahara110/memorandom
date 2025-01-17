# Tailwind CSS概要
[参考サイト](https://reffect.co.jp/html/tailwindcss-for-beginners)

以下抜粋

### Tailwind CSSのメリットとは
Tailwind CSSの詳細を確認する前にどのようなメリットがあるのか確認していきます。

classを設定する場合にclassの名前のルールやどのような名前にすべきか悩んだ経験はありませんか？Tailwind CSSでは自分でclass名を付与することはないので命名に関するストレスから解放されます。

CSSファイルをHTMLファイルと別に用意している場合、ファイルを切り替えるのが面倒だと感じたことはありませんか？Tailwind CSSではHTMLファイルの要素に対して事前に用意されたclassを適用するのでCSSを設定するためにファイルを切り替えるという手間はありません。CSSファイルを別ファイルとしてわけず1つのファイルでstyleタグの中にclassを設定する場合もstyleタグの記述場所に移動する必要がありますがTailwind CSSではその必要はありません。

hoverなどの擬似クラス(pesedo-classes)をstyle属性を利用して直接要素に設定したいと思ったことはありませんか？残念ながら疑似クラスはstyle属性で利用することができません。Tailwind CSSでは疑似クラスもclassとして事前に準備しているため要素に直接疑似クラスを設定することができます。hoverだけではなくmadia queryの設定も要素で直接行うことができるのでレスポンシブデザインも簡単に実現することができます。

### Tailwind CSSのデメリットとは
どのようなツールでもメリットがあれば必ずデメリットも存在します。Tailwind CSSのデメリットについて確認しておきます。

CSSの正しい設定方法を忘れてしまう。Media Query, Gridの設定などutility classを利用すると簡単に設定できるのでTailwind CSSがない環境でCSSの設定が必要になった場合記述できなくなる場合があります。

ビルドが必要。style属性やCSSファイルを利用している場合特別な追加設定の必要はありませんがTailwind CSSを利用する場合は事前にビルドの設定が必要になります。

リストなど同じスタイルを設定したい要素が複数ある場合に同じ設定を何度も繰り返さなくてはならない。

スタイルによっては1つの要素に設定するclassの設定が膨大な数となり何を設定しているのか理解するのが困難になります。


抜粋ここまで

## Tailwind CSSを使ってみての所感
スタイル属性と同じような感覚で使えるため、スタイル名をつけるほどでもない細かいスタイルを指定する場合などに便利

便利な反面、scssなどと併用するとスタイルの記述がやや複雑になり、可読性とメンテナンス性が落ちるため、利用は最低限にするなどルールを決めておきたい

:::note warn
Tailwind CSSを導入しない選択もあり
:::

# Tailwind CSSをnuxt3プロジェクトに導入する
[参考サイト](https://qiita.com/keitaMax/items/1a28eea86f163fd2952f)

## Tailwind CSSをインストール
以下のコマンドでTailwind CSSのインストールとtailwind.config.jsファイルの作成をします。
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init

```

## Tailwind CSSを使用できるようにする
tailwind.config.jsを以下のようにします。
```
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./components/**/*.{js,vue,ts}",
    "./layouts/**/*.vue",
    "./pages/**/*.vue",
    "./plugins/**/*.{js,ts}",
    "./app.vue",
    "./error.vue",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};

```

次に、assets/css/フォルダを作成し、その中にmain.cssファイルを作成します。

```
@tailwind base;
@tailwind components;
@tailwind utilities;

```

nuxt.config.jsファイルを以下のように修正します。

```
export default defineNuxtConfig({
  ...
  css: ["~/assets/css/main.css"],     //追加
  postcss: {     //追加
    plugins: {     //追加
      tailwindcss: {},     //追加
      autoprefixer: {},     //追加
    },     //追加
  },     //追加
  ...
});

```

これでTailwind CSSが適応できているので確認
```
yarn dev

```

## ブレークポイントを設定する
tailwind.config.jsにブレークポイントを追加する
```
/** @type {import('tailwindcss').Config} */
export default {
theme: {
    screens: {
      'sp': {'max': '767px'},
      'tab': {'min': '768px'},
      'pc': {'min': '1280px'},
    },

    extend: {},
  },
}
```
使用例
```
// 画面幅で異なるスタイルを適用する
<h2 class="sp:mb-[14px] tab:mr-[70px] pc:mr-[84px]"> 
```