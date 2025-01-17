# Nuxt3

## Nuxt3概要
Vue.jsをベースにしたフレームワーク
Vue.jsのUIだけでなくSSRを含むレンダリング機能、metaタグ、ルーティング、エラーハンドリングなどの機能を事前に組み込むことでアプリケーション開発を効率的に行う。
ルーティングであれば決められたディレクトリにファイルを作成するだけで自動でルーティングが設定される。
コンポーネントを利用する場合にimport文を使わなくても自動でimportされる
Typescriptもサポート

## Nuxt3プロジェクトの作成

### Node.jsのインストール
Node.js v18以上をインストール

### Nuxtプロジェクトを作成
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

### envファイルの作成
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

#### 本番環境・ステージング環境・開発環境で分けたい
nuxt3はdotenvが入っているためカスタムファイル（.env.local）が利用できます。各種envファイルを作成し、package.jsonへコマンドを登録しておきます。
```
// package.json

{
  ...
  "scripts": {
    ...
    "dev": "nuxt dev --dotenv .env.local",
     ...
    "generate": "nuxt generate --dotenv .env.local",
    "generate:staging": "nuxt generate --dotenv .env.staging",
    "generate:production": "nuxt generate --dotenv .env.production",
    ...
  },
}
```