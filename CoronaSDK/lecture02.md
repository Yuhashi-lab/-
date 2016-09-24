![yuhashi-lab](http://www.yuhashi-lab.net/_/rsrc/1436515256182/config/customLogo.gif?revision=1)

2016/09/28

スマートフォンアプリのプログラミング㈪

静岡大学情報学部遊橋研究室(yuhashi-lab.net)


## 1.アプリ画面の縦長/横長回転

新規プロジェクトを作成する際に、[Default Orientation]を[Sideways]に設定すると、デフォルト状態が横長画面のアプリ開発環境になります。
また、シミュレーターは、メニュー画面で[Hardware → Rotate Right]を選択すると画面が時計回りに90度回転します。

## 2.プログラミング

GitHubから手に入れたスクロール機能のソース`scroll.lua`を活用します。コーディングは、`main.lua`に対しておこないます。

`scroll.lua`と素材（画像・効果音）は[こちら](https://sites.google.com/site/yuhashilab2/files)からどうぞ（「アプリ開発㈪」のファイルの中にあります）

## 2.1.環境設定


### 他ファイルの読み込み
___main.lua:___
```lua
local scroll = require("scroll")

```

* `scroll.lua`の内の__function__が利用可能となります

### 効果音の読み込み
___main.lua:___
```lua
local SE_coin = audio.loadSound("Coin.mp3")

```

### 変数の宣言
___main.lua:___
```lua
local _W = display.contentWidth
local _H = display.contentHeight

```

* __display.contentWidth__ は画面の横幅を取得するコマンド
* __display.contentHeight__ は画面の高さを取得するコマンド

### 背景の描写
___main.lua:___
```lua
-- オブジェクトの宣言
local Back

--背景の表示
Back =  scroll.newBackGround({"LoopBack"},{dir = "right",speed = 2})
Back:show()

```

* __scroll.lua__の__function__である__scroll.newBackGround__と__show__を呼び出して利用
* __scroll.newBackGround__では、__LoopBack.png__を表示し、右方向からスピード2でスクロール

### 各種オブジェクトの表示
___main.lua:___
```lua
local Right =display.newImage("Button_R.png",_W-40,_H-30)  -- 右ボタン
local Jump = display.newImage("Button_J.png",_40,_H-30)    -- 左ボタン
local myBall = display.newImage("ball.png",30,_H*7/10+18)  -- ボール（マイキャラ）
local coin = display.newImage("coin.png",_W+100,_H/2)      -- コイン

```

* それぞれ右ボタン、ジャンプボタン、ボール（マイキャラ）、コインを表示するコマンド

## 2.2.背景スクロール

### 変数の宣言
___main.lua:___
```lua
-- 変数の宣言
local m = 0
local s = 0

```

* __m__は__move__を判定する変数
* __s__は__stop__を判定する変数

### 背景スクロールの開始
___main.lua:___
```lua
local function moveBack()
	m = m + 1
	if(m == 1)then
		Back:start()
		s = 0
	end
end

```

* __start__は__scroll.lua__のfunction
* __m__が1のときにスクロールの開始を呼び出し、変数sを0にする

### 背景スクロールの停止
___main.lua:___
```lua
-- 背景スクロールの停止
local function stopBack()
	s = s + 1
	if(s == 1)then
		Back:stop()
		m = 0
	end
end

```

* __stop__は__scroll.lua__のfunction
* __s__が1のときにスクロールの停止を呼び出し、変数mを0にする


## 2.3.ボール（マイキャラ）の移動

### ボール（マイキャラ）を右に動かすfunction
___main.lua:___
```lua
local function moveRight()
	if(myBall.x<_W/2)then            -- ボールのX座標が画面の中央より左のとき
		myBall.x = myBall.x + 5  -- ボールのX座標に5を足して動かす
	end

	if(myBall.x>_W/3)then            -- ボールの位置が画面の3分の1より右のとき
		moveBack()               -- 背景をスクロールさせる__function__の呼び出し
		coin.x = coin.x - 5      -- コインのX座標を5引く
	end
end

```

### 右ボタンを押したときのfunction
___main.lua:___
```lua
local function onRight(event)                                     -- タッチイベントを受けてのfunction
	local target = event.target
	local phase = event.phase

	if(phase == "began")then                                  -- 画面へのタッチ
		display.getCurrentStage():setFocus(target)        -- タッチが始まったら、ボタン絵お表すオブジェクトにフォーカスを設定
		target.isFocus = true
		Runtime:addEventListener("enterFrame",moveRight)  -- 画面の再描画ごとにmoveRightのfunctionを呼び出し

	elseif(target.isFocus)then
		if(phase == "ended" or phase == "cancelled")then  -- 画面の再描画ごとにmoveRightを呼び出すのを停止
			Runtime:removeEventListener("enterFrame",moveRight)
			display.getCurrentStage():setFocus(nil)
			target.isFocus = false

			if(myBall.x>_W/3)then                     -- キャラクターの位置が画面の3分の1より右のとき
				stopBack()                        -- 背景スクロールを停止させるfunctionの呼び出し
			end
		end
	end
	return true
end

```

### ジャンプボタンを押したときのfunction
___main.lua:___
```lua
local function onJump(event)
	if(event.phase == "began")then
		transition.from(myBall,{time = 100,y = _H/2})--,alpha = 0})      -- 指定した座標から、指定した時間をかけて移動
		transition.to(myBall,{time = 100,y = _H*7/10+18})--,alpha = 1})  -- 指定した座標から、指定氏や時間をかけて移動
		local l,h,lenght_W,lenght_H                                      -- on Jump内でのみ有効な変数を宣言

		l = coin.x - myBall.x                                            -- コインとボールの座標を比較
		lenght_W = math.abs(l)                                           -- 座標の差の絶対値をとる

		if(lenght_W < 30)then                                            -- 座標の差が30未満のとき
			audio.play(SE_coin)                                      -- 効果音を鳴らす

			coin.x = _W+100                                          -- コインを獲得すると画面の外に消える
		end
	end
end

Jump:addEventListener("touch",onJump)

```

## 3.参考ソースコード
___main.lua:___
```lua
-- Your code here

local scroll = require("scroll")

local SE_coin = audio.loadSound("Coin.mp3")

local _W = display.contentWidth
local _H = display.contentHeight

local m = 0
local s = 0

local Back
	Back =  scroll.newBackGround({"LoopBack"},{dir = "right",speed = 2})
Back:show()

local Right =display.newImage("Button_R.png",_W-40,_H-30)
local Jump = display.newImage("Button_J.png",_40,_H-30)
local myBall = display.newImage("ball.png",30,_H*7/10+18)
local coin = display.newImage("coin.png",_W+100,_H/2)

local function moveBack()
	m = m + 1
	if(m == 1)then
		Back:start()
		s = 0
	end
end

local function stopBack()
	s = s + 1
	if(s == 1)then
		Back:stop()
		m = 0
	end
end

local function moveRight()
	if(myBall.x<_W/2)then
		myBall.x = myBall.x + 5
	end

	if(myBall.x>_W/3)then
		moveBack()
		coin.x = coin.x - 5
	end
end

local function onRight(event)
	local target = event.target
	local phase = event.phase

	if(phase == "began")then
		display.getCurrentStage():setFocus(target)
		target.isFocus = true
		Runtime:addEventListener("enterFrame",moveRight)

	elseif(target.isFocus)then
		if(phase == "ended" or phase == "cancelled")then
			Runtime:removeEventListener("enterFrame",moveRight)
			display.getCurrentStage():setFocus(nil)
			target.isFocus = false

			if(myBall.x>_W/3)then
				stopBack()
			end
		end
	end
	return true
end

Right:addEventListener("touch",onRight)

local function onJump(event)
	if(event.phase == "began")then
		transition.from(myBall,{time = 100,y = _H/2})--,alpha = 0})
		transition.to(myBall,{time = 100,y = _H*7/10+18})--,alpha = 1})
		local l,h,lenght_W,lenght_H

		l = coin.x - myBall.x
		lenght_W = math.abs(l)

		if(lenght_W < 30)then
			audio.play(SE_coin)

			coin.x = _W+100
		end
	end
end

Jump:addEventListener("touch",onJump)

## 4.課題
ゲームを発展したものを自分で考えて作ってみよう
