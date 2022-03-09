---
marp: true
theme: default
---

# Pythonistaに伝えたいNimの魅力
発表者: medy (@dumblepytech1)

---
# Nimって何？
## 2008年から開発が始まった新しいプログラミング言語
## 静的型付き言語でC言語にトランスパイルされる
---
# 読みやすい！書きやすい！
---
フィボナッチ数列（mypy型付きPythonとの比較）

### Python
```python
def fib(n: int) -> int:
  if n < 2:
    return n
  else:
    return fib(n - 1) + fib(n - 2)

print(fib(30))
```

### Nim
```nim
proc fib(n: int): int =
  if n < 2:
    return n
  else:
    return fib(n - 1) + fib(n - 2)

echo(fib(30))
```
---
# 速い！
---
## Nimはコンパイルすると最適化されたC言語を出力し、C言語をgccやclangを使ってバイナリにコンパイルします。

## Nim→C言語→実行可能バイナリ

## Nimの実行速度≒C言語の実行速度

---
# 軽い！
---

## C言語を「最適化」してコンパイルするので、バイナリを非常に小さくできます。

---
動的型付き言語であるPHP、JavaScript、Python、Rubyより速く、C言語、Go、Rustと遜色ないパフォーマンスなことがわかります。

メモリ消費もGoよりも少ないことがわかります。

https://github.com/frol/completely-unscientific-benchmarks

![bg right 90%](./Screenshot%20from%202021-07-02%2013-24-57.jpg)
---
---

# 整った開発環境

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

Sphinxに似た機能が言語自体に備わっている（nimble docコマンド）

Pythonの「battery included」の思想を受け継いでおり、めちゃくちゃ標準ライブラリが豊富

![bg right 100%](./Screenshot%20from%202021-07-02%2014-35-53.jpg)

---
## 外部ライブラリ
検索すればライブラリが見つかる
`nimble install`コマンドでインストールできる（`pip install`みたいな）
https://nimble.directory

![](./Screenshot%20from%202021-07-02%2014-32-05.jpg)

---
## VSCodeの拡張機能
- シンタックスハイライト
- 補完
- マウスオーバーで関数定義の表示
- 定義元ジャンプ

※複数あるがnimsaem製がオススメ

![bg right 100%](./Screenshot%20from%202022-03-09%2013-39-12.jpg)

---
# Pythonとの連携
## nimpyパッケージを使って　https://github.com/yglukhov/nimpy
---
## NimをPythonに組み込んで高速化
引用：[Nimを組み込んでPythonを高速化してみた](https://zenn.dev/megane_otoko/articles/029_nim_for_python)

Nim
```nim
# セル内のfib関数をfib.minとして書き出し
%%writefile fib.nim
import nimpy

proc fib(n: int): int {.exportpy.} =
    if n == 0:
        return 0
    elif n < 3:
        return 1
    return fib(n-1) + fib(n-2)
```

コンパイル
```sh
nim c --tlsEmulation:off --app:lib --out:ファイル名.so ファイル名.nim
```
---
Python
```python
import sys

# ライブラリのインポート先を追加
sys.path.append("/content")

# 作成・コンパイルしたfibファイルをインポート
import fib

print(fib.fib(45))
```
---
## NimからPythonを呼び出す

```sh
# nimpyライブラリをインストール
nimble install nimpy
```

Nimからnumpyを呼び出せる
```nim
let np = pyImport("numpy")
```

※Pythonの型をNimの世界に変換したりは必要
※詳しくはこちら　https://qiita.com/k4saNova/items/5bb67d1cb40ba90431af

---

# メモリ最適化
参考
[Nimを調べてみる](https://zenn.dev/hastur/scraps/598f88b2b1ae14)
[Nimの新しいGC、ARCについて](https://qiita.com/dumblepy/items/be660c17556d73aa3570)

---
## C言語に変換する時にコンパイラが自動で左から右に自動で変換
.
.
.
.
.
.
.
.
.
.
.
![bg 70%](./Screenshot%20from%202022-03-09%2014-02-20.jpg)
![bg 70%](./Screenshot%20from%202022-03-09%2014-02-49.jpg)

---
## C言語のfreeがスコープ後に自動で挿入される
## スコープを抜けた変数がGCを使わず即時開放される
## GCは循環参照にのみ動く
## 全てコンパイラがやってくれるので開発者は何も考える必要が無い！

---
# 本編は以上です、ありがとうございました
## スライドはStapyのCompassページに上げているので続きもぜひご覧ください！
# ↓↓↓おまけに続く↓↓↓

---

# おまけ
---

## 動的型付け言語の連想配列のように扱えるJSON
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
# %を付けるとJsonNode型に変換される
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
# whenはコンパイル時に動く

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
## コンテナエンジン（！？）
https://github.com/fox0430/nicoru

https://www.youtube.com/watch?v=mUWOe2NUWm4
![](./Screenshot%20from%202021-07-02%2015-50-05.jpg)

---
## OS
https://twitter.com/uchan_nos/status/1359014828090683393
![](./Screenshot%20from%202021-07-02%2016-05-01.jpg)

---
# 私がNimで作ったライブラリ

---
## Basolato
https://github.com/itsumura-h/nim-basolato

LaravelライクなフルスタックWebフレームワーク
![bg right 100%](./Screenshot%20from%202021-07-02%2017-48-34.jpg)

---
## allographer
https://github.com/itsumura-h/nim-allographer

Laravelライクなクエリビルダ
![bg right 100%](./Screenshot%20from%202021-07-02%2017-50-43.jpg)

---
## interface-implements
https://github.com/itsumura-h/nim-interface-implements

Nimで`ポリモーフィズム`を表現できるようになるライブラリ

![bg right 100%](./Screenshot%20from%202021-07-02%2017-51-51.jpg)
