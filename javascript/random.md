# random

## 概要

`Math.random` 関数は 0以上1以下の浮動小数を返します。
JavaScriptには初期状態で指定された範囲の整数による乱数を返す関数がないため、
ランダムで画像を表示する際など、下記のような関数を予め作成する必要があります。

```javascript
const rnd = (min,max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

console.log(rnd(1,6))
```

あるいは以下のように `Math.floor` 関数を使わずに書くことも可能です。
最後のビット演算の `|0` により、返り値は整数化されます。

```javascript
const rnd = (min,max) => {
  return Math.random() * (max - min + 1) + min |0
}

console.log(rnd(1,6))
```