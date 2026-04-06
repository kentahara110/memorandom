
```js
const links = document.querySelectorAll('a[href^="#"]');

    // CSSと揃えたブレークポイント判定
    const mq = window.matchMedia("(max-width: 1023px)");

    const getGap = () => {
        return mq.matches ? 55 : 90;
    };

    links.forEach((link) => {
        link.addEventListener("click", (e) => {
            e.preventDefault();

            const href = link.getAttribute("href");
            const targetSection = document.querySelector(href);
            if (!targetSection) return;

            const sectionTop = targetSection.getBoundingClientRect().top;
            const currentPos = window.scrollY;

            const gap = getGap();

            const target = sectionTop + currentPos - gap;

            window.scrollTo({
                top: target,
                behavior: "smooth",
            });
        });
    });
```

## 別ページ対応

```js
<script>
    // CSSと揃えたブレークポイント
    const mq = window.matchMedia("(max-width: 1024px)");

    const getGap = () => {
        return mq.matches ? 55 : 90;
    };

    window.addEventListener("load", () => {
        // URLに # があるかチェック
        if (window.location.hash) {
            const id = window.location.hash;
            const target = document.querySelector(id);

            if (!target) return;

            // 少し待ってからスクロール（超重要）
            setTimeout(() => {
                const gap = getGap();
                const top =
                    target.getBoundingClientRect().top +
                    window.scrollY -
                    gap;

                window.scrollTo({
                    top: top,
                    behavior: "smooth",
                });
            }, 100); // ← これがないとズレることある
        }
    });
</script>
```