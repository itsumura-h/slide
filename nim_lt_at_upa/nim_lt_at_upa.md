---
marp: true
theme: default
---

# Nimについて
発表者: medy (@dumblepytech1)

---
# Nimって何よ？
## 2008年から開発が始まった新しいプログラミング言語
## GoやRustが競合です

---
# Nimの何が良いの？
アジェンダ
1. 速い
1. 軽い
1. 書きやすく読みやすい
1. 整った環境
1. どこでも動く
1. C言語と連携が簡単
1. 何でもできる
1. 私がNimで作ったライブラリ


---
# 速い

---
## Nimはコンパイルすると最適化されたC言語を出力し、C言語をgccやclangを使ってバイナリにコンパイルします。

## Nim→C言語→実行可能バイナリ

## Nimの実行速度≒C言語の実行速度

---
# 軽い

---

## C言語を「最適化」してコンパイルするので、バイナリを非常に小さくできます。

---
動的型付き言語であるPHP、JavaScript、Python、Rubyより速く、C言語、Go、Rustと遜色ないパフォーマンスなことがわかります。

メモリ消費もGoよりも少ないことがわかります。

https://github.com/frol/completely-unscientific-benchmarks

![bg right 90%](./Screenshot%20from%202021-07-02%2013-24-57.jpg)

---
## Webフレームワークで比較
## NimのJesterというフレームワークは圧倒的爆速です
![](Screenshot%20from%202021-07-02%2014-05-47.jpg)

---
# 書きやすく読みやすい

---
## フィボナッチ数列の例

```nim
proc fib(n: int): int =
  if n < 2:
    return n
  else:
    return fib(n - 1) + fib(n - 2)

echo(fib(30))
```

---
## FizzBuzzの例

```nim
let n = 600_000
for i in 1..n:
  if i mod 3 == 0 and i mod 5 == 0: echo "FizzBuzz"
  elif i mod 3 == 0: echo "Fizz"
  elif i mod 5 == 0: echo "Buzz"
  else: echo i
```

---
## GoやRustのようにmain関数は不要
## ファイルにズラズラ書けばそれで動く
## Pythonのようなオフサイドルールで読みやすい！
---
## UFCS
関数の第一引数を前に持ってきてメソッドチェーンのようにできる記法
これによってオブジェクト指向のメソッドを表現する

D言語から取り入れられた機能

---
```nim
type Customer = ref object
  name: string
  email: string

proc getName(self:Customer):string =
  return self.name
```

```nim
let taro = Customer(name: "taro")
echo taro.getName()
>> "taro"

echo getName(taro)
>> "taro"
```

ライブラリ内の独自の型に対してメソッドを後から追加できる

---
## JSONパースが楽
```nim
import json, os

# ファイルを開く
let file = readFile("./file.json")
# 文字列をJsonNode型に変換
let node = file.parseJson
echo node
>> {"key1":{"id":20,"name":"takahashi"},"key2":["val1","val2"]}

echo node["key1"]["id"].getInt
>> 20

echo node["key1"]["name"].getStr
>> "takahashi"

echo node["key2"]
>> ["val1","val2"]
```
---

```nim
# key1に要素を追加
node["key1"]["email"] = %"takahashi@gmail.com"
echo node["key1"]["email"].getStr
>> "takahashi@gmail.com"

# key2に要素を追加
node["key2"].add(%"val3")
echo node["key2"]
>> ["val1","val2","val3"]
```

- Goのように構造体を用意する必要がない
- 動的型付け言語の連想配列ようにJSONを扱うことができる

---

# 整った環境

---
## 環境構築
https://github.com/dom96/choosenim
![](./Screenshot%20from%202021-07-02%2014-29-00.jpg)

---
MacOS、Linuxなら1行で環境構築が完了

```sh
curl https://nim-lang.org/choosenim/init.sh -sSf | sh
```

WindowsでもGithubのリリースページからZipをダウンロードし
解凍して中の`runme.bat`を実行するだけ

もちろんDockerイメージもDockerhubにあります

---
## 標準ライブラリ
標準ライブラリの全ての関数とサンプル、型一覧や関数の引数の型は何かがまとめられている

Pythonの「battery included」の思想を受け継いでおり、めちゃくちゃ標準ライブラリが豊富

![bg right 100%](./Screenshot%20from%202021-07-02%2014-35-53.jpg)

---
## 外部ライブラリ
検索すればライブラリが見つかる
`nimble install`コマンドでインストールできる（`npm install`みたいな）
https://nimble.directory

![](./Screenshot%20from%202021-07-02%2014-32-05.jpg)

---
## VSCodeの拡張機能
- シンタックスハイライト
- 補完
- マウスオーバーで関数定義の表示
- 定義元ジャンプ

![bg right 100%](./Screenshot%20from%202021-07-02%2014-42-04.jpg)

---
# どこでも動く

---
## Windows
![](./Screenshot%20from%202021-07-02%2015-13-48.jpg)

---
## Android
![](./Screenshot%20from%202021-07-02%2015-14-45.jpg)

---
## iOS
![](./Screenshot%20from%202021-07-02%2015-15-35.jpg)

---
## Nintendo Switch
![](./Screenshot%20from%202021-07-02%2015-16-23.jpg)

---
# IoT
http://mpu.seesaa.net/article/463328035.html
![](./Screenshot%20from%202021-07-02%2015-18-57.jpg)

---

# C言語と連携が簡単

---
## 各OS毎のdllやsoファイルをロードして
```nim
when defined(nimHasStyleChecks):
  {.push styleChecks: off.}

when defined(windows):
  const
    dllName = "libpq.dll"
elif defined(macosx):
  const
    dllName = "libpq.dylib"
else:
  const
    dllName = "libpq.so(.5|)"
```
---

C言語のドキュメント通りに関数定義作れば、
C言語の関数をNimから呼べる

![](./Screenshot%20from%202021-07-02%2016-09-58.jpg)
```nim
PPGconn* = ptr PGconn
PPGresult* = ptr PGresult

proc pqexec*(conn: PPGconn, query: cstring): PPGresult{.cdecl, dynlib: dllName,
    importc: "PQexec".}
```

---

# 何でもできる

---
## Web
https://github.com/dom96/jester
![](./Screenshot%20from%202021-07-02%2014-22-51.jpg)

---
## デスクトップアプリ
https://github.com/status-im/status-desktop
![](./Screenshot%20from%202021-07-02%2015-43-41.jpg)

---
## 2Dゲーム
https://vladar4.github.io/nimgame2/
![](./Screenshot%20from%202021-07-02%2015-40-37.jpg)

---
## 3Dゲーム
https://github.com/pragmagic/godot-nim
https://qiita.com/NabeKz/items/f952e43d7807935734d0

![](./Screenshot%20from%202021-07-02%2015-47-27.jpg)
![bg right 100%](./Screenshot%20from%202021-07-02%2015-47-55.jpg)

---
## コンテナエンジン
https://github.com/fox0430/nicoru
![](./Screenshot%20from%202021-07-02%2015-50-05.jpg)

---
## OS
https://twitter.com/uchan_nos/status/1359014828090683393
![](./Screenshot%20from%202021-07-02%2016-05-01.jpg)

---
# 私がNimで作ったライブラリ

---
## Basolato
LaravelライクなフルスタックWebフレームワーク
![bg right 100%](./Screenshot%20from%202021-07-02%2017-48-34.jpg)

---
## allographer
Laravelライクなクエリビルダ
![bg right 100%](./Screenshot%20from%202021-07-02%2017-50-43.jpg)

---
## interface-implements
Nimで`インターフェースの実装`を表現できるようになるライブラリ

![bg right 100%](./Screenshot%20from%202021-07-02%2017-51-51.jpg)

---
# 最後に

---
今日のスライドはMarpというVSCodeの拡張機能を使って、マークダウンで作りました。
おすすめです。
![](./Screenshot%20from%202021-07-02%2016-19-31.jpg)
