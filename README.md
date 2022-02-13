#### 開発メモ
ワークフロー
<br><img width="600" src="https://user-images.githubusercontent.com/40127279/126855661-db2256c7-bfd3-4c8c-bc3d-3bb1d03bb9df.png">

### 1.検索結果のURLを解析する
　とりあえずジョルダンの乗換案内で色々試して解析　
<br>　　&eki1=出発駅（駅の文字は不要、％エンコードなし）
<br>　　&eki2=到着駅（駅の文字は不要、％エンコードなし）
<br>　　&eki3=経由駅（駅の文字は不要、％エンコードなし）
<br>　　&Dym=年月
<br>　　&Ddd=日
<br>　　&Dhh=時
<br>　　&Dmn=分
<br>　　&Cway=0が出発時間、1が到着時間
<br>　　
<br>　Alfredの入力が複雑にならないようにパラメータを考えます
<br>　出発駅、到着駅、到着時間（省略可）、当日検索のみでいいかな
<br>　そうすると、たぶんこんなURLをつくればいけそう
```
https://www.jorudan.co.jp/norikae/cgi/nori.cgi?&eki1=出発駅&eki2=到着駅&S=検索&Dhh=時間&Dmn=分&Cway=1
```
### 2.入力パラメータを分割する
　alfredの入力として3つの要素を空白で区切って入力してもらうようにします
<br>　3つの要素は、出発駅、到着駅、到着時間（省略可）で順番固定です
<br>
<br>　Alfredのワークフローユーティリティのsplit arg to valsが使えそうです
<br>　駅名（全角）と到着時間（半角）の入力となるので、区切り文字としては
<br>　全角空白と半角空白の2つを使いたかったのですが、その設定方法がわかりません
<br>　そこでスクリプトで実装することにしました
```
arr=(`echo $query |sed  's/　/ /g'`) 
```
<br>　sedで全角空白を半角空白に置き換えてから配列に代入してみました
<br>　括弧で括ると区切り文字で分割して配列に代入することができます
<br>　つまり、arr[1],arr[2],arr[3]にそれぞれ格納します
<br>　※ちなみにAlfredのsplit arg to valsを使うと、配列ではなく
<br>　split1,split2,split3というような変数になります
### 3.省略可能パラメータが入力されているか確認する
　3番目のパラメータの有無をチェックして到着時刻をつけるかどうか
<br>　コントロールします。こんなif文です
```
if [ ${arr[3]} ]; then
```
<br>　あとはOpen URLの受け渡してアクセしします
<br>　Httpsから始まる全体のURLをechoしていますので、後続フローのOpenURLの
<br>　URL欄には{query}だけを記載します
<br>　
<br>　RunScript
<br>　<img width="600"  src="https://user-images.githubusercontent.com/40127279/126855688-435aa5e2-9c6c-45fc-9c64-ae966a517161.png">

#### 背景
　今回は複数パラメータに挑戦してみました
<br>　split arg to valsを目論んでいたのですが、敢えなく失敗
<br>　スクリプトで対応しました
<br>　路線検索はナビタイム、駅探、ヤフーなどURLが露出しているので
<br>　どれでもよかったのですがジョルダンで実装しました
<br>　なおgoogleは埋め込み型のwebアプリで、『〇〇駅から××駅』という
<br>　入力だけでOKなのでAlfredのワークフローを作るまでもないです。。。
#### 取扱説明
### 機能:
　ジョルダン乗り換え案内を検索する
### インストール:
　1.[alfredworkflow](https://github.com/KitanoTamotsu/norikae/releases/download/1.1/norikae.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
### 使い方:
　Alfredへキーワード『電車』＋パタメータ（出発駅、到着駅、到着時間）
<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

