![yuhashi-lab](http://www.yuhashi-lab.net/_/rsrc/1436515256182/config/customLogo.gif?revision=1)

2016/10/4

スマートフォンアプリのプログラミング④

静岡大学情報学部遊橋研究室(yuhashi-lab.net)


## 1.サンプルの紹介

* Corona Labs⇒Corona SDK⇒Sample Code⇒Interface⇒MapView
* Corona Simulator ⇒ Samples ⇒ Interface ⇒ MapView

※１：上記を参照していただければネイティブ機能の利用によるマップの表示を理解できるかと思います。
※２：今回は上記のサンプルを若干改変したものを使用し、マップの表示のみ＋αを紹介します。
※３：シミュレータはマップ表示に対応していないため、実際の仕様確認はAndroid端末を用意して行ってください。

## 2.プログラミング（端末のネイティブ機能の利用－マップの表示－）

### ライブラリ宣言
___main.lua:___
```lua
-- Your code here

-- ライブラリの利用
local widget = require( "widget" )

```

### 背景の設定
___main.lua:___
```lua
-- Your code here
-- 背景（白）の設定
local background = display.newRect( 0, 0, display.contentWidth, display.contentHeight )
-- backgroundの左端を原点に設定する
background.anchorX = 0
background.anchorY = 0
background:setFillColor( 1 )

-- 背景（灰）の設定
local shadow = display.newRect( 7, 7, 306, 226 )
-- shadowの原点を左端に設定する
shadow.anchorX = 0
shadow.anchorY = 0
shadow:setFillColor( 0, 0, 0, 120/255 )

```

* 上記ではアンカーの値を0とすることでオブジェクトの左端を原点にしている。
* 本来のアンカーの値は0.5でオブジェクトの中心が原点となっており、1の場合は右端となる。

### マップの下部テキストの作成（シュミレータなど、マップを表示できないときに可視化されるテキスト）
___main.lua:___
```lua
-- Your code here

-- マップ非対応時に表示するテキスト
local simulatorMessage = "MapView is not supported on this platform.You should build for iOS or Android to try MapView."

-- マップ非対応時に表示するラベル
local label = display.newText( simulatorMessage, 20, 70, shadow.contentWidth - 10, 0, native.systemFont, 14 )
-- オブジェクトの左端を原点に設定する
label.anchorX = 0
label.anchorY = 0

```

### マップの作成
___main.lua:___
```lua
-- Your code here

-- ネイティブ機能を使用したマップの作成
local myMap = native.newMapView( 20, 20, 300, 220 )

-- マップの設定
if myMap then
	-- 通常のマップを指定
	myMap.mapType = "normal"
	-- マップの位置設定
	myMap.x = display.contentWidth / 2
	myMap.y = 120

	-- マップの中心(緯度，経度)を設定
	myMap:setCenter( 34.649395, 134.9992893 )

```

* 通常の地図(normal)以外にも航空地図(satellite)や複数種を合成した地図(hybrid)もある。

### イベント設定
___main.lua:___
```lua
-- Your code here

-- マップをタッチした時のイベントに対する処理
local function mapTapListener( event )
--タッチした(緯度，経度)を文字列として出力
print( "The tapped location is at: " .. event.latitude .. ", " .. event.longitude )
	--コメントのタイトルとサブタイトル(緯度，経度)
	local options = { 
		title = 'You tapped here!',
		subtitle = event.latitude .. ' , ' .. event.longitude, 
	}
	--ピンを落とすというイベント
	myMap:addMarker( event.latitude, event.longitude, options )
end
end

```

### テキストボックス設定
___main.lua:___
```lua
-- Your code here

-- 画面をタッチした時のテキストボックスへのフォーカス(今回は一つなので指定の必要なし)
local fieldHandler = function( event )
	-- テキストボックスにタッチしたのと同時にネイティブ機能キーボードを出す
	if ( "submitted" == event.phase ) then
		native.setKeyboardFocus( nil )
	end
end

-- オブジェクトの左端を原点に設定し、テキストボックス作成
display.setDefault( "anchorX", 0 )
display.setDefault( "anchorY", 0 )
local inputField = native.newTextField( 10, 250, 300, 38 )
inputField.font = native.newFont( native.systemFont, 16 )

-- テキスト内容(今回は手動で入力している)
inputField.text = "Hyogo Prefecture, Akashi, the National Astronomical Observatory" -- example of searchable location
inputField:setTextColor( 45/255, 45/255, 45/255 )

```

### サンプルソース

___main.lua:___
```lua
-- Your code here

local widget = require( "widget" )

local background = display.newRect( 0, 0, display.contentWidth, display.contentHeight )
background.anchorX = 0
background.anchorY = 0
background:setFillColor( 1 )

local shadow = display.newRect( 7, 7, 306, 226 )
shadow.anchorX = 0.0
shadow.anchorY = 0.0
shadow:setFillColor( 0, 0, 0, 120/255 )

local simulatorMessage = "MapView is not supported on this platform.You should build for iOS or Android to try MapView."
local label = display.newText( simulatorMessage, 20, 70, shadow.contentWidth - 10, 0, native.systemFont, 14 )

label.anchorX = 0
label.anchorY = 0

local myMap = native.newMapView( 20, 20, 300, 220 )

if myMap then
		myMap.mapType = "normal"
		myMap.x = display.contentWidth / 2
	myMap.y = 120
		myMap:setCenter( 34.649395, 134.9992893 )

	local function mapTapListener( event )
		print( "The tapped location is at: " .. event.latitude .. ", " .. event.longitude )
		local options = { 
			title = 'You tapped here!',
			subtitle = event.latitude .. ' , ' .. event.longitude, 
		}
		myMap:addMarker( event.latitude, event.longitude, options )
	end
end

local fieldHandler = function( event )
	if ( "submitted" == event.phase ) then
	native.setKeyboardFocus( nil )
	end
end

display.setDefault( "anchorX", 0 )
display.setDefault( "anchorY", 0 )
local inputField = native.newTextField( 10, 250, 300, 38 )
inputField.font = native.newFont( native.systemFont, 16 )

inputField.text = "Hyogo Prefecture, Akashi, the National Astronomical Observatory" -- example of searchable location
inputField:setTextColor( 45/255, 45/255, 45/255 )

```

## 4.参考
冒頭でも紹介していますが、
Corona起動時の右メニューのSamplesにサンプルがあります。
また Corona Labs\Corona SDK\Sample Code\Interface\MapView にもサンプルがあります。
