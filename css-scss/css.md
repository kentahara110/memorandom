# CSS

## リセットCSS
### ress
不要なマージンやパディングなど消しつつ`box-sizing: border-box;`など最低限のスタイルが記述してある

## 画像のレスポンシブ
`img {max-width:100％}`で画面や親要素のサイズに合わせる
```css
.parent{
    width: 100%;
    max-width:480px;
}
.parent img {
    max-width:100％;
}
```

## 画像の比率を保つ

```css
img{
  width: 100%; // or whatever
  aspect-ratio: 16 / 9;
  object-fit: cover;
}
```

## 画像の下部の隙間をなくす

```css
img{
	vertical-align:top;
}
```

## gridのエリア指定
```css
.親要素{
    display: grid;
    grid-template-areas: 
        'heading kv'
        'kw kv'
        'desc kv'
        'btn kv'
    ;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: auto auto auto auto;
    gap: 0px 50px; // row column
}
.子要素1{
    grid-area: heading;
}
.子要素2{
    grid-area: kv;
}
...
```

## flexで要素の間を開けたい場合
marginなどであけるよりもgapで簡単に開けれる
```css
.flex{
    display: flex;
    gap: 20px 50px; // row column
}
```

## 文字にオリジナルの下線を引きたい場合など
擬似要素を使って文字に下線や背景など様々なスタイルを割り当てれる
```scss
&__keyword{
    h3{ // 下線を入れたい要素
        position: relative;
        font-size: 21px;
        line-height: 41px;
        z-index: 2; // 前後関係
        ::after{
            content: '';
            position: absolute;
            width: 302px; // 長さ指定
            height: 31px; // 河川の太さ
            background-color: #FFFFFF; // 下線の色
            top: 5px; // 下線の上下位置
            left: -9px; // 下線のスタート位置
            z-index: -1; // 前後関係
        }
    }    
}
```

## アコーディオンメニュー
html
```html
<div class="c-faq__wrap">
    <details class="c-faq__item">
        <summary>
            <img src="" alt="Q. ">
            <p>
                アコーディオンのデザイン
            </p>
        </summary>
        <div class="c-faq__item__content">
            <img src="" alt="A. ">
            <p>
                下線だけのシンプルなアコーディオンメニュー。クセがなくどんなサイトでも使いやすいのが特徴です。
            </p>
        </div>
    </details>
    <details class="c-faq__item">
        <summary>
            <img src="" alt="Q. ">
            <p>
                アコーディオンのデザイン
            </p>
        </summary>
        <div class="c-faq__item__content">
            <img src="" alt="A. ">
            <p>
                下線だけのシンプルなアコーディオンメニュー。クセがなくどんなサイトでも使いやすいのが特徴です。
            </p>
        </div>
    </details>
</div>
```
scss
```scss
.c-faq{
    &__wrap{
        width: 1073px;
        margin: 0 auto;
        margin-top: 48px;
        padding: 0 35px;
    }
    &__item{
        border-bottom: 1px solid #d0d0d0;
        summary{
            position: relative;
            display: flex;
            justify-content: start;
            align-items: center;
            padding: 16px 0;
            color: #333333;
            cursor: pointer;
            width: 100%;
            &::after{
                position: absolute;
                top: 50%;
                right: 12px;
                transform: translate(0, -50%);
                background-image: url(../img/accordion_plus.svg);
                background-position: center;
                background-size: contain;
                background-repeat: no-repeat;
                width: 16px;
                height: 16px;
                margin-right: 10px;
                content: '';
            }
        }
        &[open]{
            summary{
                &::after{
                    background-image: url(../img/accordion_minus.svg);

                }
            }
        }
        &__content{
            display: flex;
            align-items: center;
            padding-bottom: 29px;
        }
    }
}
```

## fixedのスクロール
`position: fixed;`の要素に`overflow-y: scroll;`をつける

## 文字の禁則処理
```css
body {
  overflow-wrap: anywhere; /* 収まらない場合に折り返す */
  word-break: normal; /* 単語の分割はデフォルトに依存 */
  line-break: strict; /* 禁則処理を厳格に適用 */
}
```