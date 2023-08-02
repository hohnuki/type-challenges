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
**4.タプル型からオブジェクトを生成する型を作ろう**

問題<br>
タプルを受け取り、その各値のkey/valueを持つオブジェクトの型に変換する型を実装してください。
```TS
// Tuple型からObject型を生成するTupleToObjectを実装してください。
const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const

type result = TupleToObject<typeof tuple> // expected { tesla: 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}

```
回答<br>
```TS
// Tuple型からObject型を生成するTupleToObjectを実装してください。
type TupleToObject<T extends readonly any[]> = {
  [K in T[number]]: K
}

const tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const

type result = TupleToObject<typeof tuple> // expected { 'tesla': 'tesla', 'model 3': 'model 3', 'model X': 'model X', 'model Y': 'model Y'}
```
メモ<br>
Mapped Typesを数字の型に利用してオブジェクト型を生成している。また、as constの配列を型パラメータとして受け取れるようにreadonly修飾子を利用している。<br>
- Mapped Types
```TS
type tuple = ['tesla', 'model 3', 'model X', 'model Y'] as const;

type Cars = typeof tuple[number]; // 'tesla' | 'model 3' | 'model X' | 'model Y'
```
- typeof<br>
既存の変数やオブジェクトから新しい型を作り出すために使用される。コンパイル時に変数やオブジェクトの型を参照する。使用例は以下。
```TS
let person = { name: "Alice", age: 25 };

// 'typeof person' は { name: string, age: number }型を指す。
let anotherPerson: typeof person;

anotherPerson = { name: "Bob", age: 20 }; //OK
anotherPerson = { name: "Charlie" }; //Error!!
```
- 索引型（Indexed type）
型が配列またはタプル型のときに、配列の各要素の型を取得する。
```TS
type ArrayType = string[];
type ElementType = ArrayType[number]; // stringになる

type Tuple = [string, number];
type ElementType = Tuple[number]; // string | numberになる
```

---
**5.タプル型に要素を追加するPush型を実装する**

問題<br>
Array.pushのジェネリックバージョンを実装してください。
```TS
// Push型を定義してください
// 例えば、Push<[1, 2], '3'>は[1, 2, '3']を返します

type Result = Push<[1, 2], '3'> // [1, 2, '3']

type Push<T> = 
```
回答<br>
```TS
// Push型を定義してください
// 例えば、Push<[1, 2], '3'>は[1, 2, '3']を返します

type Result = Push<[1, 2], '3'> // [1, 2, '3']

type Push<T extends any[], U> = [...T, U];
```
メモ<br>
スプレッド演算子の利用と`T extends any[]`で配列しか受け取れないように制約をかけるのがポイント。
- スプレッド演算子
スプレッド演算子`...`は配列やオブジェクトの中身を展開する演算子。
```TS
type Foo = [1, 2, 3];

type Bar = [...Foo, 4]; // [1, 2, 3, 4]
```
---
**6.タプル型の先頭に要素を追加する型を作ろう**

問題<br>
Array.unshiftの型バージョンを実装してください。
```TS
// Unshiftを実装してください

type Result = Unshift<[1, 2], 0> // [0, 1, 2,]
```
回答<br>
```TS
// Unshiftを実装してください
type Unshift<T extends any[], U> = [U, ...T];

type Result = Unshift<[1, 2], 0> // [0, 1, 2,]
```
---
**7.オブジェクト型の一部を選択する型を作ろう**

問題<br>
組み込みの型ユーティリティPick<T, K>を使用せず、TからKのプロパティを抽出する型を実装してください。
```TS
interface Todo {
  title: string
  description: string
  completed: boolean
}

/* _____________ ここにコードを記入 _____________ */

type MyPick<T, K> = any
  
type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
}
```
回答
```TS
interface Todo {
  title: string
  description: string
  completed: boolean
}

/* _____________ ここにコードを記入 _____________ */

type MyPick<T, K extends string | number | symbol> = {
  [k in K]: k extends keyof T ? T[k] : never;
}
  
type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
  title: 'Clean room',
  completed: false,
}

```
メモ<br>
Mapped TypesでKの型を1個ずつ参照し、参照したプロパティに三項演算子を用いて型付けしている。`k extends keyof T`みたいな感じでTのプロパティを継承しているか確認するのがミソ。
~~むずかしい。明日もここでいいかも。~~
- Conditional Types<br>
三項演算子かな。
```TS
type Foo<T> = T extends string ? string : number;

type FooString = Foo<string>; // string
type FooNumber = Foo<boolean>; // number
```
- never型<br>
この型を使用すると型をないものとして扱える。
```TS
type Exclude<T, U> = T extends U ? never : T;

type Foo = Exclude<string | number | boolean, boolean>; //string | number
```
- symbol型<br>
~~ちょっとよくわかんない。~~ ⇚ プロパティキーは通常 stirng | number | symbol のいずれかでなかればならないから。回答のextendsしてる。
```TS
const person = {
	name: 'Alice', // "name"はプロパティキー string
	age: 30,       // "age"はプロパティキー number
	[Symbol('id')]: 1234 // "Symbol('id')はプロパティキー Symbol
}
```

---
**8.条件分岐する型を作ろう**

問題<br>
条件値C、 Cが truthy である場合の戻り値の型T、Cが falsy である場合の戻り値の型Fを受け取るIfを実装します。
```TS
// type Ifを実装してください。
type A = If<true, 'a', 'b'>; // expected to be 'a'
type B = If<false, 'a', 'b'>; // expected to be 'b'
```
回答
```TS
// type Ifを実装してください。
type A = If<true, 'a', 'b'>; // expected to be 'a'
type B = If<false, 'a', 'b'>; // expected to be 'b'

type If<C extends boolean, T, F> = C extends true ? T : F;
// type If<T extends boolean, U extends string, K extends string> = T extends true ? U : K;
```
メモ<br>
Conditional Types（三項演算子？）と用いる。
