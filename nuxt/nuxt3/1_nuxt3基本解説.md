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

## app.vue以外のファイル
app.vue ファイル以外に nuxt.config.ts, tsconfig.json, README.md, package.jsonファイルなどがあります。

nuxt.config.tsはNuxtに関する設定を行うことができるファイルで本文書でもnuxt.config.tsファイルを利用して設定を行います。拡張子が ts となっている通りデフォルトから TypeScript に対応しています。

tsconfig.json は TypeScript の一般的な設定ファイルです。README.mdはインストールコマンドなどの説明が記述されています。

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

## ComponentsのLazy Loading(遅延読み込み)
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
画像ファイルを assets ディレクトリに保存します。ここでは icon.png ファイルを assets フォルダの直下に」保存しています。Nuxtに関する画像は(https://nuxt.com/design-kit)からダウンロードすることができます。

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

## ルーティングの設定
pagesディレクトリのindex.vue, about.vueファイルを作成して自動でルーティングが設定されることを確認していますが階層ページなどルーティングの設定についてさらに詳しく確認しておきます。

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