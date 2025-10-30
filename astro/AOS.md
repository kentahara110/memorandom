公式サイト [https://michalsnik.github.io/aos/](https://michalsnik.github.io/aos/)


astroでの使用

```astro
in your principal layout in your head add

<link rel="stylesheet" href="https://unpkg.com/aos@next/dist/aos.css" />

and before your closing body tag

<script is:inline src="https://unpkg.com/aos@next/dist/aos.js"></script>
<script is:inline>
    document.addEventListener("DOMContentLoaded", function () {
        AOS.init({
        duration: 400, // オプション
        easing: "ease-out", // オプション
        once: true, // オプション
        offset: 20, // オプション
        delay: 100 // オプション
        });
    });
</script>

```

あとは各ページやコンポーネントの要素に公式の属性をつけるだけ