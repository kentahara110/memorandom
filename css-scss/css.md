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