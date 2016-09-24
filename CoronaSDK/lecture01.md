![yuhashi-lab](http://www.yuhashi-lab.net/_/rsrc/1436515256182/config/customLogo.gif?revision=1)

2016/09/04

静岡大学遊橋研究室新規生のためのCoronaSDK解説資料です。

静岡大学情報学部遊橋研究室(yuhashi-lab.net)

## 1.Corona SDKとは

![CoronaSDK](http://tr1.cbsistatic.com/hub/i/2015/05/07/3eaf92ea-f49e-11e4-940f-14feb5cc3d2a/coronasdk_logo-300x991.png)

>Corona SDK is a cross-platform framework that empowers developers to create 2D games and apps for mobile, TV, and desktop. It’s designed to enable super-fast development – up to 10x faster than other platforms. Adding sophisticated features is easy using our elegant Lua-based APIs. A streamlined workflow lets you see your changes instantly. Monetization is built in with Corona Ads. And you can publish to all major platforms from a single code base.

[公式HP](https://coronalabs.com/corona-sdk/)より(https://coronalabs.com/corona-sdk/)

1つのコードを書くだけでデスクトップやiOS,Androidなどの様々なプラットフォームで動きます。

## 2.Install
CoronaSDKの[ダウンロードページ](https://coronalabs.com/pricing/)からダウンロードします。(https://coronalabs.com/pricing/)

2016年9月4日時点ではFREE,$79,$199の3つがありますが、モバイルアプリの開発だけならばFREEで十分です。

会員登録後、DLしてインストールしてください。

### 起動画面
![image](https://raw.githubusercontent.com/taigak/-/master/CoronaSDK/Lecture01/StartingImage.png)

## 3.エディタ

CoronaSDKにはテキストエディタが統合されていません。そこで、別途エディタを用意する必要があります。

元から自分が愛用しているものがあればそれを利用してもらっても構いません。  
愛用しているエディタが無いようであれば、とりあえず統一して秀丸エディタを利用します。

### 秀丸エディタ
<img src="http://blog-imgs-37-origin.fc2.com/w/e/i/weiss2003/5126IVuAziL.jpg" alt="秀丸エディタ" title="秀丸" width="100" height="130">

>秀丸エディタは、Windows95,98,Me以降/WindowsNT4.0,2000,XP,Vista,7以降で動作するテキストエディタです。
　テキストエディタとは、テキストファイルを編集するためのソフトです。  
　Windows標準では、メモ帳で編集したファイルを、秀丸エディタで編集することができます。
　プログラムのソースファイル編集や、メール編集、HTML編集など、様々な「テキスト」を使うシーンに、高機能かつ軽快に使っていただけます。

インストールは[こちら](http://hide.maruo.co.jp/software/hidemaru.html)から(http://hide.maruo.co.jp/software/hidemaru.html)

学生ユーザはライセンスフリーで利用できます。
インストール後は`*.lua`と`*.setting`の拡張子をエディタに関連付けてください。

## 4.文字などの表示

それではプログラミングに入っていきます。

アプリケーションでは、アプリケーションのファイル一式を__プロジェクト__と呼びます。  
CoronaSDKでもプロジェクト単位で開発を進めていきます。

プロジェクト選択画面の「+」ボタンを押し、新規プロジェクトを生成します。  
プロジェクト名を付け(適当で大丈夫ですが、ここではLecture01としておきます。)、ファイル格納フォルダを選択します。  
これからの演習では、基本的にProject Templateは「__Blank__」、Default orientationは「__Upright__」を選んで下さい。

プロジェクトフォルダには、`main.lua`、`config.lua`、`build.setting`に加え、いくらかのIcon用の画像が入っています。演習では`main.lua`にプログラムを書き込んでいきます。

### コメントの挿入

```lua
-- コメント
```
* --でコメント開始
* 改行でコメント終了

### テキストの表示
___main.lua:___
```lua
-- Your code here

-- オブジェクトの宣言
local title

-- タイトルの表示
title = display.newText("Lecture01", 100,400, nil, 30)

```

* __local title:__ オブジェクトの宣言  
　localの部分はこのファイル内でのみ有効なオブジェクトを意味する
* __display.newText:__ テキスト表示のコマンド  
　表示する文字(日本語可)、X座標、Y座標、フォント指定、文字サイズ
* 座標は、画面左上が原点となり、右に向かってX軸、下に向かってY軸となる
* __nil__ はフォントのデフォルト指示
* 文字サイズで数字が大きいほど文字が大きくなる

### オブジェクトの着色
___main.lua:___
```lua
-- Your code here

-- オブジェクトの宣言
local title

-- タイトルの表示
title = display.newText("Lecture01", 100,400, nil, 30)
title:setFillColor(0.2,0.7,0.7)
```
* __title__ が操作対象オブジェクト
* __:setFillColor__ は塗りつぶしコマンド、引数でRGBを指定
* 引数は彩度を意味し、0が発色無し、1が最も強い発色

### 画像ファイルの表示
___main.lua:___
```lua
-- Your code here

-- オブジェクトの宣言
local square

-- 画像の表示
square = display.newImage("Icon-60.png",50,50)
```
* __local square:__ オブジェクトの宣言  
　__local__ の部分はこのファイル内でのみ有効なオブジェクトを意味する
* __display.newImage:__ 画面表示のコマンド

### 画面サイズの取得
___main.lua:___
```lua
-- Your code here

-- 変数の宣言
local _W = display.contentWidth
local _H = display.contentHeight
```
* 宣言部分の_Wと_Hは変数(オブジェクトとは呼ばない)
* __display.contentWidth__ は画面の横幅を取得するコマンド
* __display.contentHeight__ は画面の高さを取得するコマンド

### 取得した座標への表示
___main.lua:___
```lua
-- Your code here

-- 変数の宣言
local _W = display.contentWidth
local _H = display.contentHeight

-- オブジェクトの宣言
local square

-- 画像の表示
square = display.newImage("Icon-60.png", _W/2, _H/4)
```

### 背景の作成
___main.lua:___
```lua
-- Your code here

-- 変数の宣言
local _W = display.contentWidth
local _H = display.contentHeight

-- オブジェクトの宣言
local back

-- 背景の表示
back = display.newRect(_W/2, _H/2, _W, _H)
```
* __display.newRect__ は四角形の描写コマンド
* 前半の引数は描写四角形の中心、後半の引数は描写の大きさ
* 画像や背景の前後関係は__上書き__が基本


## 5.イベント
表示の次はイベントの登録をします。
表示した文字や画像をクリックした際の動きを定義します。

### オブジェクトのドラッグ
___main.lua:___
```lua
--- Your code here

-- オブジェクトの宣言
local icon

-- iconの表示
icon = display.newImage("Icon-60.png", 50, 50)

-- eventの作成と登録
local function onObj(event)           -- 機能の宣言
  if(event.phase == "began")then      -- タッチを開始した状況
  elseif(event.phase == "moved")then  -- ドラッグ中の状態
    icon.x = event.x                  -- タッチポイントとオブジェクトの座標を一致させる
    icon.y = event.y
  elseif(event.phase == "ended")then  -- ドラッグ終了の状態
  end
end
icon:addEventListener("touch", onObj) -- イベントのオブジェクトへの登録

```
* __function__ はオブジェクトや変数ではなく、機能
* __onObj__ は機能名
* ==は同じ値かどうかの比較(代入は=)
* __event__ は機能の状態、__event.phase__ はタッチイベントの状態
* __event.x__ と __event.y__ は、タッチしている座標
* __:addEventListener__ は、オブジェクトとイベントの関連付けコマンド
* __touch__ はタッチイベントを意味する(イベントの種類は他にも多数)

### 図形合わせゲーム
図形を(三角の図形を丸の図形に)合わせて点数計算をするゲームです。

画像は[こちら](https://github.com/taigak/images/tree/master/CoronaSDK/Lecture01)からどうぞ(https://github.com/taigak/images/tree/master/CoronaSDK/Lecture01)

__出力画面__

![out](https://raw.githubusercontent.com/taigak/images/master/CoronaSDK/Lecture01/Lecture01_game.gif)

___main.lua:___
```lua
-- Your code here

-- 変数の宣言
local _W = display.contentWidth
local _H = display.contentHeight
local score = 0

-- オブジェクトの宣言
local back
local title, numScore
local circle, triangle, resetButton

-- 背景の描画
back      = display.newRect(_W/2, _H/2, _W, _H)
back:setFillColor(0.3, 0.2, 0.5)

-- 文字の描画
title     = display.newText("Lecture01", 100,400, nil, 30)
title:setFillColor(1, 1, 1)

numScore  = display.newText(score, 150, 40, nil, 30)
numScore:setFillColor(1, 1, 1)

-- 図形の描画
circle      = display.newImage("circle.png",      _W/2, _H/4)
triangle    = display.newImage("triangle.png",    _W/2, _H/2)
resetButton = display.newImage("ResetButton.png", _W/2, _H*5/7)

-- functionの作成と登録
local function onObj(event)
  if(event.phase == "moved")then

    -- triangleの位置合わせ
    triangle.x = event.x
    triangle.y = event.y

  elseif(event.phase == "ended")then
    -- 誘導判定&誘導
    local L, H
    W = event.x - (_W/2)  -- circleとtriangleの位置差計算
    H = event.y - (_H/4)
    if((math.abs(W) < 30) and (math.abs(H) <30))then
      transition.to(triangle,{time=500, x=_W/2, y=_H/4})
      addScore()
    end

  end
end
triangle:addEventListener("touch", onObj)

function addScore()
  score = score + 100
  numScore.text = score
end

local function reset(event)
  if(event.phase == "ended")then

    -- Scoreリセット
    score = 0
    numScore.text = score

    -- triangleの位置リセット
    triangle.x = _W/2
    triangle.y = _H/2
  end
end
resetButton:addEventListener("touch", reset)
```

## 6.課題
ゲームを発展したものを自分で考えて作ってみよう（図形の数を増やすなど）

## 7.発展的な部分

発展的学習に関して参考になるものをあげておきます
* 『基礎から学ぶCorona SDK』
* Corona Docs API(https://docs.coronalabs.com/api/)
* SDKと一緒に配布されているサンプル（CoronaSDK起動時の右メニューあるSamplesから参照できます）
