# SCSS

## dart-sass
`@import`は非推奨、代わりに`@use`を使う

ディレクトリ構造の例
```
// scss/
style.scss
foundation/_index.scss
           _mixin.scss
           _variables.scss
           _reset.scss
```
各サブディレクトリの`_index.scss`で同ディレクトリのscssコンポーネントを読み込む
```scss
// foundation/_index.scss
@use './reset';
@use './valuables' as vr;
@use './mixin' as mix;
```

最終的にcssにコンパイルするstyle.scssの例
```scss
// style.scss
@charset "UTF-8";
@use './foundation/index' as foundation;
@use './common/index' as common;
@use './pages/index' as pages;
```

変数やmixinなどを他のscssファイルで使う場合は@useでひとつずつ読み込んで名前空間をつける
```scss
// pages/_top.scss
@use '../foundation/valuables' as vr;
@use '../foundation/mixin' as mix;

.top {
    width: vr.$top-width-pc;
    @include mix.tab{
        width: vr.$top-width-tab;
    }
    @include mix.sp{
        width: vr.$top-width-sp;
    }
}
```

## メディアクエリ
PCベースでSPとTABのレスポンシブに対応する記述

```scss
$pc: 1280px;
$tab: 768px;

@mixin sp {
    @media screen and (max-width: ($tab - 1)) {
        @content;
    }
}

@mixin tab {
    @media screen and (min-width: ($tab)) and (max-width: ($pc - 1)) {
        @content;
    }
}

@mixin pc {
    @media screen and (min-width: ($pc)) {
        @content;
    }
}
```
