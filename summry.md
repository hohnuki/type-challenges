**初級編**

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
型エイリアスを用いてHelloWorldという型に、string型を割り当てている。<br>
型に名前を新しくつけているので、以下のように使用できる。
```TS
let greeting: HelloWorld = "Hello World";
```
