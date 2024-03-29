# このハンドブックについて

> 参考: https://www.typescriptlang.org/docs/handbook/intro.html

プログラミング界に導入されてから20年以上が経過し、JavaScriptはこれまでに作られた言語の中で最も普及しているクロスプラットフォーム言語の1つとなっています。
Webページに些細なインタラクティブ性を追加するための小さなスクリプト言語として始まったJavaScriptは、今ではあらゆる規模のフロントエンドとバックエンド両方のアプリケーションに選ばれる言語へと成長しました。
JavaScriptで書かれたプログラムのサイズ、範囲、複雑さは指数関数的に増大しましたが、JavaScript言語がコードの異なるユニット間の関係を表現する能力は変わりません。
JavaScriptの特殊なランタイムセマンティックと相まって、この言語とプログラムの複雑さのミスマッチは、JavaScriptの開発を大規模に管理することを困難にしています。

プログラマーが各エラーの中で最も一般的なものは、型エラーです。これは単純なタイプミス、ライブラリのAPIの理解、ランタイムの動作に関する誤った過程、またその他のエラーによるものが多いです。
TypeScriptの目標は、JavaScriptプログラムの性的なタイプチェッカーになることです。つまりコードが実行される前に実行され（静的）、プログラムの型が正しいことを保証する（タイプチェックされる）ツールになることです。

## ハンドブックの構成

ハンドブックは2つのセクションに別れています。

**The Handbook**

ここでは、日常のプログラマーにTypeScriptを説明する包括的なドキュメントとなっています。それぞれの章やページで、概念を理解することを目的としています。TypeScriptハンドブックは完全な言語仕様書ではないですが、この言語のすべての機能と動作について書かれています。

このハンドブックを読み終えたとき、以下のことができるはずです。

- TypeScript構文とパターンを読み、理解する
- 重要なコンパイラオプションの効果を説明できる
- 型システムの挙動を正しく予測できる

**Reference Files**

ここでは、TypeScriptの特定の部分がどのように機能するかをより深く理解できるように作られています。上から下へ読み進めるのが目的ではなく、各セクションの1つの概念についてより深い説明を提供することを目的としています。

### 非目標

このハンドブックは、数時間で楽に読めるような簡潔な文書です。そのため細かいところはカバーしていません。
特に、関数、クラス、クロージャのようなJavaScriptの基本的な部分については、ここでは十分には説明しません。

またこのハンドブックは、言語使用の代わりにはなりません。場合によっては、エッジケースや動作の正式な記述を省略し、高レベルで理解しやすい説明をすることもあります。その代わり、TypeScriptの動作の多くの側面について、より正確で形式的な説明を行う参考ページが別で用意されています。

最後にここでは必要な場合を除きTypeScriptが他のツールとどのように組み合わせるかなどは説明しないことにしています。他のツールとは、`Webpack、rollup、parcel、react、babel、closure、lerna、rush、bazel、preact、vue、angular、svelte、jquery、yarn、npm`などです。

## 始める

TypeScriptについて学ぶ前に、TypeScript for JavaScript Programmersというページを先に見ます。これは自分が好きなプログラミング言語の主な相違点を強調し、それらの言語に特有の一般的な誤解を解くことを目的としています。
