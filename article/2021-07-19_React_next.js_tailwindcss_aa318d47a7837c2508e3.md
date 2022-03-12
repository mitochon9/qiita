<!--
title:   【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~meta設定から公開まで~
tags:    React,next.js,tailwindcss,初心者,就活
id:      aa318d47a7837c2508e3
private: false
-->
# はじめに
前々回：[【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~開発環境構築~](2021-07-06_React_next.js_tailwindcss_5bc6621a7c6160b92a7f.md)

前回：[【スキルアップできる採用試験】fwywd（フュード）採用一次試験を迷わずに進めるために必要な知識 ~サイト制作~ (React,Next.js,TailwindCSS)](2021-07-11_React_next.js_tailwindcss_9e1109f63975525f04e7.md)

前回、[【１次試験】fwywd 採用試験](https://fwywd.com/news/recruitment-1st)の課題であるポートフォリオサイトが完成できる部分までを記事にしました。
今回はそのポートフォリオサイトのmeta設定とVercelで公開するところまで進めます。


# meta設定
今回のポートフォリオサイト制作は1ページなのでtitleやdescriptionなどはindex.jsxや_app.jsxに直接書いていってもいいのですが、今後ページが増えていくと想定してmeta設定をコンポーネント化して管理し、ページによって内容を出し分けられるようにしたいと思います。

## コンポーネント、PageHeadを作成
`
components
├ Header
│  └ index.jsx
...
└ PageHead
     └ index.jsx
`
## Headタグの中にmeta設定を書く
`jsx
// components/PageHead/index.jsx

import Head from "next/head";

export const PageHead = () => {
  return (
    <Head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# article: http://ogp.me/ns/article#">
      <meta name="viewport" content="width=device-width,initial-scale=1.0" />
      <title>HOME | 私のポートフォリオサイト</title>
      <meta
        name="description"
        content="IT業界に転職するためプログラミングを全力で楽しく学習しています。"
      />
      <meta property="og:title" content="HOME | 私のポートフォリオサイト" />
      <meta
        property="og:description"
        content="IT業界に転職するためプログラミングを全力で楽しく学習しています。"
      />
      <meta property="og:type" content="website" />
      <meta property="og:url" content="https://example.com" />
      <meta property="og:image" content="https://example.com/ogp.png" />
      <meta property="og:site_name" content="私のポートフォリオサイト" />

      {/* Facebook */}
      <meta property="fb:app_id" content="Facebookのapp_idを入力" />

      {/* Twitter */}
      <meta name="twitter:card" content="summary" />
    </Head>
  );
};

```
ちなみにシェアされた際の画像表示はog:imageで設定しますが、絶対パス（`https://example.com/ogp.png`など）でないと読み込めないので注意が必要です。

## ページごとにmeta情報を出し分ける設定
```jsx
// components/PageHead/index.jsx

...
export const PageHead = ({
  title = "HOME",
  description = "IT業界に転職するためプログラミングを全力で楽しく学習しています。",
}) => {
  return (
...
<title>{title} | 私のポートフォリオサイト</title>
<meta name="description" content={description} />
...
```

ページごとに出し分ける時は`<PageHead>`を呼び出し渡してあげればOKです。

```jsx
// pages/index.jsx

<PageHead title="テスト！" />
```

![スクリーンショット 2021-07-19 14.53.42.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/7aa159bf-78da-cfae-3ff3-56acfb43d3ec.png)

## 環境変数を.envに書く
環境変数とはなんぞや？と思い調べてみました。

>環境変数とは、OSが設定値などを永続的に保存し、利用者や実行されるプログラムから設定・参照できるようにしたもの。プログラムの実行時などに必要となる、利用者やコンピュータごとに内容が異なる設定値などを記録するために用いられる。
[環境変数](https://e-words.jp/w/%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0.html#:~:text=%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%81%A8%E3%81%AF%E3%80%81OS,%E3%81%99%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AB%E7%94%A8%E3%81%84%E3%82%89%E3%82%8C%E3%82%8B%E3%80%82)

とのことです。とりあえず今回はサイト名やURLを`.env`といったファイルに書いて取り出すようにします。
.envに関しては下記を参考にさせていただきました。
[Next.js における環境変数 (env) の基本的な設定方法
](https://fwywd.com/tech/next-env)

### ルートディレクトリに.envを作成
Next.jsでは.envを使うために特別な処理は必要ないので、そのままルートディレクトリに.envというファイルを作成します。

### 必要な情報を書く
`
NEXT_PUBLIC_SITE_NAME=私のポートフォリオ
NEXT_PUBLIC_SITE_URL=https://example.com
NEXT_PUBLIC_FB_ID=Facebookのapp_idを入力
`
ブラウザで表示する内容に関してはPUBLICとつけないとundefindとなるため忘れないようにします。

### PageHead/index.jsxで置き換える
`jsx
// components/PageHead/index.jsx

<title>
/* ページのタイトル | サイトの名前 */
  {title} | {process.env.NEXT_PUBLIC_SITE_NAME}
</title>
`
※注意！！
.envまわりの設定をした時はローカルサーバーの立ち上げをしなおさないと反映がされませんでした。
私は打ち間違いがないかなどをチェックして解決するまでに時間がかかってしまったため.envの設定をしたら必ず`yarn dev`や`npm run dev`をしなおすようにしましょう。

ブラウザのデベロッパーツールで反映されていればOKです。

## シェア時に反映されているかチェック
### Facebook
[シェアデバッガー](https://developers.facebook.com/tools/debug/?locale=ja_JP)

![スクリーンショット 2021-07-19 16.17.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/32883497-f558-e993-a33f-02adc1095cc4.png)

### Twitter
[Card validator](https://cards-dev.twitter.com/validator)

![スクリーンショット 2021-07-19 16.17.53.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/c105e897-5035-8e1c-ec0f-da5b75f5e5b8.png)

# サイト公開まで
## ドメイン取得
今回は[Google Domains](https://domains.google/intl/ja_jp/)で取得しました。
管理画面や手続きがシンプルでわかりやすくオススメです。
### ドメイン検索

![スクリーンショット 2021-07-19 15.01.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/be1cc1db-c8b4-19f8-a8ad-23707347c37b.png)

### カートに入れて購入

![スクリーンショット 2021-07-19 15.05.14.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/9eb01741-de9e-e617-e6bc-6d6f0f08dbce.png)

## Vercelにデプロイ
サイトの公開に[Vercel](https://vercel.com/)を利用します。
登録など諸々を済ませて管理画面に進みます。

### New Projectの作成
管理画面からNew Projectをクリックします。

![スクリーンショット_2021-07-19_16_44_47.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/159e43f8-a637-2a46-5ade-afb5ff52d044.png)


### GitHubからインポートする

![スクリーンショット_2021-07-19_16_46_43.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/afa300fb-9f40-dad6-613c-bbdc59ba67ad.png)

### 手順通りに進める
Create a TeamはスキップしてOKです。

![スクリーンショット_2021-07-19_15_18_46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/e1318698-fa94-b5c2-2369-aa01e1529198.png)

DeployをクリックするとVercelでURLが作成されデプロイが完了します。
表示されたURLにアクセスすると制作したサイトにアクセスできます。
スマホの実機確認などがこれで手軽にできるのでとても便利です。

## ドメインを反映させる
サイトの公開はされましたが先ほど取得したドメインからはまだアクセスができないのでその設定をします。

### 管理画面から先ほどデプロイされたポートフォリオサイトを選択

![スクリーンショット 2021-07-19 16.51.12.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/5e6e8a39-02f4-9c97-a6cf-54a49eb76177.png)

### 設定画面に移動して先ほど取得したドメインを入力

![スクリーンショット_2021-07-19_16_53_06.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1686114/7a89a0f0-6dbd-a38c-38c6-550fc6f91785.png)

### ネームサーバー設定
なぜか私の画面にVercelのネームサーバーが表示される画面がありませんでした…
下記の記事を参考にしてみてください。
[Google Domainsで取得したドメインとnotion blog を繋ぐ](https://jhcoder.com/2020-08-16)

これで無事取得したドメインでのサイト公開ができたと思います。
Vercelでは5分もあればドメインが反映されるのでとても便利です。

# fwywdさんの一次試験にチャレンジした感想
ここまで読んでいただきありがとうございました。
文章を書くということに慣れておらず読みにくい部分もあったかと思いますが、少しでも誰かのお役に立てることができれば幸いです。
また、こうして経験したことを記事にするということでたくさんの発見がありました。

- 自分が今何がわからないのか明確になる
- 他の人がわかるようにと思うと知識の深堀りが必要になり、それが結果的に役に立つ
- 自分がよかれと思って伝えようと思ったことが、他の人のわかりやすさにつながるとは限らない
- 伝えたい人を明確にイメージして先回りした記事を執筆するために機転が必要になること

これらは記事を書くということだけではなく、普段の生活やビジネスの中にも重要な部分を占めているように思います。
「伝えたい誰かをイメージして記事を書く」ことによってたくさんの発見につながるといったことが最大の収穫でした。

このような素晴らしい課題を公開してくださった fwywd さんには感謝の気持ちでいっぱいです。ありがとうございました。

また、この記事が参考になったと思ったらぜひLGTMを押していただけると嬉しいです！