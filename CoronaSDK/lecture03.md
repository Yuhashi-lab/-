![yuhashi-lab](http://www.yuhashi-lab.net/_/rsrc/1436515256182/config/customLogo.gif?revision=1)

2016/09/28

スマートフォンアプリのプログラミング③

静岡大学情報学部遊橋研究室(yuhashi-lab.net)


##1．プログラミング
物理演算と加速度センサーを利用したアプリケーションをプログラミングします。
	
##○環境設定
```lua
local SE_coin = audio.loadSound( "Coin.mp3" ) ←音声ファイルの読み込み

local _W = display.contentWidth ←画面横幅を取得
local _H = display.contentHeight ←画面高さを取得

local back = display.newRect(_W/2, _H/2, _W, _H) ←背景の描写

local walls = {
	display.newRect(3, _H/2, 6, _H),
	display.newRect(_W/2, 3, _W, 6),
	display.newRect(_W -3, _H/2, 6, _H),
	display.newRect(_W/2, _H -3, _W, 6)
}
↑画面外周に壁を設置
```


##○オブジェクトの表示
```lua
local myBall = display.newImage("ball.png", 30, 30) ←ボール（マイキャラ）の表示
local coin =  display.newImage("coin.png", math.random(20, _W-20), math.random(20, _H-20)) ←コインの表示
```
①ランダム関数を使ってコインの出現位置を変更

##○物理演算
```lua
local physics = require("physics") ←物理演算を有効化
physics.start()
physics.addBody( myBall, {density = 2, friction = 0.2, bounce = 0.5, radius = 20}) 
↑ボールへの物理属性付与
physics.addBody( coin, "kinematic", {isSensor = true}) ←コインへの物理属性付与

for i=1, 4, 1 do
	walls[i]:setFillColor(0.5, 0.3, 0.1)
	physics.addBody( walls[i], "static", {} )
end
↑画面の外周壁への着色と物理属性付与
```

①物理属性とはその位置に物がある状態
②[kinematic]は、オブジェクトが重力の影響を受けて落下しないようにその場に止めるもの

##○衝突検出
ボールとコインが重なったら、物理演算を停止してコインを非表示にする。
```lua
local function onCollision(event) ←functionの宣言
	if(event.phase == "began")then ←ボールとコインが重なった状況
		audio.play( SE_coin ) ←音を鳴らす
		physics.pause() ←物理演算の停止
		coin.isVisible = false ←コインの消去
		Runtime:addEventListener("touch", reset) ←タッチされたらリセット
	end
end

coin:addEventListener("collision", onCollision) ←コインへのfunction割り当て
```

##○リセット
```lua
local function reset(event) ←functionの宣言
	if("ended" == event.phase)then
		coin.x = math.random(20, _W-20); coin.y = math.random(20, _H-20)
		coin.isVisible = true

		Runtime:removeEventListener("touch", reset)
		
		coin:toFront()
		physics.start()
	end
end
```

※衝突検出の前に記述する。

##○加速度のfanction
```lua
local function onAccelerometer( event )
	physics.setGravity( 9.8 *event.xGravity, -9.8 *event.yGravity )
↑端末の傾きに合わせて重力の方向を変化
End

Runtime:addEventListener( "accelerometer", onAccelerometer )
```

##2．スマホへの実装

##2．１．アプリのビルド
　Corona Simulatorを立ち上げ、[File → Build → Android]を選択します。もし、Java Deveropment Kit（JDK）が存在しないと警告されたら、先にJDKをインストールします（ガイダンスに従ってインストール）。

 

##2．2．アプリの実機転送
Android端末へアプリ転送する最も簡単な方法は、メール添付での送信です。メールで添付されてきたアプリをインストールするためには、Android端末のセキュリティ設定で提供元不明アプリケーションのインストールを許可しておく必要があります。

##3．参考ソースコード
___main.lua:___
```lua
local physics = require("physics")
physics.start()

local SE_coin = audio.loadSound( "Coin.mp3" )

local _W = display.contentWidth
local _H = display.contentHeight

local back = display.newRect(_W/2, _H/2, _W, _H)

local walls = {
	display.newRect(3, _H/2, 6, _H),
	display.newRect(_W/2, 3, _W, 6),
	display.newRect(_W -3, _H/2, 6, _H),
	display.newRect(_W/2, _H -3, _W, 6)
}

for i=1, 4, 1 do
	walls[i]:setFillColor(0.5, 0.3, 0.1)
	physics.addBody( walls[i], "static", {} )
end

local myBall = display.newImage("ball.png", 30, 30)
physics.addBody( myBall, {density = 2, friction = 0.2, bounce = 0.5, radius = 20})

local coin =  display.newImage("coin.png", math.random(20, _W-20), math.random(20, _H-20))
physics.addBody( coin, "kinematic", {isSensor = true})

local function reset(event)
	if("ended" == event.phase)then
		coin.x = math.random(20, _W-20); coin.y = math.random(20, _H-20)
		coin.isVisible = true

		Runtime:removeEventListener("touch", reset)
		
--		rect[1].x = math.random(100, _W-100); rect[1].y = math.random(100, _H-100)
		coin:toFront()
		physics.start()
	end
end

local function onCollision(event)
	if(event.phase == "began")then
		audio.play( SE_coin )
		physics.pause()
		coin.isVisible = false
		Runtime:addEventListener("touch", reset)
	end
end

coin:addEventListener("collision", onCollision)

local function onAccelerometer( event )
	physics.setGravity( 9.8 *event.xGravity, -9.8 *event.yGravity )
end

Runtime:addEventListener( "accelerometer", onAccelerometer )
```

以上
