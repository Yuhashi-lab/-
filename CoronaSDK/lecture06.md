![yuhashi-lab](http://www.yuhashi-lab.net/_/rsrc/1436515256182/config/customLogo.gif?revision=1)

2016/10/04

スマートフォンアプリのプログラミング⑥

静岡大学情報学部遊橋研究室(yuhashi-lab.net)


## 1.APIとは

APIとは「アプリケーションプログラミングインターフェース」を約した言葉で、自己のソフトウェアを一部公開して他のソフトウェアと機能を共有できるようにしたものです。
ソフトウェアの一部をWEB上に公開することによって、誰でも外部から利用することができるようになります。
それによって、自分のソフトウェアに他のソフトウェアの機能を埋め込むことができるようになるので、アプリケーション同士で連携することが可能になります。

APIでデータを取りに行くことを「APIを叩く」といいます。

## 2.プログラミング

サーバーとのデータ連携をする場合のプログラミングです。
参考サイトは[こちら](http://offsidenow.phpapps.jp/archives/199)です。

APIを実際に使っていません。
Corona sdkでAPIを使ったサンプルの説明が書かれているサイトは[こちら](http://sharakova.hatenablog.com/entry/20120208/1328673973)。

## 3.JSONを取得してパース（解析）する

サーバーとのデータ連携を行う設計にしてhttp通信を使う必要があります。
JSONとはJavaScript Object Notationの略で、XMLなどと同様のテキストベースのデータフォーマットです。

___main.lua:___
```lua
network.request("http://n0.x0.to/pbl/sample/test.json", "GET", networkListener)

```

* __GET__でJSONを取得します。

## 4.配列に変換

___main.lua:___
```lua
local t = json.decode( event.data )

```

* __json.decode__で配列に変換します。

## 5.データの取得

今回はquestionのデータを取得します。

___main.lua:___
```lua
	for key,value in pairs(t.response) do
            --questionだけ取り出す
            myGet.text = myGet.text .. value.MoridaiQuestion.question .. "\n\n"
        end

```

* for文で繰り返して配列から順番に値を取り出します。

## 6.参考ソースコード

___main.lua:___
```lua
--[[CoronaSDKでGETの実装+Jsonのパース]]
--表示用のテキスト
local myGet = display.newText("[レスポンス]\n", 70, 10, 200,500, native.systemFont, 20)

local json = require "json"
local function networkListener( event )
	if ( event.isError ) then --通信が失敗
		myGet.text = "Network error!"
	else --通信が成功
		local t = json.decode( event.response )
		for key,value in pairs(t.response) do
			--questionだけ取り出す
			myGet.text = myGet.text .. value.MoridaiQuestion.question .. "\n\n"
		end
	end
end
network.request("http://n0.x0.to/pbl/sample/test.json", "GET", networkListener)

```
