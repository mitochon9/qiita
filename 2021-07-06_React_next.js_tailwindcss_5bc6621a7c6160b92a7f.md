<!--
title:   【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~開発環境構築~ 
tags:    React,next.js,tailwindcss,初心者,就活
id:      5bc6621a7c6160b92a7f
private: false
-->
# はじめに
普通、試験ってスキルアップしてから受けるものですよね？
進めていくとスキルアップできる試験が存在するのを知っていますか？

そんな型破りな試験が存在するのです。

[【無料公開】fwywd 採用試験の全体像
](https://fwywd.com/news/recruitment)

こちらの試験を公開されている[fwywd](https://fwywd.com/)さん、AI領域に特化した教育事業を展開している[株式会社キカガク](https://www.kikagaku.co.jp/)さんの新規事業として立ち上がり、理念やビジョンなどがとても共感できて、素晴らしいと感じたのでチャレンジさせてもらいました。

無事に採用一次試験前半の合格をいただいたので、1 回目の制作での修正点を復習しながら、

- 本気でエンジニアを目指してスキルアップに励んでいる方
- fwywd さんの採用試験が気になっているけど、React？Next.js？ちょっと辞めておこうかな…

といった方がスムーズに技術を習得する助けとなり、共に切磋琢磨できればと思います。

今回は開発環境構築編ということで初学者にとっては躓きやすい React, Next.js での開発を始めるまでを解説していきます。

# 今回の制作物

![01_introduction_pc.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/252e2551-238e-f9dc-e9a1-7f00a5834cad.jpeg)


こちらのサイトを JavaScript の UI パーツを構築するためのライブラリである React、React のフレームワークである Next.js を使用します。（簡単に言うと開発の効率を良くしてメンテナンス性も高める手助けをしますよ！といったものです）

また CSS の実装は CSS のフレームワークである Tailwind CSS を使用して制作します。

テクノロジーの進化はすごいもので、開発効率の向上はもちろん、ファイルや画像の圧縮などのパフォーマンス面も向上してくれます。

今回制作したポートフォリオサイトも[PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=ja)で 100 点が出ました。（HTML・CSS で Web サイトを作っていた時は自分なりにあれこれ頑張っても70点とかで大変でした…）
※100点出るまでやったと書いていますが出た点数は99点でした。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">fwywdさんのポートフォリオ完成しました！最終チェックと自分自身を見つめ直すことが一番時間がかかったかも知れない…<br>PageSpeed Insightsもモバイルで100点出ました！（出るまでやったとは到底言えない…）<br>ちょくちょく詰まりながらも成長を感じられる要素がたくさんあって本当に楽しかったです✨ <a href="https://t.co/z7BLNMvgNY">pic.twitter.com/z7BLNMvgNY</a></p>&mdash; たかはし (@mitochon_9) <a href="https://twitter.com/mitochon_9/status/1409485795367346178?ref_src=twsrc%5Etfw">June 28, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
# 大まかな制作の流れ

1. 開発環境構築
1. コーディング
1. meta タグの設定
1. ドメイン取得
1. Vercel でデプロイ

このような流れで制作していきます。
その中で私が詰まった部分、理解に苦労した部分をできるだけわかりやすく伝えられたらと思います。

# 1. 開発環境構築

個人的にはこの開発環境構築が最初のハードルの高い部分でした。

今回の開発で使用する React や Next.js、TailwindCSS を導入するためにはコマンドライン（黒いプログラミングっぽい画面、Mac ならターミナル、Windows ならコマンドプロンプト）の操作が必須になります。

今まで触れたことがない上に危険そうな香りのするコマンドライン操作に慣れることが大変でした。
しかしコマンドラインの操作をするようになってからできることの幅が広がり開発効率が圧倒的に良くなりました。

コマンドラインについてはとりあえず触れてみたほうが身近に感じると思います。
こちらのサイトが実践形式でとてもわかりやすかったのでぜひ試してみてください。
参考：[コマンドラインを使ってみよう](https://tutorial.djangogirls.org/ja/intro_to_command_line/)

なんとなくコマンドライン操作に慣れたところで次は Next.js を導入するための npm や yarn といったパッケージ管理システムといったものを使用します。
参考：[npm とは　 yarn とは
](https://qiita.com/Hai-dozo/items/90b852ac29b79a7ea02b)

npm や yarn のインストールは下記を参考にしました。
参考：[Mac nodebrew のインストール方法](https://awesome-linus.com/2019/08/17/mac-nodejs-nodebrew-install/)
参考：[npm と npx。なにが違う？](https://qiita.com/sivertigo/items/622550c5d8ec991e59a6)
参考：[Mac で yarn をインストールする方法](https://awesome-linus.com/2019/04/11/mac-yarn-install/)

私は yarn を使用しています。yarn をインストールするために npm が必要になるので npm→yarn と順番にインストールしていきました。

npm,yarn がインストールできたら実際に React,Next.js,Tailwind CSS をインストールしていきます。

# React+Next.js で便利になること

## コンポーネントでのパーツの分割
Web サイトにはヘッダーやフッターなど、各ページで構成が変わらない部分がありますが、このような部分をコンポーネント化することにより、1 回の修正で全ページの修正が完了します。

![react-component-img.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/a19b483e-0519-3344-f189-fa5e12d73860.png)


## コンポーネント化による命名コストの削減
コンポーネント化することによりそれぞれのコンポーネント単位でクラス名をつけることができるため、複雑な命名をすることがしなくてもよくなり、クラス名の命名コストを大幅に削減できるようになりました。

私はこれを初めて知った時にクラス名をつけるのに悩まなくていいこんな素晴らしい世界があったのか…！と驚愕しました。

私のただの力不足なのですが、過去の自分の制作物を見ると campaign-price-container-join-box\_\_description--emphasis という命名もあって、クラス名をつけるのに迷走していた過去もありました…。

```jsx
// Header.jsx
import classes "styles/Header.module.css"
<h1 className={classes.title}>ヘッダーのタイトル</h1>
// ブラウザ表示時にはHeader_title__〇〇といったクラス名に変換されてクラス名が被ることを防いでくれる
```

```jsx
//Main.jsx
import classes "styles/Main.module.css"
<h1 className={classes.title}>メインのタイトル</h1>
// ブラウザ表示時にはMain_title__〇〇といったクラス名に変換
```

```jsx
// Footer.jsx
import classes "styles/Footer.module.css"
<h1 className={classes.title}>フッターのタイトル</h1>
// ブラウザ表示時にはFooter_title__〇〇といったクラス名に変換
```

```css
/* Header.module.css */
.title {
  font-size: 32px;
}
```

```css
/* Main.module.css */
.title {
  font-size: 20px;
}
```

```css
/* Footer.module.css */
.title {
  font-size: 16px;
}
```

## 画像の自動最適化
ブラウザに合わせて画像を WebP などの最新の形式でサイズの変更、最適化が行われます。過去に私もパフォーマンス向上のために jpg や png、WebP と画像をそれぞれ準備して出し分けるというようにしていましたがその工程が必要なくなりました。

参考：[Image Component and Image Optimization
](https://nextjs.org/docs/basic-features/image-optimization)

## スピーディーなページ移動
aタグでの別ページへの移動は移動先のページを全て読み込むためどうしても時間がかかってしまうことがありましたが、Next.jsの`<Link>`タグを使うことにより、移動先のページの変更部分のみレンダリングされることやバックグラウンドで先に移動先のページを読み込んでくれることによりスピーディーなページ移動が可能になっています。
今回のポートフォリオ制作ではページは一枚だけですが、ヘッダーナビでこの`<Link>`タグを使用します。

aタグでのページ遷移
![a-tag.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/58a65ab2-c932-e846-bf75-1127f6f5a577.gif)


Linkタグでのページ遷移
![link-tag.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/fa5171e5-b24b-232d-81c6-74368b534817.gif)



参考：[next/link
](https://nextjs.org/docs/api-reference/next/link)

## 開発時の高速リフレッシュ
Web サイトを作っている時はエディターで保存 → ブラウザで更新というように確認しながら作業しますが、Next.js ではエディターで保存すると即座に変更点が反映されるため単純に開発スピードが向上します。ほとんどの編集が１秒以内に表示されると書いている通りです。

![ezgif.com-gif-maker.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/9ac7d107-659a-0dc1-f47a-6ed44dd0d6aa.gif)



参考：[Fast Refresh
](https://nextjs.org/docs/basic-features/fast-refresh)

***
その他にも機能やメリットはたくさんあるのですが今回必要になるのはこのあたりかと思います。開発効率が段違いによくなるものばかりで、私が React、Next.js を知った時は公式ドキュメントの機能紹介をワクワクしながら読んでいました。

# React,Next.js のセットアップ

Next.js をセットアップすれば React も一緒にインストールされるので Next.js の公式ドキュメントでセットアップコマンドを確認して実行します。

React,Next.js での制作を始めてから公式ドキュメントといわれるものを見ることが増えました。ほとんど英語ですが今は翻訳技術も向上しているので意外と読み進めていけます。最新の情報はやはり公式ドキュメントにあることが多いのでなるべくチェックするようにしています。

参考：[Create Next App](https://nextjs.org/docs/api-reference/create-next-app)

npm,yarnでインストールします。

```
npx create-next-app
# or
yarn create next-app
```

この後プロジェクト名を聞かれるので入力して進めると Next.js がインストールされます。
今回は fwywd さんのポートフォリオ制作なので`fwywd-portfolio`としました。

次に cd(change directory の略)で先ほど create next-app で作成したディレクトリに移動します。

```
cd fwywd-portfolio
```

ちなみにフォルダとディレクトリとはどのように違うのかと思っていたのですが、基本的には同じ意味でとりあえず GUI(Graphical User Interface)ではフォルダ、CLI(Command Line Interface)ではディレクトリと認識しておいて良さそうです。

初学者にとっては初めて知る言葉がたくさんあるので、逐一調べながら一つ一つ覚えています。

参考：[「ディレクトリ」と「フォルダ」の違いと共通点](https://uxmilk.jp/27546)

参考：[CLI とは？GUI との違いは？](https://academy.gmocloud.com/keywords/20170301/3997)

**※重要※**
**Next.js などのパッケージをインストールする際はオンラインストレージには絶対に保存しない！**

回避策があったり、IT 業界では当たり前のことだったりするのかも知れませんが、過去に私は何でもクラウドに入れておけばいいやと思って安易に OneDrive や iCloud などのオンラインストレージにインストールしたのですが、Next.js などのパッケージには何千、何万という数のファイルがインストールされます。そうなると同期が追いつかなくなってクラウドの使用に支障が出るので、ローカルのフォルダにインストールするようにしましょう。

# ディレクトリの整理

エディターで先ほど作成した`fwywd-portfolio`を開き、ディレクトリ構造の整理をしておきます。私は大体下記のようにしています。このあたりは好みやプロジェクトにより変わることもあると思いますが、参考までに。

![Cursor_と_fwywd-qiita.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/b463376a-8cc8-71bc-1564-776b08c9d414.png)

- src フォルダに pages と styles を移動

- src フォルダに components フォルダを追加

- 拡張子を js から jsx に変える

- Home.module.css の削除

- public に素材画像を追加する

ここでの注意点としては、

- public,pages の名前を変えてはいけない

  Next.js が決めているディレクトリの名前なので変えるとエラーが出ます。

- 画像は基本的に public に入れる

  Next.js で画像を自動的に最適化してくれる機能を使用するために public に入れます。

# Tailwind CSS

Tailwind CSS とは CSS を CSS ファイルに書くのではなく HTML のタグに直接書いて CSS を当てる CSS フレームワークです。

- クラス名を考えなくていい

- CSS のファイルサイズ削減

- コーディング時のファイル間の移動が少なくて効率的

などメリットがたくさんあります。先ほどReactではクラス名の命名コストが削減できると書きましたがTailwind CSSを使えばそもそもクラス名をつけることがほとんどなくなります。

参考：[Tailwind CSS のメリットとデメリット](https://zenn.dev/yohei_watanabe/articles/def8bbd6415d90)

```css
/* 従来のCSS */
.title {
  background-color: #fff;
  font-size: 18px;
}
```

```html
// Tailwind CSSの場合
<h1 className="bg-white text-lg">タイトル</h1>
```

## Tailwind CSS のインストールと構成ファイル追加

npm または yarn を使用して Tailwind CSS をインストールします。

参考：[Install Tailwind CSS with Next.js](https://tailwindcss.com/docs/guides/nextjs)

ドキュメントには yarn のコマンドは書いていませんが、yarn で Tailwind CSS をインストールするには下記のコマンドでいけました。

```
// Tailwind CSSのインストール
yarn add tailwindcss@latest postcss@latest autoprefixer@latest --dev

// tailwind.config.jsやpostcss.config.jsの構成ファイル追加
yarn tailwindcss init -p
```

**※ dependencies と devDependencies について**

私が開発した時は`dependencies`と`devDependencies`の違いを理解していなかったので、全て`dependencies`でインストールしていました。

`dependencies`：本番環境で必要

`devDependencies`：開発環境でのみ必要

Tailwind CSS は開発環境で必要なパッケージなので`--dev`とつけて`devDependencies`にインストールします。

![Cursor.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/1b374422-25aa-f2ee-13de-277c6ea49865.png)

## global.css を Tailwind CSS 用の設定に置き換える

Tailwind CSS でカスタム CSS（w-10 や text-center などではなく従来のように CSS ファイルに書く方法）も組み合わせたい場合は globals.css で以下のように書きます。

```css
/* ./styles/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Tailwind CSS では上記の`@tailwind base`を記述することによりスタイル全般がリセットされるので、reset.css などを別に当てる必要がありません。

参考：[Preflight](https://tailwindcss.com/docs/preflight)

## JIT モードを適用

Tailwind CSS に JIT モードというものがあり、これを適用することでビルド時間の短縮など、パフォーマンスの向上ができます。
また、柔軟な CSS の書き方ができるようにもなるのでぜひ適用しましょう。

```html
// 1remが初期値の16pxの場合
<div className="w-10"></div> // width:2.5rem(40px)

<div className="w-11"></div> // width:2.75rem(44px)

// JITモードの場合、TailwindCSSにデフォルトでない数値も設定できる
<div className="w-[42px]"></div> // width:42px
```

jit の設定と CSS を軽量化するための purge の設定を tailwind.config.js に書きます。

また、purge の設定を間違えると CSS が当たらないので注意しましょう。

```js
// tailwind.config.js
module.exports = {
  mode: "jit",
  purge: [
    "./public/**/*.html",
    "./src/**/*.{js,jsx,ts,tsx}", // srcディレクトリにあるjs,jsx,ts,tsxを監視して軽量化してくれる
  ],
  ...
};
```

参考：[Just-in-Time Mode](https://tailwindcss.com/docs/just-in-time-mode)

ちなみになぜか私は JIT モードを適用するとどうやっても CSS が当たらない不具合に遭遇し、最終的に Mac を初期化することとなりました…。

## カラーコードの設定追加

今回はカラーコードが指定されているのでその設定を`tailwind.config.js`に追加します。

```js
const colors = require("tailwindcss/colors");

module.exports = {
  ...
  theme: {
    colors: {
      transparent: "transparent",
      current: "currentColor",
      black: colors.black,
      white: colors.white,
      "text-black": "#243c5a",
      "text-green": "#008c8d",
      "bg-green": "#6bc2c3",
      "bg-black": "#262c3a",
      "progress-pale": "#c5eaea",
      "progress-deep": "#2bb9ba",
      "border-green": "#c5eaea",
    },
  ...
```

カラーコード：[【１次試験】fwywd 採用試験
](https://fwywd.com/news/recruitment-1st)

参考：[Customizing Colors
](https://tailwindcss.com/docs/customizing-colors)

module.exports > theme > colors に追加すると初期設定されているカラーコードも置き換わります。

module.exports > theme > extend > colors に追加すると初期設定されているカラーコードを残したまま追加されます。

## container 設定の追加

container Tailwind CSSのクラスのひとつでブレークポイントごとに max-width を設定してくれ、簡単に画面の幅のレスポンシブ対応ができます。

```jsx
// index.jsx
<div class="container">...</div>
```

containerなし
![no-container.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/d2ac3494-6936-3ea9-ad14-a164d64d9784.gif)

containerあり
![container.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/d2e4b971-7021-1b6d-6418-112ce570accd.gif)



 初期設定ではcontainerを使用したときには左揃えになっているためデフォルトで中央揃えになるように設定します。

```js
// tailwind.config.js
module.exports = {
  theme: {
    container: {
      center: true,
    },
  },
};
```

参考：[Container
](https://tailwindcss.com/docs/container)

これで初期設定は完了です。

# 動かしてみる

Next.jsではpagesに入っているjs,jsx,ts,tsxファイルが実際にアクセスするページになります。
今回ではpages内のindex.jsxがトップページとなります。

index.jsx の内容を下記の内容に変えてローカルサーバーを立ち上げます。

```jsx
// index.jsx
const Home = () => {
  return (
// returnの中が実際に表示される部分になり、複数行になる場合は一つのタグで囲まれている必要がある。
    <div>
      <h1 className="text-2xl">
        こんにちは！！
      </h1>
      <p className="text-xs">お元気ですか？</p>
    </div>
  );
};
export default Home;
```

npm,yarnでローカルサーバーを立ち上げます。

```
npm run dev
# or
yarn dev
```

ブラウザで[http://localhost:3000](http://localhost:3000)にアクセスすると以下のように表示されます。
ちなみにローカルサーバー監視の終了はctrl+cでできます。

![localhost_3000.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/56e33efd-ef59-ab27-9ee1-d1c870efccfd.png)

試しに`<h1>`の`text-2xl`を`text-9xl`などに変更したり、テキストを変更してみたりして保存してください。先程ブラウザで開いた画面が即座に更新されます。

![Environment.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/9fbe024a-7d0a-faa5-777a-e0da51c83eba.gif)


ぜひTailwind CSSの公式ドキュメントを見ながらいろいろCSSを当ててみてください。

参考：[Utility-First
](https://tailwindcss.com/docs/utility-first)

あとVSCodeでTailwind CSSを書く時は必ず補完機能を追加してくれる拡張機能を入れておきましょう。
これがあるかないかで開発の効率がかなり変わってきます。

参考：[Editor Support
](https://tailwindcss.com/docs/editor-support)

次回は実際に制作を始めるにあたって私がコーディングで苦労した部分などの解説ができればと思います。
できるだけわかりやすく書けるように文章力も向上させていきますのでよろしくお願いします。

最後まで読んでいただきありがとうございました。

この記事が参考になったと思ったらぜひLGTMを押していただけると嬉しいです！

# 続きはこちら
[【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~サイト制作~ (React,Next.js,TailwindCSS)](2021-07-11_React_next.js_tailwindcss_9e1109f63975525f04e7.md)