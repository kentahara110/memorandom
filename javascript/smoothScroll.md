
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