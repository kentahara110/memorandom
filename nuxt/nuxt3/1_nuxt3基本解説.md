# Nuxt3概要
Vue.jsをベースにしたフレームワーク
Vue.jsのUIだけでなくSSRを含むレンダリング機能、metaタグ、ルーティング、エラーハンドリングなどの機能を事前に組み込むことでアプリケーション開発を効率的に行う。
ルーティングであれば決められたディレクトリにファイルを作成するだけで自動でルーティングが設定される。
コンポーネントを利用する場合にimport文を使わなくても自動でimportされる
Typescriptもサポート

# Nuxt3解説
[参考サイト](https://reffect.co.jp/vue/nuxt3)

## プロジェクト作成
インストール
```
npx nuxi@latest init <project-name>
```

パッケージマネジャーを選択　ナイスではyarnを利用することが多い
```
❯ Which package manager would you like to use?
○ npm
○ pnpm
● yarn
○ bun
○ deno
```

起動確認
```
yarn dev
```

## 初期画面のメッセージ
app.vueファイルからNuxtWelcome, NuxtRouteAnnouncerコンポーネントを削除して”Hello World”を記述します。

```
// app.vue
<template>
  <h1>Hello World</h1>
</template>
```
ブラウザで確認すると”Hello World”が確認できます。

app.vueファイルはNuxt 3のメインコンポーネントです。

複数のページを作成する場合は pages ディレクトリを利用しますが pages ディレクトリはオプションなので必ず利用する必要はありません。

ランディングページのみ、ルーティングが必要ないアプリケーションの場合は、app.vue ファイルを利用するだけでアプリケーションを構築することができます。app.vue ファイルのみ利用した場合にはルーティングのvue-router をバンドルする必要がないためビルド後のバンドルファイルのサイズを減らすことができます。

バンドルサイズが小さくなることでクライアントが受け取るデータ量も減るためページの表示速度の高速化につながります。

## 補足　app.vue以外のファイル
<details>
<summary>app.vue以外のファイル</summary>
app.vue ファイル以外に nuxt.config.ts, tsconfig.json, README.md, package.jsonファイルなどがあります。

nuxt.config.tsはNuxtに関する設定を行うことができるファイルで本文書でもnuxt.config.tsファイルを利用して設定を行います。拡張子が ts となっている通りデフォルトから TypeScript に対応しています。

tsconfig.json は TypeScript の一般的な設定ファイルです。README.mdはインストールコマンドなどの説明が記述されています。
</details>

## ディレクトリ構成
Nuxt 3ではプロジェクト作成時に node_modules, public, server 以外のディレクトリは存在しませんがそれぞれ異なる役割を持っているディレクトリを追加していくことでアプリケーションを構築していきます。

ディレクトリ名はNuxt 3の中で事前に決められておりコンポーネントを利用する場合は componentsディレクトリを作成します。

componentsディレクトリの下にコンポーネントファイルを保存することで Auto Imports 機能により import 文を利用しなくても自動でコンポーネントの import が行われます。そのためディレクトリ名や作成したディレクトリがどのような役割を持っているのかをディレクトリ毎に理解しておく必要があります。

## pagesディレクトリ
app.vue ファイルだけでアプリケーションを構築することができるということは説明しましたがNuxtを利用する開発者は通常複数ページで構成されたアプリケーションを構築します。

ページを追加したい場合にはpagesディレクトリを利用します。Nuxtではファイルベースルーティングを採用しているので pages ディレクトリの中にファイルを作成するだけで自動でルーティングが設定されます。

pagesディレクトリはデフォルトでは存在しないのでpagesディレクトリを作成してその下にindex.vueファイルを作成して下記のコードを記述してください。

```
//pages/index.vue
<template>
  <h1>Main Page</h1>
</template>
```
pages/index.vueファイルを作成しただけでブラウザ上の表示が変わるわけではなりません。

pages/index.vueファイルの内容を表示させるためにはapp.vueファイルにNuxtPageコンポーネントを追加する必要があります。

```
// app.vue
<template>
  <div>
    <NuxtPage />
  </div>
</template>
```
NuxtPage タグを追加すると NextPage タグを追加した箇所に pages/index.vue ファイルのコンテンツが表示されます。説明通りに設定したにも関わらずコンテンツが表示されず画面が真っ白の場合は起動コマンドを再実行してください。

さらにpagesディレクトリにabout.vueファイルを作成しますが試しに”npx nuxi add page”コマンドを利用してみましょう。
```
% npx nuxi add page about
ℹ 🪄 Generated a new page in /Users/mac/Desktop/nuxt3-app/pages/about.vue
```
“npx nuxi add page”コマンドの利用方法がわかったのでabout.vueファイルの内容を以下のコードで上書きします。

```
// about.vue
<template>
  <h1>About Page</h1>
</template>
```

about.vueファイルを作成後にブラウザのURLに`http://localhost:3000/about`を直接入力すると”About Page”が表示されます。自動でルーティングが設定されていることがわかります。もしNuxtに自動ルーティング機能が備わっていない場合はルーティングファイルを利用して/aboutにアクセスがあった場合に表示させるコンポーネントを指定する必要があります。

pages ディレクトリにファイルを作成するとルーティングが自動で設定されること、コンテンツを表示するためにはメインファイルである app.vue ファイルにNuxtPageタグが必要であることがわかりました。

## layoutsディレクトリ
index.vue ファイル、about.vue ファイルを作成しましたがaboutページにアクセスする場合は手動でURLを設定する必要があります。ページ間を移動するためにナビゲーションを追加します。

ページ間を移動できるナビゲーションを追加したい場合に両方のファイルに同じ内容を追加することは非効率です。レイアウトファイルを作成することでページで共通する内容を1つのファイルにまとめることができ、レイアウトファイルを共有するすべてのページに適用することができます。レイアウトファイルにはヘッダーやフッター、サイドバーなど複数のページで共有できる内容を記述します。

レイアウトを利用する場合はlayouts ディレクトリを作成して default.vue ファイルを作成してください。<slot />は必須です。
```
// layouts/default.vue
<template>
  <div>
    <nav>ここにナビゲーションバーを入れる</nav>
    <slot />
  </div>
</template>
```
layoutsフォルダにdefault.vueファイルを作成してだけでは何も変化はありません。app.vueファイルにNuxtLayoutタグを追加しNuxtPageタグをラップします。
```
// app.vue
<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
```

app.vueファイルにNuxtLayoutタグを追加することで自動でlayoutsディレクトリに存在するdefault.vueファイルの内容が適用されます。

`http://localhost:3000/`だけではなく`http://localhost:3000/about`ページにアクセスしてもdefault.vueファイルの内容が適用されていることが確認できます。

もしレイアウトをある特定のページで適用したくない場合にはdefinePageMeta関数で制御することができます。pages/about.vueファイルにレイアウトを適用したくない場合はdefinePageMega関数の引数にlayoutプロパティを持つオブジェクトを追加しlayoutの値をfalseに設定します。設定するとabout.vueファイルのみレイアウトの適用が行われなくなります。index.vueファイルについてはdefault.vueのレイアウトが適用されたままになります。
```
// pages/about.vue
<script setup>
definePageMeta({
  layout: false,
});
</script>
<template>
  <h1>About Page</h1>
</template>
```

## カスタムレイアウトファイル
layoutsフォルダにdefault.vueファイルを作成することで自動でレイアウトとして適用されましたがページ毎に異なるレイアウトを適用したい場合の方法を確認します。

layoutsフォルダにcustom.vueファイルを作成します。defaultとは異なり任意の名前をつけることができます。
```
// layouts/custom.vue
<template>
  <div>
    <nav>カスタムレイアウト</nav>
    <slot />
  </div>
</template>
```
追加したcustome.vueファイルのレイアウトを利用したい場合もdefinePageMeta関数を利用します。引数のオブジェクトのlayoutプロパティの値にlayoutsディレクトリにあるファイル名を指定します。ここでは作成したcustomを指定します。
```
// pages/about.vue
<script setup>
definePageMeta({
  layout: 'custom',
});
</script>
<template>
  <h1>About Page</h1>
</template>
```

全体にデフォルトで設定するレイアウトファイルをdefault.vueからcustom.vueファイルに変更したい場合にはNuxtLayoutタグのname propsを利用することができます。
```
// app.vue
<template>
  <NuxtLayout name="custom">
    <NuxtPage />
  </NuxtLayout>
</template>
```
“/”にアクセスするとlayouts/default.vueファイルのレイアウトではなくlayouts/customのレイアウトが適用されます。

v-bind を name Props を利用することで動的にレイアウトを変更することもできます。admin が true の場合は custom.vue ファイル、admin が false の場合は default.vue ファイルが適用されます。

```
// app.vue
<script setup>
const admin = true;
const layout = admin ? 'custom' : 'default';
</script>
<template>
  <NuxtLayout :name="layout">
    <NuxtPage />
  </NuxtLayout>
</template>
```

## 補足　NuxtLayoutコンポーネント

<details>
<summary>NuxtLayoutコンポーネント</summary>

レイアウトファイルの設定についてはabout.vueファイルに直接NuxtLayoutコンポーネントを利用することもできます。name propsにレイアウトファイル名を設定することができますがここではname propsを設定していないのでデフォルトのレイアウトが適用されます。
```
// pages/about.vue
<template>
  <div>
    <NuxtLayout>
      <h1>About Page</h1>
    </NuxtLayout>
  </div>
</template>
```
pageファイルでNuxtLayoutを利用する場合はNuxtLayoutタグがルートエレメントにならないように設定することが推奨されています。pages/about.vueファイルのルートエレメントはdivにしています。

app.vueファイルにもNuxtLayoutタグが利用されていることを確認しておきます。

ブラウザで/aboutに確認すると2回デフォルトのレイアウトが適用されることになります。

ページファイルへのレイアウトファイルの適用

about.vueのNuxtLayoutにname propsを追加しcustomレイアウトの設定のみ設定後、customレイアウトのみ反映させたい場合はdefinePageMeta関数でlayoutをfalseに設定します。app.vueファイルで適用されているdefaultレイアウトは適用されず、customレイアウトのみ適用されます。
```
// pages/about.vue
<script setup>
definePageMeta({
  layout: false,
});
</script>
<template>
  <div>
    <NuxtLayout name="custom">
      <h1>About Page</h1>
    </NuxtLayout>
  </div>
</template>
```

補足
app.vue ファイルは必須ではないので削除できる
app.vue ファイルが存在しない場合には自動でデフォルトのレイアウトが適用される
</details>


## 補足　動的にLayoutの変更
<details>
<summary>動的にLayoutの変更</summary>
ページに適用するレイアウトを動的に変更したい場合に setPageLayout 関数を利用することもできます。

最初は definePagaMeta 関数で layout を false に設定しているのでデフォルトのレイアウトが設定されていない状態です。画面上に表示される”Update layout”ボタンをクリックすると custom レイアウトが適用されます。レイアウトの適用に setPageLayout 関数を利用しています。
```
// pages/about.vue
<script setup>
function enableCustomLayout() {
  setPageLayout('custom');
}
definePageMeta({
  layout: false,
});
</script>
<template>
  <div>
    <button @click="enableCustomLayout">Update layout</button>
  </div>
</template>
```

useRoute関数を利用してroute.meta.layoutの値を設定することでも動的に変更することは可能です。
```
pages/index.vue
<script setup>
const route = useRoute();
const enableCustomLayout = () => {
  route.meta.layout = 'custom';
};
definePageMeta({
  layout: false,
});
</script>
<template>
  <div>
    <button @click="enableCustomLayout">Update layout</button>
  </div>
</template>
```
definePageMeta関数を利用しない場合はデフォルトのlayoutが適用されておりボタンをクリックするとcustomのレイアウトが適用されます。

</details>

## 補足　名前付きslotの設定
<details>
<summary>名前付きslotの設定</summary>

app.vue ファイルを削除し layouts ディレクトリに default.vue ファイルが存在し index.vue ファイルにデフォルトのレイアウトが適用された状態で slot の設定の動作確認を行います。

名前付きslotを利用するためにapp.vueファイルを削除しないといけないわけではありません。

```
// layouts/default.vue
<template>
  <div>
    <nav>こにナビゲーションバーを入れる</nav>
    <slot />
  </div>
</template>
```

```
// pages/index.vue
<template>
  <h1>Main Page</h1>
</template>
```

default.vueファイルで名前付きslotを追加します。slotタグにname propsを追加し任意の名前をつけています。

```
// layouts/default.vue
<template>
  <div>
    <slot name="header" />
    <nav>こにナビゲーションバーを入れる</nav>
    <slot />
  </div>
</template>
```

index.vueファイルではdefinePageMeta関数でlayoutプロパティをfalseに設定しNuxtLayoutコンポーネントを追加します。nameにはdefaultを設定します。templateタグに#headerを設定しタグの中に名前付きslotを設定した場所に表示させたい内容を記述します。

```
// pages/index.vue
<script setup>
definePageMeta({
  layout: false,
});
</script>
<template>
  <div>
    <NuxtLayout name="default">
      <template #header>ヘッダー</template>
      <h1>Main Page</h1>
    </NuxtLayout>
  </div>
</template>
```
templateタグで設定した”ヘッダー”が表示されることがわかります。

名前付きSlotの設定方法
NuxtLayoutに明示的にname propsでdefaultを設定しない場合はdefinePageMeta関数の設定によりdefaultのレイアウトが適用されないのでnameの設定が必須になります。またNuxtLayoutタグがない場合は、エラーになります。

</details>

## componentsディレクトリ
app.vue ファイルは削除して layouts ディレクトリに default.vue ファイルがある状態で動作確認を行っています。
```
// pages/index.vue
<template>
  <h1>Main Page</h1>
</template>
```
再利用可能なコンポーネントファイルを保存するためにcomponentsディレクトリを作成します。

作成したcomponentsディレクトリにNavbar.vueファイルを作成します。Navbar.vueファイルはdefault.vueファイルでナビテーションバーを設定するために利用します。

```
// components/navbar.vue
<template>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</template>
```

作成した Navbar コンポーネントを layouts の default.vue ファイルに追加します。Vue.js では別ファイルに記述したコンポーネントを利用する場合には import を行う必要があります。Nuxt 3 では components ディレクトリに保存したコンポーネントファイルは自動で import されるため import 処理を行う必要がありません。
```
// layouts/default.vue
<template>
  <div>
    <Navbar />
    <slot />
  </div>
</template>
```

設定後は上部にリンクが表示されます。Home、Aboutそれぞれにaタグでリンクが貼られているのでクリックするとページの移動を行うことができます。

## 補足　ComponentsのLazy Loading(遅延読み込み)
<details>
<summary>ComponentsのLazy Loading(遅延読み込み)</summary>

Nuxt のコンポーネントは Lazy Loading という機能を持っているため必要な時だけ Lazy Loading を設定したコンポーネントに対応する JavaScript ファイルをダウンロードすることができます。Lazy Loading の機能を利用することで ページアクセス時にダウンロードするJavaScript のサイズを小さくすることができます。Lazy LoadingはDynamic Importsとも呼ばれます。

コンポーネントの Lazy Loading を利用した時としない時の動作をネットワークタブを見ながら確認します。

components フォルダに Coupon.vue ファイルを作成します。コードの中身を下記の通りです。
```
coupon.vue
<template>
  <div>Couponコード:1234</div>
</template>
```

index.vueファイルで作成したCouponコンポーネントを利用します。ref 関数を利用して定義した show 変数と v-if ディレクトリを利用してページが表示された時は Coupon コンポーネントは非表示にし、ボタンをクリックした時に表示できるように設定します。
```
// pages/index.vue
<script setup>
const show = ref(false);

const handleClick = () => {
  show.value = true;
};
</script>
<template>
  <div>
    <h1>Main Page</h1>
    <button @click="handleClick">Coupon Get</button>
    <Coupon v-if="show" />
  </div>
</template>
```

デベロッパーツールのネットワークタブを開いてページを表示します。一連のダウンロードファイルの中に Coupon.vue ファイルが含まれていることが確認できます。

CouponコンポーネントのダウンロードボタンをクリックするとCoupon.vueファイルの内容が表示されます。

次にLazy Loadingを確認するためにCouponの前にLazyを追加したタグに変更します。
```
// pages/index.vue
<script setup>
const show = ref(false);

const handleClick = () => {
  show.value = true;
};
</script>
<template>
  <div>
    <h1>Main Page</h1>
    <button @click="handleClick">Coupon Get</button>
    <LazyCoupon v-if="show" />
  </div>
</template>
```
ページを開いてネットワークタブを見ても Copon.vue ファイルは見つかりません。ページ上の”Coupon Get”ボタンをクリックしてください。ボタンをクリックした後にネットワークタブを見ると最後に Coupon.vue ファイルが追加されることがわかります。このように Lazy Loading を利用することでコンポーネントが必要な時のみLazy Loadingを設定したコンポーネントに関連するJavaScriptファイルをダウンロードさせることができます。

Lazy Loadingの動作確認が終わったらindex.vueファイルを元のコードに戻しておきます。

</details>

## ナビゲーション設定
a タグを利用してもページを移動することはできますが a タグではページを移動する度にページの再読み込みが行われます。クリックする度にページ全体を読み込みをするのではなくページの中で更新が必要な箇所だけ更新が行われるように a タグから NuxtLink コンポーネントを利用するために NuxtLink タグに変更します。
```
// components/navbar.vue
<template>
  <nav>
    <NuxtLink to="/">Home</NuxtLink>
    <NuxtLink to="/about">About</NuxtLink>
  </nav>
</template>
```

a タグから NuxtLink コンポーネントに変更するとページ全体が再読み込みされなくなるためページの移動がスムーズに行われるようになります。デベロッパーツールで確認すると NuxtLink タグを利用した場所には a タグが設定されていることが確認できますが JavaScript によって本来の a タグとは異なる動作になるように制御されています。NuxtLink で to props に設定できる値は内部リンクだけではなく外部のサイトを指定することもできます。外部のサイトの場合は通常の a タグのように動作し、内部リンクのようにスムーズにページが移動するということはありませんが内部リンクと外部サイトのリンクでタグを使い分ける必要はありません。また a タグのように target props(target 属性)を設定することもできます。
```
// components/navbar.vue
<template>
  <nav>
    <NuxtLink to="/">Home</NuxtLink>
    <NuxtLink to="/about">About</NuxtLink>
    <NuxtLink to="https://google.com" target="_blank">Google</NuxtLink>
  </nav>
</template>
```

URLの設定はto propsで行なっていますがtoのaliasとしてhref propsも利用可能です。ここまでの動作確認で分かる通りNuxt 3でリンクを設定する場合にはaタグではなくNuxtLinkを利用して設定していきます。

Nuxt 3のルーティングにはVue Routerライブラリが利用されています。

## assetsディレクトリの設定
assetsディレクトリにはstylesheetsやfontsなどの情報を保存します。assetsディレクトリにcssや画像を保存しassetsディレクトリを利用した場合の動作を確認します。

## CSSファイルの設定
assetsディレクトリににcssディレクトリを作成してstyle.cssファイルを作成します。

cssファイルはNuxtの設定ファイルであるnuxt.config.tsファイルで指定することができます。

```
// nuxt.config.ts
export default defineNuxtConfig({
  devtools: { enabled: true },
  css: ['/assets/css/style.css'],
});
```

## 画像のファイルの保存
画像ファイルを assets ディレクトリに保存します。ここでは icon.png ファイルを assets フォルダの直下に」保存しています。Nuxtに関する画像は[https://nuxt.com/design-kit](https://nuxt.com/design-kit)からダウンロードすることができます。

index.vue ファイルから img タグを利用して assets ディレクトリの icon.png ファイルを指定しますがパスの先頭には”~”が必要となります。

```
// pages/index.vue
<template>
  <div>
    <h1>Main Page</h1>
    <div>
      <img src="~/assets/icon.png" alt="Nuxt3 Icon" />
    </div>
  </div>
</template>
```
デベロッパーツールの要素を確認するとsrc属性の値は/assets/icon.pngではなく先頭に/_nuxt/が追加されていることがわかります。

## publicディレクトリの設定
プロジェクト作成時から存在するpublicディレクトリには画像ファイルのような静的なファイルを保存することができます。publicディレクトリにはfavicon.iconファイルが保存されています。

assetsディレクトリでも画像を保存しブラウザ上から表示することができましたがassetsディレクトリと設定方法が異なります。

publicディレクトリにicon.pngファイルを保存します。index.vueファイルのimgタグでicon.pngを指定しますがpublicは必要ではありません。
```
// pages/index.vue
<template>
  <div>
    <h1>Main Page</h1>
    <div>
      <img src="/icon.png" alt="Nuxt3 Icon" />
    </div>
  </div>
</template>
```

## faviconの設定
publicディレクトリへの画像ファイルの設定方法がわかったのでfaviconの設定方法について確認しておきます。

Nuxt 3ではmetaタグなどのデフォルトの設定をNuxtの設定ファイルであるnux.config.tsファイルで行うことができます。metaタグについては後ほど説明を行いますがtitleとdescriptionのみ設定を行いたい場合は下記のように行うことができます。設定はすべてのページで反映されます。
```
// nuxt.config.ts
export default defineNuxtConfig({
  css: ['/assets/css/style.css'],
  app: {
    head: {
      title: 'Nuxt 3 basic',
      meta: [{ name: 'description', content: 'Nuxt 3 for beginners' }],
      link: [{ rel: 'icon', href: '/icon.png' }],
    },
  },
});
```

## composablesディレクトリ
composables ディレクトリの中に composable な関数を含むファイルを保存するとコンポーネントから import することなく(Auto imports)利用することができます。composables な関数は reactive な変数とロジックをコンポーネントから切り離して再利用できるようにした関数です。

composables な関数がどのようなものかはっきりしていない人もいる思いますので useCounter という名前の composables な関数を作成して動作確認を行います。composables な関数は Nuxt 特有の関数ではなく Vue.js でも頻繁に利用します。他のフレームワークでもどのようの機能が利用されており、 React ではカスタムフックと呼ばれます。

composables ディレクトリを作成して useCounter.ts ファイルを作成します。useCounter 関数の引数から initialValue を受け取り ref 関数の初期値として count 変数を定義しています。count 変数を更新するための inc、dec 関数を定義して return で count, inc, dec を戻すといった内容です。composables な関数には ref 関数で定義された reactive な変数とロジックのみが含まれています。
```
// composables/useCounter.ts
export function useCounter(initialValue: number) {
  const count = ref(initialValue);
  const inc = () => (count.value = count.value + 1);
  const dec = () => (count.value = count.value - 1);
  return {
    count,
    inc,
    dec,
  };
}
```

pages/index.vue ファイルで useCounter を利用したい場合には下記のように import なし(Auto imports)で利用することができます。
```
// pages/index.vue
<script setup>
const { count, inc, dec } = useCounter(100);
</script>
<template>
  <div>
    <h1>Main Page</h1>
    <div>Count:{{ count }}</div>
    <div>
      <button @click="() => inc()">increase</button>
      <button @click="() => dec()">decrease</button>
    </div>
  </div>
</template>
```

ブラウザ確認するとuseCounterの引数に設定した100が表示されincreaseボタンまたはdecreaseボタンを押すと数字が増減します。

## pluginsディレクトリ
plugins ディレクトリを作成してプラグインファイルを作成するとアプケーションの起動時に Nuxt が作成したプラグインを自動で登録を行ってくれるため登録作業を行う必要がありません。

plugins ディレクトリを作成し、もっともシンプルなプラグイン hello.ts を利用して設定方法を確認します。

引数 msg を受け取り、Hello と受け取った引数を表示するだけのプラグインです。
```
// plugin/hello.ts
export default defineNuxtPlugin(() => {
  return {
    provide: {
      hello(msg: string) {
        return `Hello ${msg}!`;
      },
    },
  };
});
```
pluginsディレクトリにプラグインを作成するだけでコンポーネントから利用することができます。index.vueでcompsablesのuseNuxtApp関数を実行してプラグイン$helloを取り出します。

```
// pages/index.vue
<script setup>
const { $hello } = useNuxtApp();
</script>
<template>
  <div>
    <h1>Main Page</h1>
    <h2>{{ $hello('World') }}</h2>
  </div>
</template>
```
ブラウザにはプラグインhelloを利用したHello World!が表示されます。

## 階層ページの作成
About ページは pages ディレクトリの直下に about.vue ファイルを作成してブラウザから URL の/about でアクセスすることで About ページの内容を表示することができました。次は URL が/users/list というように階層になっている場合のルーティング方法について確認します。

pages ディレクトリの中に users ディレクトリを作成し、その下に list.vue ファイルを作成して以下のコードを記述します。
```
// pages/users/list.vue
<template>
  <h1>User Listページ</h1>
</template>
```
Nuxt 3が自動でルーティングを行なってくれるので、ファイル作成後ブラウザで/users/listにアクセスするとlist.vueのtemplateタグに記述した内容が表示されます。このようにNuxtを利用すると階層的なURLのルーティングも自動で行ってくれます。

追加したListページのルーティングをNavbar.vueファイルに追加します。NuxtLinkのto propsにはオブジェクトを利用してnameプロパティでルーティングの名前を利用することでも設定できます。
```
// navbar.vue
<template>
  <nav>
    <NuxtLink to="/">Home</NuxtLink>
    <NuxtLink to="/about">About</NuxtLink>
    <NuxtLink :to="{ name: 'users-list' }">User List</NuxtLink>
  </nav>
</template>
```
users-list という名前が突然出てきましたが、ルーティングの name プロパティに設定する値については NuxtDevToolsを利用してpagesの情報から確認することができます。

/users/listでアクセスを行うことができるようになりましたが/users/にアクセスした場合にページを表示したい場合はusersディレクトリの下にindex.vueファイルを作成します。index.vueファイルない状態でアクセスすると404ページが表示されます。
```
// pages/users/index.vue
<template>
  <h1>User Indexページ</h1>
</template>
```

## ページのネスト化
pagesディレクトリの下にusersディレクトリと同じ名前のusers.vueファイルを作成します。
```
// pages/users/users.vue
<template>
  <h1>Usersページ</h1>
</template>
```
users.vueファイルを作成すると/users, /users/listどちらにアクセスしてもusers.vueファイルに記述したUsersページのみ表示されるようになります。

users/listコンポーネントの中身を表示させるには親コンポーネントにあたるusers.vueファイルでNuxePageタグを追加する必要があります。
```
// pages/users.vue
<template>
  <div>
    <h1>Usersページ</h1>
    <NuxtPage />
  </div>
</template>
```
NuxtPageタグを追加後に/users/listにアクセスするとusers.vueファイルの内容とusers/list.vueファイルの内容が表示されます。list.vueファイルの内容はNuxtPageタグを追加した場所に表示されます。

## Dynamicルーティング
/users/list にアクセスすると User List ページが表示されますが、/users/1、/users/2 のように/users/以下が動的に変わる場合に/users/以下の値をパラメータとして受け取りパラメータ毎に異なるのページ内容を表示したい場合の設定方法を確認していきます。パラメータは動的に変わるので users ディレクトリに[id].vue ファイルを作成します。[]で囲んだ値を任意の名前をつけることができます。
```
// pages/users/[ID].vue
<template>
  <p>ユーザID: {{ $route.params.id }}</p>
</template>
```
idに入った値は$routeオブジェクトを利用して取得することができます。$routeオプジェクトのparamsの中にidの値が入っています。

/users/4にアクセスするとidの4がブラウザ上に表示されます。パラメータの値によってページ内容を変えることができるようになりました。

scriptタグの中でidにアクセスするために$routeではなくuseRoute関数を利用することができます。
```
// pages/users/[ID].vue
<script setup>
const router = useRoute();

console.log(router.params.id);
</script>
<template>
  <p>ユーザID: {{ $route.params.id }}</p>
</template>
```
ブラウザのデベロッパーツールのコンソールにはidの値が表示されます。

Dynamicルーティングを利用する際に`[id].vue`というファイル名だけではなく`user-[id].vue`ファイルという名前をつけることもできます。idの部分が動的に変わるのでブラウザからアクセスする場合は`/users/user-4`という形でアクセスすることになります。”user-“は固定値となります。


## Catch-all Route
pagesディレクトの下にdashboardディレクトリを作成してさらに[…slug].vueファイルを作成します。dashboard以下のすべてのURLでこのファイルの内容が表示されることになります。
```
// pages/dashboard/[...slug].vue
<template>
  <h1>Dashboard</h1>
  <p>{{ $route.params.slug }}</p>
</template>
```
/dashboard以下の100/foo/barにアクセスした場合でも[…slug].vueファイルの内容が表示されていることが確認できます。

## middlewareディレクトリ
middleware ディレクトリの下にファイルを作成することで Route Middleware の設定を行うことができます。Route Middleware を利用することでページ間の移動、サイトへのアクセス後のページが表示される前に事前に設定していた処理を行うことができます。例えばある特定のページには管理者権限を持っているユーザしかアクセスさせたくないといった場合、ページを表示する前に認証チェックを行うといったことが可能になります。認証のチェックに失敗した場合にはアクセスしたページを表示させず別のページにリダイレクトさせるといったことが行えます。

Route Middleware の動作確認を行うために middleware ディレクトリを作成しその下に auth.ts ファイルを作成します。

auth.ts ファイルでは defineNuxtRouteMiddleware 関数を利用します。defineNuxtRouteMiddleware 関数の中では to, from のオブジェクトが渡されるのでどのような値が入っているのか確認します。
```
// middleware/auth.ts
export default defineNuxtRouteMiddleware((to, from) => {
  console.log('from', from);
  console.log('to', to);
});
```

設定したmiddlewareはabout.vueファイルのdefinePageMeta関数で設定することができます。middlewareプロパティの値に作成したファイル名authを設定します。
```
// pages/about.vue
<template>
  <div>
    <h1>About Page</h1>
  </div>
</template>
<script setup>
definePageMeta({
  middleware: 'auth',
});
</script>
```
aboutページ以外のページからaboutページにアクセスするとコンソールログにfromとtoのオブジェクトが表示されます。ここでは/(ルート)ページから/aboutページに移動しています。

to, fromオブジェクトの確認
from はどのページから移動してきているのか to はどのページに移動しようとしているのかがわかります。from、to オブジェクトにはそれぞれのページのパスやパラメータなどが含まれていることがわかります。

middleware と from オブジェクトを利用することで/(ルート)ページからアクセスがあった場合のみ再度/(ルート)ページにリダイレクトさせるといったこともできます。

```
// middleware/auth.ts
export default defineNuxtRouteMiddleware((to, from) => {
  if (from.fullPath === '/') {
    return navigateTo('/');
  }
});
```

上記のコードではfrom の fullPath の値が”/“の場合には router の helper 関数である navigateTo 関数で”/“にリダイレクトさせるように設定しています。”/“から/about にページ移動すると about ページに移動することはできません。”/“以外のその他のページからは about ページにアクセスすることは可能です。

```
// middleware/auth.ts
export default defineNuxtRouteMiddleware((to, from) => {
  if (from.fullPath === '/') {
    return abortNavigation();
  }
});
```
ページ移動を中止したい場合にはaboutNavigation関数を利用することができます。

### 管理者チェック
アクセスするユーザが管理者かどうかをチェックする関数を作ることでmiddlewareを利用してページへのアクセス制限をかけることができます。管理者ではない場合は/loginにリダイレクトされ管理者の場合はそのままページが表示されます。
```
// middleware/auth.ts
export default defineNuxtRouteMiddleware((to, from) =>
  //isAdmin関数はアクセスしているユーザが管理者かどうかチェックする関数です。isAdminは存在しないので動作しません。各自が要件にあった関数を作成する必要があります。
  if (isAdmin() === false) {
    return navigateTo('/login')
  }
)
```

about ページのみに auth.ts ファイルによる middleware を設定しましたがすべてのページに設定したい場合にはファイルに global をつけることで自動で設定されます。auth.ts の場合は auth.global.ts とします。

ファイル名にglobalを追加設定した後はどのページにアクセスしてもブラウザのデベロッパーツールのコンソールに from, to オブジェクトが表示されるようになります。

補足　layoutsディレクトリのレイアウトファイルではdefinePageMeta関数によるmiddlewareの設定はできません。

midelewareディレクトリにファイルを作成するのではなくdefinePageMeta関数にinlineでmiddlewareを設定したい場合には以下のように記述することができます。/aboutページに移動した場合のみデベロッパーツールのコンソールにto, fromオブジェクトが表示されます。
```
// pages/about.vue
<script setup>
definePageMeta({
  middleware: defineNuxtRouteMiddleware((to, from) => {
    console.log('to', to);
    console.log('from', from);
  }),
});
</script>
<template>
  <div>
    <h1>About Page</h1>
  </div>
</template>
```

## Page Transitions
ページを移動する際にアニメーションを設定したい場合にPage Transitionsを利用することができます。

Nuxtの設定ファイルであるnuxt.config.tsファイルのappプロパティにpageTransitionを追加します。ページ移動のモードの設定も行います。

```
// nuxt.config.ts
export default defineNuxtConfig({
  // css: ['/assets/css/style.css'],
  app: {
    ...
    pageTransition: { name: 'page', mode: 'out-in' },
    ...
  },
});
```
設定後にapp.vueファイルにstyleを設定しますがapp.vueファイルを利用していない場合にはレイアウトファイルに設定します。
```
// layouts/default.vue
<template>
  <div>
    <Navbar />
    <slot />
  </div>
</template>
<script setup></script>
<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(1rem);
}
</style>
```

ページ上部のナビゲーションバーを利用してページを移動するとblurによる少し文字にぼかしが入り通常よりもゆっくりと文字が画面に表示されます。

Page Tranditionは設定するだけではなくページ単位またはアプリケーション全体でPage Transitionの設定を無効にすることができます。

ページ単位の場合はdefinePageMeta関数で設定を行います。
```
// pages/about.vue
<script setup lang="ts">
definePageMeta({
  pageTransition: false
})
</script>
```
アプリケーション全体ではNuxtの設定ファイルであるnuxt.config.tsファイルで設定します。
```
defineNuxtConfig({
  ...
  app: {
    pageTransition: false,
  }
  ...
}
)
```

## Meta Tagsの設定
構築したアプリケーションのページを検索エンジンに正しく認識してもらうためには SEO(Search Engine Optimization)への対応が必要になります。HtmlのMeta タグを設定することはSEOにとっても非常に重要なのでNuxt 3でのMeta タグの設定方法を確認します。

アプリケーション全体のMetaタグ設定は Nuxt の設定ファイルである nuxt.config.ts ファイルの app.head で行うことができます。アプリケーション全体に設定するとすべてページが同じ Meta タグになってしまうため、Meta タグはページ毎にも設定する必要がありその場合には composables の useHead 関数を利用することができます。さらにuseSeoMetaではSEO用のタグを設定することができます。

プロジェクト作成時には nuxt.config.ts、useHead 関数による Meta タグの設定は行われていませんが head タグを確認するとデフォルトの状態では charset が”utf-8”, viewport が”width=device-width, initial-scale=1”に設定されています。

### nuxt.config.tsによる設定
faviconsの設定の時にtitleとmetaタグでdescriptionを設定しましたが再度確認しておきます。
```
export default defineNuxtConfig({
  app: {
    head: {
      title: 'Nuxt 3 basic',
      meta: [{ name: 'description', content: 'Nuxt 3 for beginners' }],
      link: [{ rel: 'icon', href: '/icon.png' }],
    },
  },
});
```
nuxt.config.tsで設定した場合にはすべてのページで同じ内容が設定されている状態です。

### useHeadによる設定
scriptタグの中でComposableのuseMeta関数を利用をしてtitle, base, script, style, metaとlinkを設定することができます。ここではtitleとmetaのdescriptionの設定のみabout.vueファイルで行います。他のmetaタグの設定についても同様の方法で行うことができます。
```
// pages/about.vue
<template>
  <div>
    <h1>About Page</h1>
  </div>
</template>
<script setup>
useHead({
  title: 'Aboutページ',
  meta: [
    {
      name: 'description',
      content: 'Aboutページ',
    },
  ],
});
</script>
```

useHeadの設定はref関数によって動的に設定を行うこともできます。ref関数でtitleとdescriptionを定義しています。
```
// pages/about.vue
<template>
  <div>
    <h1>About Page</h1>
  </div>
</template>
<script setup>
const title = ref('Aboutページ');
const description = ref('Aboutページ');
useHead({
  title,
  meta: [
    {
      name: 'description',
      content: description,
    },
  ],
});
</script>
```

各ページでtitleにサイト名などを含めたい場合はtitleTemplateを利用することができます。

app.vueファイル、レイアウトファイルでもuseHead関数を利用することができるのでlayousディレクトリのdefault.vueファイルにtitleTemplateに関数を利用して以下のように設定します。
```
// layouts/default.vue
<template>
  <div>
    <Navbar />
    <slot />
  </div>
</template>
<script setup>
useHead({
  titleTemplate: (title) => {
    return title ? `${title} - Nuxt 3 basic` : 'Nuxt 3 basic';
  },
});
</script>
```

Dynamicルーティングで確認したidを設定する場合は下記のように行うことができます。
```
// pagees/[ID].vue
<script setup>
const router = useRoute();
useMeta({
  title: 'Nuxt 3',
  meta: [
    {
      name: 'description',
      content: `User Id: ${router.params.id}`,
    },
  ],
});
</script>
<template>
  <p>ユーザID: {{ $route.params.id }}</p>
</template>
```
titleTemplateで関数を利用しない場合はtitleの箇所に%sを設定することでtitleが設定されます。
```
useHead({
  titleTemplate: '%s - Nuxt 3 basic',
});
```

### Meta Componentsによる設定
Nuxt 3では`<Title>, <Base>, <Script>, <Style>, <Meta> , <Link>, <Body>, <HTML><Head>`タグを利用することができます。useHeadと同様にtitleとdescriptionを設定するのであれば下記のように行うことができます。
```
<template>
  <div>
    <Head>
      <Title>Aboutページ</Title>
      <Meta name="description" content="Aboutページ" />
    </Head>
    <h1>About Page</h1>
  </div>
</template>
<script setup></script>
```
レイアウトファイルに設定するとそのレイアウトファイルを利用するページすべてで設定が反映されます。

## useState
コンポーネント間やページ間で状態管理(データ共有)したい場合に composables の useState を利用することができます。useState はシンプルな状態管理に利用することができより複雑な状態管理であればライブラリの Pinia を利用することになります。

状態管理とはどのようなことかを理解するために ref 関数を利用した場合と比較します。

about.vue ファイルを利用して ref 関数で counter を定義します。ボタンにより counter の数を増やせるように設定を行います。
```
<template>
  <div>
    <Head>
      <Title>Aboutページ</Title>
      <Meta name="description" content="Aboutページ" />
    </Head>
    <h1>About Page</h1>
    <h2>Counter</h2>
    <p>Count: {{ counter }}</p>
    <div><button @click="counter++">+</button></div>
  </div>
</template>
<script setup>
const counter = ref(0);
</script>
```
Countを増やした後上部のリンクを利用して別のページに移動して再度aboutページに戻ってきてください。ブラウザ上のCountの値をクリアされて0と表示されます。このようにref関数ではページ間の移動を行うと値がクリアされ保持することができません。

ページを移動してもcountの値を保持したい場合にuseStateを利用することができます。
```
<script setup>
const counter = useState('counter', () => 0);
</script>
```
useStateの第一引数にkeyのcounterを設定して初期値として関数で0を戻しています。keyを設定することで後ほど別のページから共有したcounterにアクセスします。

ボタンをクリックして別のページに移動し再度aboutページに戻ってきてください。先ほどのref関数とは異なりcountの値が保持されていることが確認できます。

1つのページ内ではページを移動してもuseStateにより状態が保持できることがわかりました。

次は別のページからもcounterの値にアクセスできるか確認します。users.vueファイルを使って先ほど設定したキーのcounterとuseStateを利用します。

補足　counterはref関数なのでscriptタグ内でcounterの値を確認したい場合はcounter.valueでアクセスすることができます。
```
// pages/users.vue
<template>
  <div>
    <h1>Usersページ</h1>
    <p>Count:{{ counter }}</p>
    <NuxtPage />
  </div>
</template>
<script setup>
const counter = useState('counter');
</script>
```
aboutページでボタンをクリックしてcountを増やした後に/users/listページにリンクから移動します。ユーザリストページでもcountの値が保持できていることが確認できます。useStateによって状態の保持とコンポーネント間でのデータ共有が行えることがわかりました。


リンクによりaboutページからの移動ではなく/users/listページに直接アクセスを行ってください。/users/listページを開いた状態でブラウザのリロードでも問題ありません。

ブラウザで確認するとcountの値が入っていない状態でブラウザ上に表示されます。この時の値はundefinedです。定義がされていないために表示されていません。

この後、リンクを利用してaboutページに移動して戻ってくるとCounterの値が表示されます。このようにuseStateを利用した場合は定義しているコンポーネントで最初に状態の初期化を行う必要があります。

この問題を回避するためにcounter用のcomposablesを作成します。composablesディレクトリのuseCounter.tsファイルをuseStateを利用して書き換えます。useCounter.tsファイルがない場合は作成してください。
```
// composables/useCounter.ts
export function useCounter() {
  return useState(() => 0);
}
```
useCounter.tsファイルを作成後、aboutページからuseCounterを利用します。counterの初期値の0が表示され、ボタンをクリックするとcountの値が増えます。
```
<template>
  <div>
    <Head>
      <Title>Aboutページ</Title>
      <Meta name="description" content="Aboutページ" />
    </Head>
    <h1>About Page</h1>
    <h2>Counter</h2>
    <p>Count: {{ counter }}</p>
    <div><button @click="counter++">+</button></div>
  </div>
</template>
<script setup>
const counter = useCounter();
</script>
```
users.vueファイルでも同様にuseCounterを利用します。composablesでuseCounterを定義することでどのコンポーネントからcounterの値が取得できるようになります。/users/listに最初にアクセスしてもcounterの値は表示されます。
```
// pages/users.vue
<template>
  <div>
    <h1>Usersページ</h1>
    <p>Count:{{ counter }}</p>
    <NuxtPage />
  </div>
</template>
<script setup>
const counter = useCounter();
</script>
```

## Modules
Nuxt3ではModulesを利用することでNuxtプロジェクトに簡単に機能の追加を行うことができます。Nuxt3で利用できるモジュールは[https://nuxt.com/modules](https://nuxt.com/modules)で確認することができます。

## Data Fetching
アプリケーションを構築する場合に Data Fetching によりデータを取得し取得したデータをブラウザ上に表示させる必要があります。Nuxt 3 ではデータを取得するためのComposablesが事前に準備されているためそれらのComposablesを利用することができます。Nuxt 3 で利用できるデータ取得に関するComposablesは useFetch, useLazyFetch, useAsyncData, useLazyAsyncData の 4 つです。4つ以外に$fetchも利用されます。ここではそれぞれの利用方法を確認していきます。

useFetchなどのComposablesを利用するメリットには以下のようなものがあります。

useFetchはServer Side Rendering(SSR)に対応しており、サーバ側でfetchにより取得したデータをクライアント側であるブラウザに渡します。これによりクライアント側でfetchを行う必要がなくなります。useFetchの代わりにfetch関数を利用するとSSRに対応していないためサーバ側とクライアント側で2度fetchを行います。
useFetchで戻されるデータはリアクティブなデータなのでref関数で定義したリアクティブな変数に取得したデータを入れるといった処理が必要ありません。
useFetchでは戻されるデータの中にエラーなどの情報も含まれているためエラーハンドリングの処理なども用意に行うことができます。

### useFetch
useFetchはPromise を返すcomposablesで GET リクエストであれば引数に URL を指定するだけで指定したURLからデータを取得することができます。useFetchの利用場所はドキュメントでは下記のように記載されています。

useFetch is the most straightforward way to handle data fetching in a component setup function.

フォームなどのPOSTリクエストでuseFetchを利用した場合はブラウザのデベロッパーコンソールに`”index.vue:6 [nuxt] [useFetch] Component is already mounted, please use $fetch instead.”`のメッセージが表示され$fetchを利用するように促されます。上記のメッセージが表示された場合は**useFetchの利用場所として適切ではない**ことがわかります。

posts ディレクトリを作成して index.vue ファイルを作成して useFetch composablesを利用して無料で利用できる JSONPlaceHolder からデータを取得します。利用する URL は[https://jsonplaceholder.typicode.com/posts/](https://jsonplaceholder.typicode.com/posts/)でアクセスすると100件のPostデータが取得できます。

useFetch を利用してどのようなデータが戻されるのか確認を行います。
```
// pages/posts/index.vue
<script setup>
const response = await useFetch('https://jsonplaceholder.typicode.com/posts/');
console.log(response);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
  </div>
</template>
```
`/posts`にブラウザからアクセスするとブラウザのデベロッパーツールのコンソールにはuseFetch関数の実行によって戻される情報が表示されます。

戻されたデータの中には data の他にclear, error, pending, execute, refresh,status が含まれていることがわかります。

コンソールのメッセージの情報から data, error, pending, status は RefImp と表示されていることからリアクティブな状態で戻されていることがわかります。ref 関数で変数を定義した時と同じなので script setupタグ内では data.value で data に含まれる情報にアクセスすることができます。残りの execute, refresh, clear は関数です。

data には取得したデータ、error にはエラーメッセージ(エラーがある場合)、pending にはデータの取得中かどうか Boolean 値が含まれています。execute, refresh は関数なので実行することができ、実行すると再度データ取得を行うことができます。

data には 100 件の Post データ含まれているので分割代入で data のみ取り出して data にはわかりやすいように posts という別名をつけて v-for ディレクティブで展開しブラウザ上にタイトルを表示させます。
```
// pages/posts/index.vue
<script setup>
const { data: posts } = await useFetch(
  'https://jsonplaceholder.typicode.com/posts/'
);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>
```

dataだけではなくerrorの内容も確認します。errorの中身はscriptタグ内ではerror.valueで確認することができます。
```
// pages/posts/index.vue
<script setup>
const { data: posts, error } = await useFetch(
  'https://jsonplaceholder.typicode.com/post/'
);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <p v-if="error">{{ error }}</p>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>
```

ブラウザ上にエラーメッセージを表示
refresh関数を利用してデータの再取得を行うため再取得ボタンを追加します。ボタンをクリックするとrefresh関数が実行され再取得が行われます。JSONPLaceHolder上のデータに変更がないため再取得しても画面上には変化がありませんがデベロッパーツールのネットワークタブを見るとリクエストが送信されrefresh関数が動作していることを確認することができます。
```
<script setup>
const {
  data: posts,
  error,
  refresh,
} = await useFetch('https://jsonplaceholder.typicode.com/posts/');

</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <button @click="refresh()">再取得</button>
    <p v-if="error">{{ error }}</p>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>
```
pendingについてはuseLazyFetch関数の箇所で確認します。

### useAsyncData
useFetch関数はuseAsyncData関数と$fetch関数の利用を便利にしたラッパーです。そのためuseFetch関数と同じ処理をuseAsyncData関数と$fetch関数を使って行うことできます。

useFetch関数をuseAsyncData関数と$fetch関数で書き換えると以下のコードになります。
```
<script setup>
const { data: posts, error } = await useAsyncData('posts', () =>
  $fetch('https://jsonplaceholder.typicode.com/posts/')
);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <p v-if="error">{{ error }}</p>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>
```

useFetch の中では$fetchが利用されますがuseAsyncDataでは$fetch を利用せず別の関数に変更することができます。useFetch とは異なり第二引数の関数の中で処理を追加することもできます。つまり useFetch よりも useAsyncData を利用することで複雑な処理が行えるようになります。
```
const { data: posts, error } = await useAsyncData('posts', () => {
  console.log('fetch'); //追加の処理
  return $fetch('https://jsonplaceholder.typicode.com/posts/');
});
```

useAsyncData の第一引数にはキャッシュに利用されるユニークキーを設定しています。キーは省略することが可能で省略した場合は自動でキーが設定されます。その場合のキーはファイル名と行番号を利用して作成されます。

posts というキーを設定して取得した値は useNuxtApp().payload.data からアクセスすることができます。
```
<script setup>
const { data: posts, error } = await useAsyncData(() =>
  $fetch('https://jsonplaceholder.typicode.com/posts/')
);
console.log(useNuxtApp().payload.data);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <p v-if="error">{{ error }}</p>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>
```

### $fetchについて
useAsyncDataの中で突然出てきた$fetch関数について確認しておきます。

`$fetch`はヘルパー関数で内部ではofetchライブラリを利用しています。ofetchライブラリはNode, ブラウザ、ワーカー上で利用できるfetch APIです。

useFetchではSSRに対応しており、サーバ側のみfetchが行われますが$fetchを利用した場合はサーバ側、クライアント側で2度fetchが行われます。そのためuseFetchをすべての場面で使うのかというとそうではなくドキュメントでは$fetchの利用場所として下記のように記載されています。

`$fetch` is great to make network requests based on user interaction.

**クライアント側でのみ実行されるユーザのインタクションに伴うリクエストは`$fetch`を利用するのが適しています。**

#### fetch関数との違い
fetch 関数であれば使い慣れている人もいると思いますので fetch 関数との代表的な違いを確認しておきます。

fetch 関数では取得したデータを JSON Parse する必要(response.json())がありますが ofetch では自動で行ってくれます。
```
//$fetch関数
const posts = ref([]);
const data = await $fetch('https://jsonplaceholder.typicode.com/posts/');
posts.value = data;

//fetch関数
const posts = ref([]);
const response = await fetch('https://jsonplaceholder.typicode.com/posts/');
const data = await response.json();
posts.value = data;
```
fetchでPOSTリクエストを実行する際にJSON.stringifyでJSON形式のデータに変換しますがofetchでは自動で行ってくれます。
```
//$fetch関数
const data = await $fetch('/api/user', {
  method: 'POST',
  body: { name: 'John Doe' },
});

//fetch関数
const response = await fetch('/api/user', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ name: 'John Doe' }),
});
```
その他には存在しないURLにアクセスした場合にresponse.okがfalseの場合エラー処理を行ってくれたりinterceptorsやauto retryやbaseURLを設定することもできます。

クライアント側で行うフォームのPOSTリクエストなどでも$fetchを利用されます。

### useLazyFetch
useFetchとuseLazyFetchの違いを確認するためにuseFetchのコードをuseLazyFetchのコードに書き換えます。
```
<script setup>
const { data: posts, error } = await useLazyFetch(
  'https://jsonplaceholder.typicode.com/posts/'
);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <p v-if="error">{{ error }}</p>
    <ul>
      <li v-for="post in posts" :key="post.id">
        <NuxtLink :to="`/posts/${post.id}`">{{ post.title }}</NuxtLink>
      </li>
    </ul>
  </div>
</template>
```
最初に”/“にアクセスして上部のリンクから/posts に移動します。現在の設定では useFetch から useLazyFetch に変更しても違いはわかりません。

違うを明確にするため JSONPlaceHolder からのデータ取得の時間が遅延するようにブラウザのデベロッパーツールでネットワークのスピードを”Slow 3G”に変更します。

再度動作確認を行うと useFetch の場合は/posts にアクセスするとすぐに posts ページの画面が表示されるわけではなくしばらく時間が経過すると”Post 一覧”と取得したデータが同時に表示されます。useLazyFetch の場合は最初に”Post 一覧”が表示されてからその後取得したデータが表示されます。

ドキュメントを確認すると”By default, useFetch blocks navigation until its async handler is resolved.”と記載されており、useFetch が async handler が resolve されるまでナビゲーションをブロックしているためデータの取得が完了するまでページへの移動が行われません。useLazyFetch ではナビゲーションがブロックされないのでページに移動して”Post 一覧”がデータの取得が完了する前に表示されることになります。

useFetch のオプションの lazy を true に設定することで useLazyFetch と同じ動作になります。
```
const { data: posts, error } = await useFetch(
  'https://jsonplaceholder.typicode.com/posts/',
  {
    lazy: true,
  }
);
```
useLazyFetchから戻されるpendingを利用することでデータ取得中のみローディングを表示させることができます。
```
<script setup>
const {
  data: posts,
  error,
  pending,
} = await useLazyFetch('https://jsonplaceholder.typicode.com/posts/');
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <p v-if="error">{{ error }}</p>
    <p v-if="pending">Loading...</p>
    <ul>
      <li v-for="post in posts" :key="post.id">
        <NuxtLink :to="`/posts/${post.id}`">{{ post.title }}</NuxtLink>
      </li>
    </ul>
  </div>
</template>
```

### useLazyAsyncData
useLazyFetch関数はuseLazyAsyncData関数と$fetch関数を利用を便利にしたラッパーです。そのためuseLazyFetch関数と同じ処理をuseLazyAsyncData関数と$fetch関数で行うことができます。
```
<script setup>
const {
  data: posts,
  error,
  pending,
} = await useLazyAsyncData('posts', () =>
  $fetch('https://jsonplaceholder.typicode.com/posts/')
);
</script>
<template>
  <div>
    <h1>Posts一覧</h1>
    <p v-if="error">{{ error }}</p>
    <p v-if="pending">Loading...</p>
    <ul>
      <li v-for="post in posts" :key="post.id">
        <NuxtLink :to="`/posts/${post.id}`">{{ post.title }}</NuxtLink>
      </li>
    </ul>
  </div>
</template>
```

