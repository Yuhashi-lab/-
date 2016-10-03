![yuhashi-lab](http://www.yuhashi-lab.net/_/rsrc/1436515256182/config/customLogo.gif?revision=1)

2016/10/04

スマートフォンアプリのプログラミング⑤

静岡大学情報学部遊橋研究室(yuhashi-lab.net)


##1．プログラミング
画面遷移を利用したアプリケーションをプログラミングします。
	
##○環境設定
　アプリ起動時に出てくる研究室のロゴの表示の方法
 
___main.lua:___
```lua
--スプラッシュ画像の設定（研究室のロゴを表示）？
local logo = display.newImage("Splash.png", 10, 20)
logo.x = display.contentCenterX 
logo.y = display.contentCenterY + 50


--スプラッシュ画像の削除（背景と研究室のロゴの削除）
local function closeSplash()
   
    display.remove(logo)
    logo = nil
    display.remove(background)
    background = nil
end

--2秒後にscene1.luaのシーンに画面遷移
timer.performWithDelay(2000, closeSplash)

```
*Splash.pngは研究室のロゴの画像です。
*closeSplash()で最初の研究室のロゴの画像と白い背景を消します
*timer.performWithDelay(2000, closeSplash)は2秒後にcloseSplashが呼び出されるという意味。

##画面遷移（基礎編）
```lua
local composer = require("composer")
local scene = composer.newScene()

--storyboardのcreateSceneに相当
--シーンのビューが存在しない場合に呼ばれる。
function scene:create(event)
	local sceneGroup = self.view
end

function scene:show(event)
	local sceneGroup = self.view
	local phase = event.phase

	--storyboardのwillEnterSceneに相当
	--シーンがアクティブになる前に呼ばれる。
	--遷移エフェクトがある場合はそれの前。
	if(phase == "will") then

	--storyboardのenterSceneに相当
	--シーンがアクティブになった時に呼ばれる。
	--遷移エフェクトがある場合はそれの後。
	elseif(phase == "did") then

	end
end

function scene:hide(event)
	local sceneGroup = self.view
	local phase = event.phase

	--storyboardのexitSceneに相当
	--シーンが非アクティブになる前に呼ばれる。
	--遷移エフェクトがある場合はそれの前。
	if(phase == "will") then

	--storyboardのdidExitSceneに相当
	--シーンが非アクティブになった時に呼ばれる。
	--遷移エフェクトがある場合はそれの後。
	elseif(phase == "did") then

	end
end

--storyboardのdestroySceneに相当
--シーンのビューが削除される前に呼ばれる。
function scene:destroy(event)
	local sceneGroup = self.view
end

scene:addEventListener("create", scene)
scene:addEventListener("show", scene)
scene:addEventListener("hide", scene)
scene:addEventListener("destroy", scene)

return scene
```	

##○画面遷移（ボタン編）
```lua
--composerという画面遷移ライブラリを使用する
local composer = require( "composer" )
--新しいシーンを作成する
local scene = composer.newScene()

--scene1.luaのシーンに画面遷移する
composer.gotoScene( "scene1", "fade", 2000  )
```
*fadeは画面遷移する際のエフェクトです。（fadeは現在の画面がフェードアウトし、その後新しい画面がフェードインするというエフェクト）
*2000は画面遷移に要する時間

```lua
local widget = require "widget"

local Gallery	--「ギャラリー」ボタンを格納する変数
local function onGalleryRelease()	--「ギャラリー」ボタンが押された際に呼び出されるコールバック関数
	composer.gotoScene( "scene2", "fromBottom", 500 )	--scene2.luaのシーンに画面遷移する
	return true
end

--シーンが作られた際に呼び出される関数
function scene:create( event )	
	local sceneGroup = self.view

	--ギャラリーボタンの作成
	Gallery = widget.newButton{
		label = "ギャラリー",
		labelColor = { default={255}, over={128} },
		defaultFile = "btn.png",
		overFile = "btnover.png",
		width=70, height=35,
	
		emboss = true,
		
		onRelease = onGalleryRelease	-- event listener function
	}
	Gallery.x = display.contentWidth*0.5 - 50
	Gallery.y = display.contentHeight - 70
	
	-- シーンにGalleryというオブジェクトを追加
	sceneGroup:insert( Gallery )
end

--シーンが表示されたときに呼び出される関数
function scene:show( event )
	local sceneGroup = self.view
	local phase = event.phase
	
	if phase == "will" then
		-- Called when the scene is still off screen and is about to move on screen

	elseif phase == "did" then
	
	end	
end
--シーンの表示が終わった際に呼び出される関数
function scene:hide( event )
	local sceneGroup = self.view
	local phase = event.phase
	
	if event.phase == "will" then
	
	elseif phase == "did" then
		-- Called when the scene is now off screen
	end
end

--シーンが破棄される際に呼び出される関数
function scene:destroy( event )
	local sceneGroup = self.view
	
	if Gallery then
		Gallery:removeSelf()	-- widgets must be manually removed
		Gallery = nil
	end

end

---------------------------------------------------------------------------------

-- リスナーとしてそれぞれのイベントをシーンに登録
scene:addEventListener( "create", scene )
scene:addEventListener( "show", scene )
scene:addEventListener( "hide", scene )
scene:addEventListener( "destroy", scene )

return scene
```


##○画面遷移（タッチ編）
```lua
local image3	--「image3」という写真にタッチ機能を格納する変数
 
local function onImage3Touch( self, event )	-- 「image3」という写真がタッチされた際に呼び出されるコールバック関数
	if event.phase == "ended" or event.phase == "cancelled" then	--タッチをしたかどうかの条件文
		composer.gotoScene( "text", "slideLeft", 800 )	--text.luaのシーンに画面遷移する 
		
		return true	
	end
end

--シーンが作られた際に呼び出される関数
function scene:create( event )	
	local sceneGroup = self.view
	
	-- 「image3」という写真の表示
	image3 = display.newImageRect("image3.jpg",200,150)
	image3.x = display.contentWidth*0.5 
	image3.y = display.contentHeight*0.5 
	-- シーンにimage3というオブジェクトを追加
	sceneGroup:insert( image3 )

--シーンが表示されたときに呼び出される関数
function scene:show( event )
	local sceneGroup = self.view
	local phase = event.phase
	
	if phase == "will" then	
	
	elseif phase == "did" then
		--「ギャラリー」という画像にタッチイベントを追加
		image3.touch = onImage3Touch
		image3:addEventListener( "touch", image3 )
	end	
end
--シーンの表示が終わった際に呼び出される関数
function scene:hide( event )
	local sceneGroup = self.view
	local phase = event.phase
	
	if event.phase == "will" then
		-- 「ギャラリー」という画像からタッチイベントを削除
		image3:removeEventListener( "touch", image3 )
	elseif phase == "did" then
		-- Called when the scene is now off screen
	end
end
--シーンが破棄される際に呼び出される関数
function scene:destroy( event )
	local sceneGroup = self.view
	
end

-- リスナーとしてそれぞれのイベントをシーンに登録
scene:addEventListener( "create", scene )
scene:addEventListener( "show", scene )
scene:addEventListener( "hide", scene )
scene:addEventListener( "destroy", scene )

return scene	
```

##3．参考ソースコード
___main.lua:___
```lua
--composerという画面遷移ライブラリを使用する
local composer = require( "composer" )
--新しいシーンを作成する
local scene = composer.newScene()

--scene1.luaのシーンに画面遷移する
composer.gotoScene( "scene1", "fade", 2000  )

--シーンが表示されたときに呼び出される関数
function scene:show( event )
	local sceneGroup = self.view
	local phase = event.phase
	
	if phase == "will" then
	
	elseif phase == "did" then
	
	--「ギャラリー」という画像にタッチイベントを追加
		image3.touch = onImage3Touch
		image3:addEventListener( "touch", image3 )
	end	
end
--シーンの表示が終わった際に呼び出される関数
function scene:hide( event )
	local sceneGroup = self.view
	local phase = event.phase
	
	if event.phase == "will" then
	
	-- 「ギャラリー」という画像からタッチイベントを削除
		image3:removeEventListener( "touch", image3 )
	elseif phase == "did" then
		-- Called when the scene is now off screen
	end
end
--シーンが破棄される際に呼び出される関数
function scene:destroy( event )
	local sceneGroup = self.view
end
--背景を設定
local background = display.newRect(0,0, display.contentWidth, display.contentHeight)
background:setFillColor( 1, 1, 1 )
background.x = display.contentCenterX
background.y = display.contentCenterY


--研究室のロゴを表示
local logo = display.newImage("Splash.png", 10, 20)
logo.x = display.contentCenterX 
logo.y = display.contentCenterY + 50


--背景と研究室のロゴの削除
local function closeSplash()
   
    display.remove(logo)
    logo = nil
    display.remove(background)
    background = nil
end

--2秒後にscene1.luaのシーンに画面遷移
timer.performWithDelay(2000, closeSplash)
```

以上
