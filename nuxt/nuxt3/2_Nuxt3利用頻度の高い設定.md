# Nuxt3プロジェクトの作成と利用頻度の高い設定
[参考サイト](https://qiita.com/amishiro/items/478576fa1b21a224d81e)

## Node.jsのインストール
Node.js v18以上をインストール

## Nuxtプロジェクトを作成
Nuxtをインストール
```
% npx nuxi@latest init <project-name>
```
パッケージマネジャーを選択　ナイスのプロジェクトではyarnを選ぶことが多い
```
❯ Which package manager would you like to use?
○ npm
○ pnpm
● yarn
○ bun
```
開発モードで起動して動作確認
```
% cd <project-name>
% yarn run dev
```

## envファイルの作成
納品先ディレクトリを変更できるように.envファイルを作成

プロジェクトルートに.envファイルを作成
```
# .env

# ディレクトリ構造に合わせる
# NUXT_APP_BASE_URL=/my-dir
```
nuxt.config.tsで.envを読み込みベースのディレクトリを指定
```
// nuxt.config.ts

export default defineNuxtConfig({
  ...
  app: {
    // ベースのディレクトリ
    baseURL: process.env.NUXT_APP_BASE_URL,
	...
  },
  ...
})
```

## 本番環境・ステージング環境・開発環境で分けたい
nuxt3はdotenvが入っているためカスタムファイル（.env.localなど）が利用できます。各種envファイルを作成し、package.jsonへコマンドを登録しておきます。

```
// package.json

{
  ...
  "scripts": {
    ...
    "dev": "nuxt dev --dotenv .env.local",
     ...
    "generate": "nuxt generate --dotenv .env.dev",
    "generate:stg": "nuxt generate --dotenv .env.staging",
    "generate:prd": "nuxt generate --dotenv .env.production",
    ...
  },
}
```

## sassのインストール
sassをインストール
```
% yarn add -D sass
```

sassグローバルスタイルを作成してプロジェクト全体に適用するスタイルを設定
```
// 例としてvaliabesファイルを生成
% mkdir -p assets/scss
% touch assets/scss/_valiabes.scss
```

nuxt.config.tsで読み込む
```
// nuxt.comfog.js
export default defineNuxtConfig({
  ...
  vite: {
    css: {
      preprocessorOptions: {
        scss: {
          additionalData: 
          `
            @use "~/assets/scss/_variables.scss" as *;
          `
        }
      }
    }
  },
  ...
})
```

## 最低限のページ作成
細かな設定をしていくにあたり必要なページの生成します。
```
% mkdir pages/layouts
% touch pages/index.vue layouts/default.vue
```

```
// pages/index.vue
<template>
  <div>index</div>
</template>
```

```
// layouts/default.vue
<template>
  <div>
    <header>header</header>
    <slot />
    <footer>footer</footer>
  </div>
</template>
```

```
// app.vue
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
```

## 404エラーページ
generate時にerrorページを404.htmlファイルとして生成します。
```
% touch error.vue
```

```
// error.vue
<template>
  <div>
    <NuxtLayout>
      <!-- 404 -->
      <template v-if="error && error.statusCode === 404">
        <h1>{{ error.message }}</h1>
        <h2>ページが見つかりません</h2>
        <p>
          申し訳ありませんが、お客さまがお探しのページが見つかりませんでした。
          <br />ページが移動したか、もしくは掲載期間が終了した可能性がございます。
          <br />お手数ですが、「<a @click="toTop">トップページに戻る</a
          >」を押して、ご覧になりたいページをお探しください。
        </p>
        <!-- 開発環境のみで表示 -->
        <pre v-if="isDev">{{ error }}</pre>
      </template>

      <!-- 404以外 -->
      <template v-else>
        <h1 v-if="error">{{ error.message }}</h1>
        <h2>ページが表示できません</h2>
        <p>
          ご不便をおかけし申し訳ありません。
          <br />正常にご覧いただけるよう、解決に取り組んでおります。
          <br />しばらく時間をおいて再度ご覧ください。
        </p>
        <small v-if="error">{{ error?.statusCode }} error</small>
        <!-- 開発環境のみで表示 -->
        <pre v-if="isDev">{{ error }}</pre>
      </template>

      <br />

      <!-- トップへ戻る -->
      <button @click="toTop">トップページに戻る</button>
    </NuxtLayout>
  </div>
</template>

<script setup lang="ts">
defineProps({
  error: {
    type: Object,
    default: () => ({}),
  },
})

/**
 * 開発環境かどうかのチェック
 */
const isDev = import.meta.dev

/**
 * エラーを削除してトップへ遷移
 */
function toTop() {
  clearError({ redirect: '/' })
}
</script>

```

Apacheサーバーのために.htaccessを準備しておきます。
.htaccessはmod_rewriteで記述しています。サーバー仕様に合わせて変更してください。サーバーの管理画面から変更できる場合はそちらも検討してください。

```
touch public/.htaccess
```

```
<IfModule mod_rewrite.c>

# RewriteEngineをOn、ドキュメントルートを設定する ---
RewriteEngine on
RewriteBase /

# 404リダイレクト ---
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^.*$ 404.html

</IfModule>
```

## メタタグ nuxt.config.tsのapp:[]内に
```
head: {
    charset: 'utf-8',
    viewport: 'width=device-width,initial-scale=1',
    title: '認知機能タイプチェック',
    meta: [
                { name: 'description', content: '認知症世界の歩き⽅「MNTI」は、30の質問に答えることであなたの認知機能の凸凹を可視化し、関連する知識と対処法を学ぶことができるウェブサービスです。' },
                { name: 'format-detection', content: 'telephone=no' },
                // OGP
                { property: 'og:site_name', content: '認知機能タイプチェック' },
                { property: 'og:url', content: 'https://issueplusdesign.jp/dementia_world/mnti/' },
                { property: 'og:type', content: 'website' },
                { property: 'og:title', content: '認知機能タイプチェック' },
                { property: 'og:description', content: '認知症世界の歩き⽅「MNTI」は、30の質問に答えることであなたの認知機能の凸凹を可視化し、関連する知識と対処法を学ぶことができるウェブサービスです。' },
                { property: 'og:image', content: 'https://issueplusdesign.jp/dementia_world/mnti/og.png' },
                { property: 'og:locale', content: 'ja_JP' },
                // Twitter
                { name: 'twitter:card', content: 'summary_large_image' },
                { name: 'twitter:description', content: '認知症世界の歩き⽅「MNTI」は、30の質問に答えることであなたの認知機能の凸凹を可視化し、関連する知識と対処法を学ぶことができるウェブサービスです。' },
                { name: 'twitter:image:src', content: 'https://issueplusdesign.jp/dementia_world/mnti/og.png' }
            ],
    // link: [{ rel: 'icon', href: '/icon.png' }],
    script: [
        {
            hid: 'adobe-fonts',
            innerHTML: `(function(d) {
                var config = {
                kitId: 'ouz2hmf',
                scriptTimeout: 3000,
                async: true
                },
                h=d.documentElement,
                t=setTimeout(function(){
                h.className=h.className.replace(/\\bwf-loading\\b/g,"")+" wf-inactive";
                },config.scriptTimeout),
                tk=d.createElement("script"),
                f=false,
                s=d.getElementsByTagName("script")[0],
                a;
                h.className+=" wf-loading";
                tk.src='https://use.typekit.net/'+config.kitId+'.js';
                tk.async=true;
                tk.onload=tk.onreadystatechange=function(){
                a=this.readyState;
                if(f||a&&a!="complete"&&a!="loaded")return;
                f=true;
                clearTimeout(t);
                try{Typekit.load(config)}catch(e){}
                };
                s.parentNode.insertBefore(tk,s)
            })(document);`,
            type: 'text/javascript'
        },
        {
            src: "https://www.googletagmanager.com/gtag/js?id=G-29Z0DCE3RK",
            async: true,
        },
        {
            children: `
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());
            gtag('config', 'G-29Z0DCE3RK');
            `,
            type: "text/javascript",
        },
    ],
},
```


## SEO対策
[@nuxtjs/seoの推奨構成](https://nuxtseo.com/docs/nuxt-seo/guides/using-the-modules)
```
yarn add -D @nuxtjs/seo
```

```
// nuxt.config.ts
export default defineNuxtConfig({
	...
  modules: [
		...
    // https://nuxtseo.com/
    '@nuxtjs/seo',
  ],
  
  // nuxtjs/seoの設定
  site: {
    defaultLocale: 'ja',
    url: 'http://localhost:3000',
    name: '株式会社サイト名',
    description: '共通のディスクリプション', 
    // URL末尾にスラッシュを付与する
    trailingSlash: true, 
  },
  ...
})
```
yarn generateを実行してdistフォルダ内を確認すると、sitemap.xmlをはじめ、必要なファイル群が書き出されていることが確認できます。
あとは、お好みで各種設定を追加していきます。

## URL末尾にスラッシュを付与する
Nuxtは、ディフォルトではURLの末尾に/がつきません。先ほどのnuxtjs/seoの設定で、trailingSlash: trueを追加することで末尾のスラッシュを強制できます。

Tips：defineNuxtLinkのオプションでも末尾スラッシュの追加は可能ですが、こちらの方がお手軽です。

## 本番環境・ステージング環境・開発環境で分けたい
urlをはじめとした環境ごとに違う設定を.envファイルで管理します。

baseURLの項で作成した、.envのカスタムファイル（.env.local``.env.staging``.env.production）に以下を追記します。
```
# .env.local | dev | staging | production

# ディレクトリ構造に合わせる
# NUXT_APP_BASE_URL=/my-dir

# 環境のURL
NUXT_SITE_URL=http://localhost:3000 # or https:staging.example.com | https://example.com

# 環境
NUXT_SITE_ENV="local" # or | dev | staging| production
```

## robots.txtの設定
[@nuxtjs/robots](https://nuxtseo.com/docs/robots/getting-started/installation)のドキュメントを参考に設定します…。が、ほとんど場合何もしなくて大丈夫です。後で調べ直さなくていいように、例を記載しておきます。

注意点としては、NUXT_SITE_ENV === 'production’以外では<meta name="robots" content="noindex, nofollow"> が挿入されインデックスされません。書き出されたファイルをちゃんとチェックしないと、後で泣きます。
```
// nuxt.config.ts

export default defineNuxtConfig({
  ...
  // robots.txt(nuxt/seo)
  // memo: NUXT_SITE_ENV === 'production' 以外では全て無効化される
  robots: {
    // disallow: ['/admin'], // 無効化リスト
  },
  ...
})
```

## sitemap.xmlの設定
[@nuxtjs/sitemap](https://nuxtseo.com/docs/sitemap/getting-started/installation)のドキュメントを参考に設定します…。が、こちらも、ほとんど場合何もしなくて大丈夫です。

個人的に余分なファイルは出力したくないため、sitemap.xml用のスタイルシートとクレジットを無効化しておきます。

Tips：robots.disallowで指定したパスはサイトマップに含まれないので安心です。
```
// nuxt.config.ts

export default defineNuxtConfig({
  ...
  // sitemap.xml(nuxt/seo)
  // memo: robots.disallowで指定したパスはサイトマップに含まれない
  sitemap: {
    xsl: false, // sitmap.xmlのスタイルシートを無効化
    credits: false, // サイトマップのクレジットを無効化
  },
  ...
})
```

## og:imageの設定
[nuxt-og-image](https://nuxtseo.com/docs/og-image/getting-started/installation)のドキュメントを参考に設定します…。が、OGP画像の自動生成を実施するとパフォーマンスが重いため扱いに気をつけましょう。

[@nuxt/content](https://content.nuxt.com/)の利用を想定している場合などはとても強力なツールですが、自動生成を利用しない場合は、素直にuseHeadやuseSeoMetaを利用した方が楽なので、プロジェクトに合わせて検討をしてください。

Tips：2024/1/22現在、baseURLを指定してるとgenerate時にogp画像へのリンクが壊れます。今後の開発に期待。

設定サンプルを記載しておきます。
```
// nuxt.config.ts

export default defineNuxtConfig({
  ...
  // og:image(nuxt/seo)
  // memo: 無効化したい場合は、enabled: falseを指定
  // memo: 変更が反映されない場合はキャッシュの無効化を試す。
  ogImage: {
    // enabled: false, // 無効化
    // runtimeCacheStorage: false, // キャッシュの無効化
    fonts: ['Noto+Sans+JP:700'], // 日本語使えるフォントを指定
  },
  ...
})
```

```
// app.vue 全ページ共通の画像ベース例

<script lang="ts" setup>
const siteConfig = useSiteConfig()
defineOgImage({
  url: 'https://amiten.co.jp/ogp/home.png', // 絶対URLを利用
  alt: siteConfig.name,
})
</script>
```

```
// pages/hoge.vue 下層ページで画像ベース例

<script lang="ts" setup>
defineOgImage({
  url: 'https://amiten.co.jp/ogp/hoge.png', // 絶対URLを利用
})
</script>
```

```
// pages/fuga.vue 下層ページで自動生成の例
// Nuxt DevToolsにアクセスしたら、コマンドパレット(Ctrl+K)を開いて「og」と入力することで、`OG Image`タブに移動できます。

<script lang="ts" setup>
useSeoMeta({
  title: 'fugaページ',
  description: 'これはfugaページのディスクリプションです',
})

defineOgImageComponent('Nuxt', {
  headline: 'Fugas-Category',
})
</script>
```

## schema.orgの設定
[nuxt-schema-org](https://nuxtseo.com/docs/schema-org/getting-started/installation)のドキュメントを参考に設定します…。が、こちらも、ほとんど場合何もしなくて大丈夫です。設定するのは、組織のサイトか個人サイトかの指定部分ぐらいでしょうか？

参考：[ページ詳細の設定](https://nuxtseo.com/docs/schema-org/guides/nodes)
```
// nuxt.config.ts

export default defineNuxtConfig({
  ...
  // schema.org(nuxt/seo)
  schemaOrg: {
    identity: 'Organization', // or 'Person'
  },
  ...
})
```

## Nuxt/Seoのその他機能
[Nuxt Link Checker](https://nuxtseo.com/docs/link-checker/getting-started/installation)
リンクやURLのチェックをしてくれます。個人的には細かく設定してチェックをしたいのだけど、プロジェクト毎に変わるので割愛。

Nuxt SEO Experiments
実験的機能なので割愛。個人的には好きな方向性でもパフォーマンス重そう。

ディフォルトが「有効」になっているので、両方とも無効化する。

```
// nuxt.config.ts

export default defineNuxtConfig({
  ...
  // linkChecker(nuxt/seo)
  linkChecker: { enabled: false },
  // 実験的機能(nuxt/seo)
  seoExperiments: { enabled: false },
  ...
})
```

## favicon.icn
nuxtのディフォルト機能で設定します。

各種ブラウザ用のファビコンfavicon.iconを作成し、staticディレクトリ直下へ設置します。
```
// nuxt.config.ts
export default defineNuxtConfig({
  ...
  app: {
    ...
    head: {
      link: [
        // favicon
        {
          rel: 'icon',
          type: 'image/x-icon',
          href: `${process.env.NUXT_APP_BASE_URL ?? ''}/favicon.ico`,
        },
      ],
    },
    ...
  },
  ...
})
```

## GTMの設置
[@zadigetvoltaire/nuxt-gtm](https://nuxt.com/modules/nuxt-gtm)のドキュメントを参考に設定します…。が、こちらも、ほとんど場合何もしなくて大丈夫です。でも、IDは必須です。
```
% yarn add -D @zadigetvoltaire/nuxt-gtm
```

```
// nuxt.config.ts

module.exports = {
  modules: [
    ...
    // https://github.com/nuxt-community/gtm-module
    '@zadigetvoltaire/nuxt-gtm',
  ],
  ...
  // nuxt-gtm(IDは各自変更)
  gtm: {
    id: 'GTM-XXXXXXXX',
  },
}
```
gtmを利用した広告関連のためにタイトルとパスを渡す必要が出てくることがあります。

GTMへのイベント発行用composableを記載しておきます。
↓
```
// composables/gtmEvemt.ts

interface GtmEventOptions {
  title: string
  path: string
}

/**
 * GTMへのイベント発行
 * @description
 * - gtmを利用した広告関連のためにタイトルとパスを渡す
 */
export const gtmEvent = (options: GtmEventOptions) => {
  onMounted(() => {
    nextTick(() => {
      // gtmの取得
      const gtm = useGtm()
      if (!gtm) return

      // gtmの発火
      gtm.trackEvent({
        event: 'loadready', // event名はgtm管理者へ確認
        trackPageTitle: options.title,
        trackPageview: options.path,
      })
    })
  })
}
```

## GAの場合
[Nuxt Gtag](https://nuxt.com/modules/gtag)を参照してください。

## ハッシュリンクのスクロール動作
[ドキュメント](https://nuxt.com/docs/guide/recipes/custom-routing#scroll-behavior-for-hash-links)を参考に入れてください。
```
// nuxt.config.ts
export default defineNuxtConfig({
  ...
  router: {
    options: {
      scrollBehaviorType: 'smooth'
    }
  }
  ...
})
```