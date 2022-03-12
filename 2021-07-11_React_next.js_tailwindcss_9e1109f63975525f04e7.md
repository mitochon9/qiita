<!--
title:   【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~サイト制作~ 
tags:    React,next.js,tailwindcss,初心者,就活
id:      9e1109f63975525f04e7
private: false
-->
# はじめに

前回の[【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~開発環境構築~](2021-07-06_React_next.js_tailwindcss_5bc6621a7c6160b92a7f.md)で開発を始められるところまで環境構築をしていきました。

- 本気でエンジニアを目指してスキルアップに励んでいる方
- fwywd さんの採用試験が気になっているけど、React？Next.js？ちょっと辞めておこうかな…

といった方がスムーズに技術を習得する助けとなり、共に切磋琢磨するべく引き続き、スキルアップできる採用試験、[【１次試験】fwywd 採用試験
](https://fwywd.com/news/recruitment-1st)の課題であるポートフォリオサイトを制作するために必要な知識についてお伝えできればと思います。

# 前回終了時点

前回、以下のような形で終わっていると思います。

```jsx
// index.jsx
const Home = () => {
  return (
    <div>
      <h1 className="text-2xl">こんにちは！！</h1>
      <p className="text-xs">お元気ですか？</p>
    </div>
  );
};
export default Home;
```

# インポートをルートディレクトリ基準にする設定

前回の環境構築では書いていませんでしたが、初期設定になります。

Next.js ではコンポーネントやモジュールなどをインポート（外部からデータを持ってくる）して使うことが多々あります。今回は相対パス（今いる場所が基準）でのインポートよりも、ルートディレクトリ（最上位のディレクトリ）を基準としたルートパスでインポートする方がわかりやすいためその設定をします。

参考：[絶対パス、相対パス、ルートパスの違いってなに？メリット・デメリットは？
](https://fastcoding.jp/blog/all/frontend/path/)

ルートディレクトリ（最上位のディレクトリ、package.json や tailwind.config.js のある場所）、に jsconfig.json を作成します。

## baseUrl の設定
"basrUrl": "." とすることでルートディレクトリ基準になります。

```json
// jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

## \_app.jsx の globals.css の import 部分を変更する

```jsx
//_app.jsx

/** 変更前
 import "../styles/globals.css";
 */

import "src/styles/globals.css";
```

恐らくここでエラーが出ると思うのでコマンドラインで`ctrl+c`を入力し監視を中止して yarn dev し直すときちんと表示されると思います。

これでコンポーネントやモジュールのインポートをする際はルートディレクトリを基準としたインポートができるようになりました。

参考：[【Next.js】特定のディレクトリを基準にし、絶対パスでモジュールをインポートする方法](https://fwywd.com/tech/next-base-url)

# ベースの CSS を当てる

Next.js ではいろいろな CSS の当て方がありますが今回のポートフォリオ制作では、

1. globals.css による全ページの`<body>`や`<a>`に適用するベースの CSS

1. `<h1>`などの要素に直接書く Tailwind CSS

1. どうしても Tailwind CSS で CSS を当てられない場合に CSS Modules

を使用します。

参考：[Built-In CSS Support
](https://nextjs.org/docs/basic-features/built-in-css-support)

まずベースの CSS を書きたいので`globals.css`に書きます。

```css
// globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* @layer base の中に書く */
@layer base {
  body {
    @apply text-text-black tracking-widest; /* 文字色と文字間隔 */
  }

  a {
    @apply hover:opacity-75 duration-300; /* aタグにカーソルを合わせたときの設定 */
  }
}
```

参考：[Adding Base Styles
](https://tailwindcss.com/docs/adding-base-styles)

globals.css は\_app.jsx で読み込まれている必要があります。

```jsx
// _app.jsx
import "src/styles/globals.css";
```

\_app.jsx は全ページで適用される部分を記述するファイルです。それにより全ページでのスタイルを適用させることができます。

例）

```jsx
// _app.jsx
import "src/styles/globals.css";

function MyApp({ Component, pageProps }) {
  return (
    <div>
      {/* 全ページに適用 */}
      <h1 className="bg-bg-green">ヘッダー</h1>

      {/*  pagesディレクトリ内各ページの内容が入る */}
      <Component {...pageProps} />

      {/* 全ページに適用 */}
      <h1 className="bg-bg-green">フッター</h1>
    </div>
  );
}
export default MyApp;
```
![_app.jsx-header-footer.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/6530d8ba-52be-5b5b-3180-9613f9a8008a.png)

ヘッダーやフッターなど各ページで共通の部分は\_app.jsx に記述することで読み込みも修正も少なく済むので便利です。
\_app.jsx の`<h1>`などの不要な内容は元に戻しておきます。

# ヘッダー部分のコーディング

index.jsx の return 以降にページで表示される部分を書きます。

```jsx
const Home = () => {
  return (
    <header>
      <a href="">
        <img src="/logo.png" alt="ロゴ" width="240" height="120" />
      </a>

      <ul>
        <li>
          <a href="#about">ABOUT</a>
        </li>
        <li>
          <a href="#skills">SKILLS</a>
        </li>
        <li>
          <a href="#values">VALUES</a>
        </li>
        <li>
          <a href="#future">FUTURE</a>
        </li>
      </ul>
    </header>
  );
};
export default Home;
```

ちゃんと表示されました。
![localhost_3000.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/933a5054-2082-ec9c-f844-ddb3c498ce91.png)


# `<Image>` で高パフォーマンスの画像表示

きちんとブラウザに表示されましたが Next.js をセットアップしたときに備わっている eslint に、`<img>`ではなく next/image の image を使ってくださいと怒られます。

> "message": "Do not use `<img>`. Use Image from 'next/image'

和訳：

> `<img>`は使用しないでください。next/image'から画像を使用してください。

Next.js では画像の最適化を自動的に行なってくれる`<Image>`を使用します。サイズの自動調整、対応ブラウザでの WebP 対応などパフォーマンスがかなり向上するので必ず使いましょう。

参考：[next/image
](https://nextjs.org/docs/api-reference/next/image)

## 実装方法

`<Image>`を利用するには next/image からインポートします。

```jsx
import Image from "next/image";

const Home = () => {
...
```

`img`を`Image`に変更します。

```jsx
<Image src="/logo.png" alt="ロゴ" width="240" height="120" />
```

これで無事に表示され eslint に怒られることもなくなりました。Next.js では public フォルダに入っている画像ファイルは`/`から書き始めてファイルを読み込みます。

## 【最新】Next.js11 での `<Image/>`追加機能

Next.js では 2021 年 6 月に行われたアップデートでローカルのファイル（public に入っているファイル）のみになりますが`<Image>`に便利な機能が追加されました。

1. 画像サイズの自動検出

   width と height の設定が不要になりました。

1. 画像が読み込めるまでのぼかし表示

   Next.js では画像は遅延読み込みされるようになっていますが、画像が表示されるまでの間ぼかして表示してくれる機能です。

参考：[Next.js 11
](https://nextjs.org/blog/next-11)

設定の仕方は簡単で、画像自体をインポートして呼び出すだけです。ぼかし機能を追加する場合は`placeholder="blur`を追加します。

```jsx
// 画像をインポートする
import logoImg from "public/logo.png";

/* 読み込む。JavaScriptのものを呼び出す際は{}で囲む */
/* placeholder="blur" でぼかし機能追加 */
<Image src={logoImg} alt="ロゴ" placeholder="blur" />;
```

これで無事に表示されています。
が、元画像が大きかったです。
![logo-big.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/6b449d9a-4d2a-9379-8941-ce8416b10f8b.png)


こんなときも width と height を設定すると Next.js が端末に合わせて画像サイズの調整をしてくれてパフォーマンス向上に役立ちます。

```jsx
// index.jsx
...
<Image src={logoImg} alt="ロゴ" width={240} height={120} placeholder="blur" />
...
```

# `<Link>` で高速なページ遷移を実装

Next.js では`<a>`でのページの遷移と別に `<Link>`でのページ遷移の機能があります。

- 遷移先ページの変更部分のみのレンダリング
- バックグラウンドでのリンク先ページのプリフェッチ（先回りして読み込み）

このような機能がありページ遷移がとてもスムーズになります。
内部リンクの場合はこの `<Link>`で実装するといいですね。
外部リンク（他サイト）の場合は`<a>`を使用してくださいとのことです。

> 外部 URL や /pages を使ったルートナビゲーションを必要としないリンクは、Link で処理する必要はありません; このような場合には、代わりにアンカータグを使用してください。

参考：[next/link
](https://nextjs.org/docs/api-reference/next/link)

## 実装方法

Image タグのときと同じように next/link から Link をインポートします。

```jsx
import Image from "next/image";
import Link from "next/link"; // 追加

const Home = () => {
...
```

`<a>`の外側（親要素）に`<Link>`を設置し`href`を`<a>`から`<Link>`の方に移動させます。

```jsx
    <header>
      {/* "/"はpages内のindex.jsxへの遷移になる */}
      <Link href="/">
        <a>
          <Image
            src={logoImg}
            alt="ロゴ"
            width={240}
            height={120}
            placeholder="blur"
          />
        </a>
      </Link>
      ...
```

これで完了です。

試しに`index.jsx`をコピーして pages ディレクトリ内に複製し、`Second.jsx`という名前に変えます。pages 内に作られたファイルは一つのページとなります。

```
components
└ pages
    ├  index.jsx
    └  Second.jsx
```

以下からアクセスできます。

[http://localhost:3000/Second](http://localhost:3000/Second)

実験的にトップページからロゴをクリックすると先ほど作成した`Second.jsx`に遷移するようにします。

```jsx
// index.jsx
      ...
      <Link href="/Second">
        <a>
          <Image
            src={logoImg}
            alt="ロゴ"
            width={240}
            height={120}
            placeholder="blur"
          />
        </a>
      </Link>
      ...
```

遷移したことがわかりやすいように`Second.jsx`の背景には色をつけてこちらのロゴをクリックするとトップページに戻るようにしておきます。

```jsx
// Second.jsx
const Second = () => {
  return (
    // 背景色追加
    <header className="bg-bg-green">
      <Link href="/">
        <a>
          <Image
            src={logoImg}
            alt="ロゴ"
            width={240}
            height={120}
            placeholder="blur"
          />
        </a>
      </Link>
      ...
      export default Second;
```

これでロゴをクリックすると各ページ間へ遷移するようになっています。パッと切り替わるのがわかると思います。
![link-tag.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/5db566ed-092a-5066-b558-63ce0d2e9478.gif)


ちなみに従来の`<a>`の場合は以下のような感じです。
![a-tag.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/870e55a6-23e8-f746-b7dd-8a1344211965.gif)


ページ間の移動は早い方がユーザー体験がいいのでぜひ活用しましょう。

index.jsx の不要な記述や作成した Second.jsx は使用しないので削除します。

```jsx
// index.jsx
import Image from "next/image";
import Link from "next/link";
import logoImg from "public/logo.png";

const Home = () => {
  return (
    <header>
      {/* "/"に戻す */}
      <Link href="/">
        <a>
          <Image
            src={logoImg}
            alt="ロゴ"
            width={240}
            height={120}
            placeholder="blur"
          />
        </a>
      </Link>
      <nav>
        <ul>
          <li>
            <a href="#about">ABOUT</a>
          </li>
          <li>
            <a href="#skills">SKILLS</a>
          </li>
          <li>
            <a href="#values">VALUES</a>
          </li>
          <li>
            <a href="#future">FUTURE</a>
          </li>
        </ul>
      </nav>
    </header>
  );
};
export default Home;
```

# Tailwind CSS で CSS を当てる

CSS で見た目を調整していきます。

1. container で画面幅のレスポンシブ対応
1. ロゴとナビを横並びにする
1. ナビのリストを横並びにして文字色変更
1. ナビのリスト間を調整

```jsx
import Image from "next/image";
import Link from "next/link";
import logoImg from "public/logo.png";

const Home = () => {
  return (
    // ① 画面幅のレスポンシブ対応
    <header className="container">
      {/* ② ロゴとナビを横並びにする */}
      <div className="flex justify-between items-center">
        <Link href="/">
          <a>
            <Image
              src={logoImg}
              alt="ロゴ"
              width={240}
              height={120}
              placeholder="blur"
            />
          </a>
        </Link>

        <nav>
          {/* ③ ナビのリストを横並びにして文字色変更 */}
          <ul className="flex text-text-green">
            {/* ④ ナビのリスト間を調整 */}
            <li className="mr-4">
              <a href="#about">ABOUT</a>
            </li>
            <li className="mr-4">
              <a href="#skills">SKILLS</a>
            </li>
            <li className="mr-4">
              <a href="#values">VALUES</a>
            </li>
            <li>
              <a href="#future">FUTURE</a>
            </li>
          </ul>
        </nav>
      </div>
    </header>
  );
};
export default Home;
```

これできちんと表示されています。
![css-1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/5e29d80b-16ea-6ea2-9a78-21a3586a1e09.png)



container の挙動については以下のようになります。

containerなし
![no-container.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/3164d4ca-f28f-634e-3146-255d041b9a76.gif)

containerあり
![container.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/bec48fbb-e2ac-02f3-70c5-961dba227f3e.gif)


# 繰り返し処理（map 関数）

Web サイトの制作では繰り返す部分が多々あります。そういった部分は map 関数を使うことで記述が少なくメンテナンス性を高くできます。

今回のヘッダーの場合だと`<li>`が繰り返しになっています。その部分を map 関数を使用して繰り返します。

## 変数に配列を格納

```jsx
// index.jsx
const ITEMS = [
  {
    link: "#about",
    title: "ABOUT",
  },
  {
    link: "#skills",
    title: "SKILLS",
  },
  {
    link: "#values",
    title: "VALUES",
  },
  {
    link: "#future",
    title: "FUTURE",
  },
];

const Home = () => {
...
```

## ITEMS の中身を map 関数で繰り返す

繰り返したい部分を map 関数で書きます。

```jsx
<ul className="flex text-text-green">
  {ITEMS.map((item) => {
    return (
      // 最初の要素にkeyの記述（一意の値、他とかぶらないもの）が必要
      <li key={item.title} className="mr-4 last:mr-0">
        {/* ITEMSのlinkとtitleを先頭から順番に繰り返す */}
        <a href={item.link}>{item.title}</a>
      </li>
    );
  })}
</ul>
```

これできちんと表示されました。
記述量も減り`<li>`全てにつけていた`mr-4`が一回の記述で済みました。変更があった場合にも 1 回の変更で済みます。もっと大きいコンポーネントの繰り返しだとさらに記述量を減らすことができます。

`<li>`についている`last:mr-0`は Tailwind CSS が用意している疑似要素を使用しています。Tailwind CSS では疑似要素もある程度使えるので便利です。

参考：[Configuring Variants
](https://tailwindcss.com/docs/configuring-variants#overview)

## 記述の簡素化

今回各セクションに移動するための`link`(#about など)と`<li>`で表示される ABOUT などが大文字か小文字かだけの違いなのでその部分も簡単にできます。

## 変数を簡易化する

```
const ITEMS = ["about", "skills", "values", "future"];
```

`{item.title}`や`{item.link}`から`{item}`に変更して読み込むことができます。

## テンプレートリテラル or 文字列結合を使う

変数を簡易化しましたが `<a href={item}` の方には`#`をつけないとセクションへの移動ができません。このまま map 関数 で`<a href={item}>`を繰り返すと、#about ではなく/about で別ページに遷移するようになってしまいます。

`#`をつけたいのですが、単純に`<a href="#{item}"`としてもうまくいきません。こういった場合は文字列の結合が必要になります。

参考：[テンプレートリテラルを使って文字列を表す
](https://www.javadrive.jp/javascript/string/index5.html)

また画面上に表示される文字を大文字に変えるために CSS で`uppercase`といったものを使用します。

参考：[Text Transform
](https://tailwindcss.com/docs/text-transform#uppercase)

```jsx
<ul className="flex text-text-green">
  {ITEMS.map((item) => {
    return (
      <li key={item} className="mr-4 last:mr-0">

        {/* テンプレートリテラルを使用する場合 */}
        <a href={`#${item}`} className="uppercase">

          {/* 文字列結合を使用する場合 */}
          {/* <a href={"#" + item} className="uppercase"> */}
          {item}
        </a>
      </li>
    );
  })}
</ul>
```

ちゃんとURLに#aboutや#skillsとついています。
![Template-literal.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/ae7b7e09-0a75-76a7-91a0-5bc25387b3d9.gif)

個人的な話にはなるのですが、JavaScript をしっかり学習しないまま React、Next.js の学習に入ったので、JavaScript で使用する構文の理解ができておらず現在進行形で学習中です。

少し荒削りな気もしますが、それはそれで必要に迫られて自分で調べてたどり着くのも学習方法の一つかと考えています。

# コンポーネント化する

ヘッダーはこれで完成したのでコンポーネント化して整理します。

- components フォルダに Header フォルダを作成

- Header フォルダに先ほど作った index.jsx を複製

```
// 今回はこちらのディレクトリ構造
components
└ Header
    └  index.jsx

or

components
└ Header.jsx
```

上記どちらでもいいのですが、今回は componens 内に Header フォルダを作成し、その中に index.jsx を作成します。

1. const Home を export const Header に変更する
1. 最下部の export default を削除

```jsx
// Header/index.jsx
import Image from "next/image";
import Link from "next/link";
import logoImg from "public/logo.png";

const ITEMS = ["about", "skills", "values", "future"];

// ① Home を Header に変更して export const Header にする
export const Header = () => {
  return (
    <header className="container">
      <div className="flex justify-between items-center">
        <Link href="/">
          <a>
            <Image
              src={logoImg}
              alt="ロゴ"
              width={240}
              height={120}
              placeholder="blur"
            />
          </a>
        </Link>

        <nav>
          <ul className="flex text-text-green">
            {ITEMS.map((item) => {
              return (
                <li key={item} className="mr-4 last:mr-0">
                  <a href={`#${item}`} className="uppercase">
                    {item}
                  </a>
                </li>
              );
            })}
          </ul>
        </nav>
      </div>
    </header>
  );
};
// ② 最下部の export default を削除
```

pages/index.jsx で Header 部分の記述を削除して Header コンポーネントを読み込む

```jsx
// pages/index.jsx

// Headerをインポート
import { Header } from "../components/Header";

const Home = () => {
  return (
    {/* Headerを読み込む */}
    <Header />
  );
};
export default Home;
```

これでコンポーネント化完了です。
ちゃんと表示されていれば OK です。

コンポーネント化に関しては、

- コーディング → コンポーネント化 → 必要なところでインポート
- コンポーネント化 → コーディング → 必要なところでインポート

のどちらでもいいと思います。私は今のところ先にコンポーネントを作ってからコーディングしています。

ここまででこれ以降もある程度作り進めていけると思います。

# 背景画像設定

今回のポートフォリオ制作では ABOUT セクション以降背景画像の設定があり、Tailwind CSS を使用した背景画像の設定と CSS Modules を使用した背景画像の設定をそれぞれ紹介します。

## Tailwind CSS での背景画像設定の仕方

① tailwind.config.js で背景画像の設定を theme > extend に追加します。

```jsx
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      backgroundImage: (theme) => ({
        "bg-about-pc": "url('public/bg-about.png')",
      }),
    },
  },
};
```

② 背景画像読み込み

bg-[tailwind.config.js でつけた名前]（今回は bg-about-pc）で読み込みます。

```jsx
// About/index.jsx
export const About = () => {
  return (
    <section id="about" className="bg-bg-about-pc bg-no-repeat bg-cover py-20">
...
```

## CSS Modules での背景画像設定の仕方

components/About フォルダ内に CSS ファイルを作成します。

```
components
└ About
    ├  index.jsx
    └  About.module.css
```

About/index.jsx で CSS ファイルをインポートして CSS を当てたい要素にクラス名をつけます。

```jsx
// About/index.jsx

// CSSファイルのインポート classesやstylesといった名前がよくつけられる
import classes from "src/components/About/About.module.css";

    ...
    <section id="about" className={`${classes.bg} py-20`}>

    // 今回は余白はclasses.bgと別にTailwind CSSでつけています
    // CSS Modulesのみで使用する場合
    // <section id="about" className={classes.bg}>
    ...

```

About/About.module.css で CSS を当てます。

```jsx
// About/About.module.css
.bg {
  background-image: url(public/bg-about.png);
  background-repeat: no-repeat;
  background-size: cover;
}
```

CSS ファイル内でも Tailwind CSS が使えます。

```jsx
// About/About.module.css
.bg {
  background-image: url(public/bg-about.png);
  @apply bg-no-repeat bg-cover;
}
```

# セクションのタイトル部分を props で再利用する

今回のポートフォリオ制作ではセクションごとのタイトルのデザインが一緒になっていて、テキストをその場所ごとに可変にしてコンポーネント化できると便利です。
そういった場合は props を使用すると便利です。
![props-description.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/398d069b-6cba-5503-89e6-d8c2e7b8158c.jpeg)


参考：[コンポーネントと props
](https://ja.reactjs.org/docs/components-and-props.html)

① components 内に SectionTitle フォルダを作りその中に index.jsx を作成

```
components
├ Header
│  └ index.jsx
├ Hero
│  └ index.jsx
├ About
│  └ index.jsx
└  SectionTitle
     └ index.jsx
```

② セクションのタイトル部分を書く

```jsx
// SectionTitle/index.jsx
export const SectionTitle = () => {
  return (
    <div className="flex items-baseline justify-center">
      <h1 className="text-3xl font-bold">私について</h1>
      <span className="ml-4 text-xl text-text-green">ABOUT</span>
    </div>
  );
};
```

③ `SectionTitle/index.jsx`で props を受け取るようにする

```jsx
// SectionTitle/index.jsx
export const SectionTitle = (props) => {
  return (
    <div className="flex items-baseline justify-center">
      <h1 className="text-3xl font-bold">{props.title}</h1>
      <span className="ml-4 text-xl text-text-green">{props.lead}</span>
    </div>
  );
};
```

④ 表示したい場所で SectionTitle を読み込んで props に渡したい部分を書く

```jsx
// About/index.jsx
import { SectionTitle } from "src/components/SectionTitle";
...
export const About = () => {
  return (
...
<SectionTitle title="私について" lead="ABOUT"/>
...
```

これできちんと表示されます。後のスキルセクションや価値観セクションでも SectionTitle を読み込んで props として値を渡すと同じデザインで文字を変えることができます。

さらに後のセクションを見てみると、スキルセクションと 3 年後にやりたいことセクションではタイトルが左寄せになっているので className の部分も props で渡すようにします。

![props-center-left.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/28e54eff-aff2-ae49-6bc4-0ddc5d2856df.jpeg)

```jsx
// SectionTitle.jsx
    <div className={`flex items-baseline justify-${props.position}`}>

// About/index.jsx
    {/* 左寄せにしたい場合はposition="start"にする */}
    <SectionTitle title="私について" lead="ABOUT" position="center" />
```

きちんと表示されていれば OK です。


# 変数内での改行

ABOUT セクションなどでテキスト部分も含めて map 関数で繰り返したいこともありますが、変数の中では`<br />`を入れることはできません。map 関数を使いたいけど改行ができない…と私はこの部分で躓きました。

こういった場合はテンプレートリテラル内で改行し、改行したい要素の className や globals.css で`<p>`に対して`whitespace-pre-wrap`を書くと改行ができるようにます。

```jsx
// About/index.jsx
const ITEMS =[{
  ...
  text:"テキスト<br />テキスト" // NG
  text:`テキスト
テキスト` // OK
}]
...
<p className="text-left whitespace-pre-wrap">{item.text}</p>
```

# ライブラリを追加してスムーズなスクロールを実装

現在は`<a href="#about">`というように書いて各セクションへの移動をできるようにしていますが、ヘッダーのリンクをクリックするとパッと移動するようになっています。
![scroll.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/e381b559-73d9-8923-22fa-d456e5a27e66.gif)


※ 各セクションに id をつけるのを忘れないようにしましょう。

```jsx
<section id="about">...</section>
```

この部分をライブラリを追加して簡単にスムーズにスクロールする機能を実装します。下記記事を参考にして react-scroll を導入しました。

参考：[react-scroll を導入し超簡単に画面内のスクロールを実装しよう](https://fwywd.com/tech/install-react-scroll)

npm もしくは yarn で react-scroll をインストールします。

```
npm install --save-dev react-scroll

# or

yarn add react-scroll --dev
```

`Header/index.jsx`で react-scroll をインポートして`<a></a>`を`<Scroll></Scroll>`に変更します。

```jsx
// Header/index.jsx

// react-scrollをインポート
import { Link as Scroll } from "react-scroll";


// 変更前
<a href={`#${item}`} className="uppercase">{item}</a>

// 変更後
<Scroll to={item} className="uppercase">
  {item}
</Scroll>;
```

これで問題なく移動できています。あとはスムーズに移動させる設定とスクロールスピードの設定をします。

```jsx
/** smooth={true}でスムーズにスクロールさせる
 * duration={...}でスクロールスピードの設定
 */
<Scroll to={item} smooth={true} duration={600} className="uppercase">
  {item}
</Scroll>
```

これでスムーズなスクロールが実装できました。
![smooth-scroll.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/e01cc56c-5a67-776b-4951-4e01b5c1269d.gif)


以上で今回のポートフォリオ制作は完成させられることができるはずです。
まだまだ React、Next.js の機能はありますが一旦これぐらいで WEB サイトは作ることができるのではないかと思います。私もさらに学習してどんどん高度なものを作れるようにしていきます。

次回は実際にサイトを公開するにあたって meta タグの設定やドメインの取得、Next.js で作成したプロジェクトを簡単にデプロイ（配備する、使える状態にする意味だそうです）できる Vercel といったサービスの使い方を書きます。

わかりにくい部分もあったかと思いますが最後まで読んでいただきありがとうございました。

この記事が参考になったと思ったらぜひ LGTM を押していただけると嬉しいです！

# 続きはこちら
[【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~meta設定から公開まで~](2021-07-19_React_next.js_tailwindcss_aa318d47a7837c2508e3.md)