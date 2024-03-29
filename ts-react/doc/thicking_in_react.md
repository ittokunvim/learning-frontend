# Reactについて考える

> 参考: https://en.reactjs.org/docs/thinking-in-react.html

Reactは、JavaScriptで大規模かつ高速なWebアプリケーションを構築するための最高の方法だと思います。
FacebookやInstagramでも、Reactは非常によく拡張されています。

Reactの素晴らしいところは、アプリを作りながら考えさせられることです。ここではReactを使って検索可能な商品データテーブルを構築する思考プロセスについてみていきます。

## モックから始める

すでにJSON APIがあり、デザイナーからモックが提供されているとします。モックは以下のようなものです。

![mock image](https://en.reactjs.org/static/1071fbcc9eed01fddc115b41e193ec11/d4770/thinking-in-react-mock.png)

JSON APIは以下のようなものです。

```json
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## Step 1: UIをコンポーネント階層に分割する

まずはじめに、モックの全てのコンポーネントの周りにボックスを書き、全てのコンポーネントに名前をつけます。

しかし、何を独自のコンポーネントとすべきかは、どのように判断すれば良いのでしょうか。これには、単一責任の原則というものがあり、これは、コンポーネントは1つのことだけを行うべきというルールです。
もし、コンポーネントが大きくなるようなら、小さなサブコンポーネントに分解した方が今後のためになります。

JSONデータモデルはユーザーに表示するところが多いので、モデルが正しく構築されていれば、UIもうまくマッピングされます。これは、UIとデータモデルが同じ情報アーキテクチャに準拠する傾向があるためです。
UIをコンポーネントに分割し、各コンポーネントがデータモデルの1つのピースに一致するようにします。

![ProductTable](https://en.reactjs.org/static/9381f09e609723a8bb6e4ba1a7713b46/90cbd/thinking-in-react-components.png)

上記の画像は商品検索アプリについて書かれており、数字には次のようなコンポーネントが当てはまります。

1. FilterableProductTable、サンプルの全体
2. SearchBar、ユーザの入力フォーム
3. ProductTable、ユーザの入力に基づいて、データを表示、フィルタリングする
4. ProductCategoryRow、各カテゴリーの見出し
5. ProductRow、各製品の行を表示

ProductTableを見ると、テーブルのヘッダーがそれ自身のコンポーネントでないことがわかります。これは好みの問題になりますが、この例では、そのコンポーネントはデータのレンダリングに使われています。
もしヘッダーが複雑になっていくことを考えると、この例が最適ではないかと思います。

モック内のコンポーネントを特定できたので、これらを階層構造に並べてみましょう。

- FilterableProductTable
    - SearchBar
    - ProductTable
      - ProductCategoryRow
      - ProductRow

## Step 2: Reactで静的に構築する

> [Product table step 2](https://codepen.io/gaearon/pen/BwWzwm)

コンポーネント改装ができたので、いよいよアプリを実装します。最も簡単な方法は、データモデルを受け取ってUIをレンダリングし、インタラクティブ性を持たないバージョンを構築することです。
なぜなら、静的なバージョンの作成は楽に作ることができるからです。

データモデルをレンダリングする静的バージョンのアプリの構築には、他のコンポーネントを再利用するコンポーネントを構築し、`props`を使用しデータを渡すことになります。
これにはインタラクティブ性、つまり時間と共に変化する要素がないため、`state`を使うことはありません。

アプリの構築には、トップダウン、ボトムアップの2つの方法があります。Reactではどちらも使えます。通常はトップダウンで書いた方が簡単で、大規模なプロジェクトでは、ボトムアップでテストを書きながら構築する方が簡単です。

最終的にはデータモデルをレンダリングする再利用可能なコンポーネントのライブラリが完成します。これはアプリの静的バージョンなので、コンポーネントは`render()`のみを持ちます。
FilterableProductTableは、データモデルを`props`として受け取り、基礎となるデータモデルに変更を加え、再度レンダーしUIを更新します。
Reactの一方向のデータフロー（ワンウェイバインディング）は、全てをモジュール化し、高速にします。

## 休憩: Props vs State

Reactのモデルデータには、`props, state`の2つのタイプがあります。この2つの違いは何でしょう

## Step 3: UIの状態を表す最小限の表現を特定

UIをインタラクティブにするには、基盤となるデータモデルに変更を加える必要があります。Reactではステートでそれを行います。

アプリを正しく構築するためには、まずアプリが必要とする最小限の変更可能な状態のセットから変更していきます。ここで重要なのはDRY(Don't Repeat Yourself)です。

このアプリには次のようなデータがあります。

- 元の商品リスト
- ユーザーが入力した検索文字列
- チェックボックスの値
- フィルタリングされた製品リスト

ここからどれが状態に当たるのかみていきましょう。

- 親から渡されたデータか
- 時間が経っても変化しないか
- コンポーネント内の他の状態やプロップに基づいて計算できるか

元の商品リストは、`props`として渡されるので、`state`ではありません。検索テキストとチェックボックスは時間で変化し、計算できないので、`state`です。
フィルタリングされた商品リストは、これらの値を組み合わせて計算できるため、`state`ではありません。

## Step 4: Stateを使う

> [Product table step 4](https://codepen.io/gaearon/pen/qPrNQZ)

アプリの最小セットは何か特定ができました。次にどのコンポーネントを変異させるか、つまり所有させるか特定する必要があります。

Reactはコンポーネント階層を下る一方通行のデータフローです。初めはどのコンポーネントに状態を所有させるべきかわかりにくく理解しにくい部分ですが、以下の手順に従うことで特定ができます。

- stateに基づいて、レンダリングする全てのコンポーネントを特定する
- 共通のコンポーネントを見つける
- 共通のコンポーネント化、より高い階層のコンポーネントのstateを所有する
- 状態を保有するコンポーネントがない場合は、状態を保持するだけの新しいコンポーネントを作成する

では実際にアプリケーションで実行しましょう。

- ProductTableは状態に応じて商品リストをフィルタリングする必要があり、SearchBarは検索テキストとチェックボックスを表示する必要があります。
- 共通のオーナーコンポーネントはFilterableProductTableです
- フィルタテキストとチェック値がオーナーコンポーネントにあるのは理にかなっています。

## Step 5: 逆方向のデータフローを追加

> [Product table step 5](https://codepen.io/gaearon/pen/LzWZvb)

今回は逆に流れるデータを扱います。回想の深いところにあるフォームコンポーネントは、FilterableProductTableの状態を更新する必要があります。

Reactはｋのデータフローを明示することで、プログラムがどのように動作するかわかりやすくなりますが、少し多くのタイピングが必要になります。

Step 4ではボックスを入力、チェックしても無視されていました。これはデータをデータを下に送ることで解決します。

## まとめ

ここで学んだことが、Reactでコンポーネントやアプリケーションを構築する際の考え方のヒントになれば幸いです。初めは慣れませんが、コードは書くより読まれることの方が多く、Reactのコードを読むのはそれほど難しくはありません。

コンポーネントの大規模なライブラリを構築するようになると、この明治性とモジュール性に感謝するようになり、コードの再利用により、コードの行数が減少し始めることでしょう。:)
