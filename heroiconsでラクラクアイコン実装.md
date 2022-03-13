<!--
title:   heroicons でラクラクアイコン実装
tags:    React,heroicons,icon,next.js,tailwindcss
id:      07c1512bba08659fa8f3
private: false
-->

TailwindCSS の作成者によって作られており簡単に便利なアイコンの実装ができます！

[https://heroicons.com](https://heroicons.com/)

[https://github.com/tailwindlabs/heroicons](https://github.com/tailwindlabs/heroicons)

# 1. SVG 画像から実装する方法

![heroicons.png](image/heroicons/heroicons.png)

```jsx
// 貼り付け

export const Index = () => {
  return (
    <div>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        className="h-5 w-5"
        viewBox="0 0 20 20"
        fill="currentColor"
      >
        <path
          fillRule="evenodd"
          d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-11a1 1 0 10-2 0v2H7a1 1 0 100 2h2v2a1 1 0 102 0v-2h2a1 1 0 100-2h-2V7z"
          clipRule="evenodd"
        />
      </svg>
    </div>
  );
};
```

## メリット

- ライブラリのインストールが不要

## デメリット

- コードが長くなる

（icons ディレクトリなどにコンポーネント作成しインポートするとコードがスッキリする）

例）

```jsx
// iconsディレクトリ内にコンポーネント作成
export const PlusIcon = () => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      className="h-5 w-5"
      viewBox="0 0 20 20"
      fill="currentColor"
    >
      <path
        fillRule="evenodd"
        d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-11a1 1 0 10-2 0v2H7a1 1 0 100 2h2v2a1 1 0 102 0v-2h2a1 1 0 100-2h-2V7z"
        clipRule="evenodd"
      />
    </svg>
  );
};

// インポートする
import { PlusIcon } from "../icons/PlusIcon";

export const Index = () => {
  return (
    <div>
      <PlusIcon />
    </div>
  );
};
```

# 2.ライブラリをインストールして実装する方法

```bash
npm install @heroicons/react

or

yarn add @heroicons/react
```

```jsx
import { PlusCircleIcon } from "@heroicons/react/solid";

const Index = () => {
  return (
    <div>
      <PlusCircleIcon className="w-5 h-5" />
      <p>...</p>
    </div>
  );
};
```

## メリット

- コードがスッキリする
- ディレクトリを増やさなくて済む

## デメリット

- ライブラリのインストールが必要

# まとめ

シンプルでおしゃれなアイコンがラクラク実装できます！
