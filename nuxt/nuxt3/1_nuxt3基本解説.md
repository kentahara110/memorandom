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

app.vueファイルはNuxt 3のメインコンポーネントです。複数のページを作成する場合は pages ディレクトリを利用しますが pages ディレクトリはオプションなので必ず利用する必要はありません。ランディングページのみ、ルーティングが必要ないアプリケーションの場合は、app.vue ファイルを利用するだけでアプリケーションを構築することができます。app.vue ファイルのみ利用した場合にはルーティングの vue-router をバンドルする必要がないためビルド後のバンドルファイルのサイズを減らすことができます。バンドルサイズが小さくなることでクライアントが受け取るデータ量も減るためページの表示速度の高速化につながります。

## app.vue以外のファイル
app.vue ファイル以外に nuxt.config.ts, tsconfig.json, README.md, package.jsonファイルなどがあります。nuxt.config.tsはNuxtに関する設定を行うことができるファイルで本文書でもnuxt.config.tsファイルを利用して設定を行います。拡張子が ts となっている通りデフォルトから TypeScript に対応しています。tsconfig.json は TypeScript の一般的な設定ファイルです。README.mdはインストールコマンドなどの説明が記述されています。

## ディレクトリ構成
Nuxt 3ではプロジェクト作成時に node_modules, public, server 以外のディレクトリは存在しませんがそれぞれ異なる役割を持っているディレクトリを追加していくことでアプリケーションを構築していきます。ディレクトリ名はNuxt 3の中で事前に決められておりコンポーネントを利用する場合は componentsディレクトリを作成します。componentsディレクトリの下にコンポーネントファイルを保存することで Auto Imports 機能により import 文を利用しなくても自動でコンポーネントの import が行われます。そのためディレクトリ名や作成したディレクトリがどのような役割を持っているのかをディレクトリ毎に理解しておく必要があります。

## pagesディレクトリ
app.vue ファイルだけでアプリケーションを構築することができるということは説明しましたがNuxtを利用する開発者は通常複数ページで構成されたアプリケーションを構築します。ページを追加したい場合にはpagesディレクトリを利用します。Nuxtではファイルベースルーティングを採用しているので pages ディレクトリの中にファイルを作成するだけで自動でルーティングが設定されます。

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

## NuxtLayoutコンポーネント
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

## 動的にLayoutの変更
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

## 名前付きslotの設定
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