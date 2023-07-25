# 初級編

**1.Hello World**

問題<br>
any型として定義されているHelloWorldをstring型として定義してみましょう。
```TS
// HelloWorldを文字列型にしてください
type HelloWorld = any;
```
回答
```TS
// HelloWorldを文字列型にしてください
type HelloWorld = string;
```
メモ<br>
TypeScriptの型エイリアスを用いてHelloWorldという型に、string型を割り当てている。<br>
型に名前を新しくつけているので、以下のように使用できる。
```TS
let greeting: HelloWorld = "Hello World";
```
---
**2.Readonly型を定義してみよう**

問題<br>
組み込みの型ユーティリティReadonly< T >を使用せず、T のすべてのプロパティを読み取り専用にする型を実装してみよう。実装された型のプロパティは再割り当てできません。
```TS
// MyReadonly<T>を実装してください
// MyReadonlyは、Tのプロパティをすべて読み取り専用にする型です

interface Todo {
  title: string
  description: string
}

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}

```

回答
```TS
// MyReadonly<T>を実装してください
// MyReadonlyは、Tのプロパティをすべて読み取り専用にする型です

type MyReadonly<T> = {
  readonly [K in keyof T]: T[K]
}

interface Todo {
  title: string
  description: string
}

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}

```
メモ<br>
TypeScriptのMapped Typesを用いて、既存の型から新しい型を生成している。またオブジェクトのキーを取得するためにkeyofを使用し、オブジェクトの型を取得するためにインデックスアクセス型も使用している。<br>

- Mapped Types<br>
例は以下。`K in Todokeys` はループ文を回してるイメージかな。
```TS
type TodoKeys = "title" | "description"

type Todo = {
	[K in TodoKeys]: string
}

//以下のように展開される
type Todo = {
	title: string
	description: string
}
```
- key of<br>
オブジェクトのキーを取得するキーワード。`keyof T` とすると `T` のキーを取得できる。
```TS
inter fase Todo {
	title: string
	description: string
}

type TodoKeys = keyof Todo; // "title" | "description"
```

- インデックスアクセス型<br>
オブジェクトのプロパティにアクセスするための機能。
```TS
interface Todo {
	title: string
	description: string
}

type Title = Todo["string"] // string
```

---
**3.タプル型の長さを返すLength< T >を作ろう**

問題<br>
タプルTを受け取り、そのタプルの長さを返す型Length<T>を実装してください。
```TS
// タプル型の長さを返すLength<T>を実装してください。

type tesla = ['tesla', 'model 3', 'model X', 'model Y']
type spaceX = ['FALCON 9', 'FALCON HEAVY', 'DRAGON', 'STARSHIP', 'HUMAN SPACEFLIGHT']

type teslaLength = Length<tesla>  // expected 4
type spaceXLength = Length<spaceX> // expected 5

```
回答
```TS
// タプル型の長さを返すLength<T>を実装してください。
type Length<T extends any[]> = T["length"];

type tesla = ['tesla', 'model 3', 'model X', 'model Y']
type spaceX = ['FALCON 9', 'FALCON HEAVY', 'DRAGON', 'STARSHIP', 'HUMAN SPACEFLIGHT']

type teslaLength = Length<tesla>  // expected 4
type spaceXLength = Length<spaceX> // expected 5
```

メモ<br>
タプル型（配列型）から要素の長さを取得するには、`T['length']` というインデックスを使用する。使用例は以下。<br>
※このインデックスを使用できるように、`T` を `T extends any[]` と宣言して配列を受けるようにしてる。
```TS
type tesla = ['tesla', 'model 3', 'model x', 'model y'];
type teslaLength = tesla['length']; // 4
```
この方法で得られる長さは静的なものだから、動的なものには対応できないみたい。<br>
⇒ 動的なものはJavaScriptで対応する。`tesla.length` とか。

---
