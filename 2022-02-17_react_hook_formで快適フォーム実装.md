<!--
title:   react hook form で快適フォーム実装
tags:    JavaScript,React,react-hook-form,useState
id:      7e953cbd4677e15ec511
private: false
-->
Reactでのフォーム実装にはreact hook formが便利でした！

- 無駄なレンダリングを減らしてくれてパフォーマンスアップ
- ルールに合わない場合のreturn処理やエラーメッセージを表示するなどの処理が簡潔に書ける
- useStateの量が減るのでコードがスッキリする

などなど、メリットがたくさんです！

[https://react-hook-form.com/](https://react-hook-form.com/)

# インストール

```bash
npm install react-hook-form

or

yarn add react-hook-form
```

# 実装

inputの入力内容をalertで表示させる機能を実装します。

その際に、

- 入力がなければreturnしてエラーメッセージを表示する
- 入力があればエラーメッセージを表示させない
- alert表示後はフォーム内はリセットする

機能も実装しました。

## useStateでの実装

[https://codesandbox.io/s/usestate-form-381rvq](https://codesandbox.io/s/usestate-form-381rvq)

```jsx
import { useState } from "react";

export default function useStateForm() {
	const [text, setText] = useState("");
  const [error, setError] = useState(false);

  const onSubmit = () => {
    // フォーム未入力の場合にerror === trueにする
    if (!text) {
      setError(true);
      return;
    }
    alert(text);
    // 送信完了後にフォームリセット、error === falseにする
    setText("");
    setError(false);
  };

  return (
    <form method="dialog" onSubmit={onSubmit}>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
      {/* フォーム未入力＋error === trueの場合にエラーメッセージ表示 */}
      {!text && error && <span>入力してください</span>}
    </form>
  );
}
```

## react-hook-formでの実装

[https://codesandbox.io/s/react-hook-form-wedw9k?file=/src/App.js](https://codesandbox.io/s/react-hook-form-wedw9k?file=/src/App.js)

```jsx
import { useForm } from "react-hook-form";

export default function ReactHookForm() {
  const { handleSubmit, formState: { errors }, register, reset } = useForm();

  const onSubmit = (formData) => {
    alert(formData.text);
    reset();
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("text", { required: true })} />
      {errors.text && <span>入力してください</span>}
    </form>
  );
}
```

onSubmitの関数部分が簡潔にできました！

主にエラー時のreturn処理やエラー状態の制御、送信後のフォームリセットの記述が削減できています。

その他にもinputの数が増える場合などでuseStateの数が増えることを防げることや、エラーメッセージを出すための処理が簡単に実装できます。

開発が進んで処理が増えていくごとにコード量に差が出てくるのですごく良かったです！

# まとめ

react hook form最高！