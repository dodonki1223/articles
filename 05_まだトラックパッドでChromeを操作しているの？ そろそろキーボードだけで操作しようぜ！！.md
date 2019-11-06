# まだトラックパッドでChromeを操作しているの？ そろそろキーボードだけで操作しようぜ！！ Chromeを使い倒そう！！

ちょっとタイトルで煽ってみました（笑）
さて本題です。

ブックマークを開く時、検索する時、タブの切り替え時、ページ内のリンクをクリックする時、ページ内の入力項目を選択する時……トラックパッドで操作していませんか？  

この記事を読んでいるあなたに私からの提案です。  

エンジニアならそろそろキーボードだけで操作してみませんか？
だってキーボードだけで操作したほうがかっこよくないですか（笑）

キーボードで操作する前に……そもそもChromeをちゃんと使いこなせていると言えますか？
Chromeの機能を極限まで使用してChromeを使いこなそうっていうのがこの記事の目的です。  
あまり使われていないであろう機能も紹介したいと思います。

キーボードだけで操作する極意は一番最後に紹介します。

## Chromeのショートカットキーは知っていますか？

これぐらいはショートカットキーで出来るってことを理解しておいた方が良いでしょう

| キー                  |  結果                          | 
|:----------------------|:------------------------------:|
| command + t           |  新しいタブを開く              |
| command + shift + t   |  閉じたタブを開く              |
| command + w           |  タブを閉じる                  |
| command + l           |  アドレスバーへフォーカス      |
| command + r           |  更新                          |
| command + n           |  新規ウィンドウを開く          |
| command + shift + n   |  シークレットウィンドウを開く  |
| command + shift + w   |  ウィンドウを閉じる           |
| option + command + i  |  デベロッパーツールを開く      |
| control + tab         |  右のタブに切り替える          |
| control + shift + tab |  左のタブに切り替える          |

## アドレスバーは使いこなせていますか？

アドレスバーに検索したい文字を入力するとGoogleで検索できますよね？

検索したいサイトっていろいろありますよね？  

Amazonで検索したい時、楽天で検索したい時、何かの言語のリファレンスサイトから検索したい時など検索したいサイトってたくさんありますよね？

検索対象のサイト（検索エンジン）って簡単に切り替えることができることって知ってますか？

#### アドレスバーのカスタマイズについて

アドレスバーを右クリックして**検索エンジンの編集**を選択して下さい
検索エンジンの管理画面へ遷移します

<img width="582" alt="スクリーンショット 2019-01-22 19.47.27.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/2d960d31-4b77-ea75-f086-59af63fc5855.png">

**規定の検索エンジン**というところがアドレスバーに文字を入力して検索されるサイトになります。私の場合ですと、Googleになっています。  
「私はYahooじゃないとダメなんだ」という人はここをYahooにすれば常にYahooで検索されるようになります。  

<img width="679" alt="スクリーンショット 2019-01-22 19.48.40.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/ed7fe79d-18ea-5bff-75fa-e8007c3543a5.png">

**その他の検索エンジン**というところに検索したいサイトの設定をすると簡単に検索対象のサイトを切り替えて検索することが出来ます。

<img width="694" alt="スクリーンショット 2019-01-22 19.49.49.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/44f99165-2d34-4ea4-9ff7-506fa5a969d3.png">

今回は例としてQiitaで検索出来るように設定したいと思います。

#### Qiitaで記事検索出来るようにする

おそらく、Qiitaの検索用の入力BOXから検索したことがある人は下記のような内容がその他の検索エンジンに表示されているでしょう。

<img width="639" alt="スクリーンショット 2019-01-22 19.34.47.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/ee5e1a1d-971d-e38b-dfab-b1d0863d87ba.png">

右側の三点リーダを縦にしたようなものをクリックします。
「編集」をクリックし下記のように設定して保存してみましょう。

<img width="516" alt="スクリーンショット 2019-01-22 19.58.25.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/39bfa812-80c5-1804-b848-270a2bd92c8e.png">

これでQiitaで簡単に検索することが出来るようになりました。

![Jan-25-2019 19-16-51 (1).gif](https://qiita-image-store.s3.amazonaws.com/0/299904/0c3b667b-34ac-82c0-114b-00e76b0fe4f0.gif)

#### Qiitaで記事検索する方法

1. アドレスバーにフォーカスをセット
1. 「q」を入力
1. スペースまたはTabキーを押下
1. 入力したい文字を入力してEnter

以上の操作でQiitaで記事を検索することが出来るようになりました。

先程の設定画面で設定した内容についてです。

キーワードに設定した文字が検索エンジンを切り替えるためのキーワードになります。今回は「q」になります。
検索エンジンとかかれた箇所が「q」を入力してスペースまたはTabキーを入力した時に表示される文字となります。
URLというところには`https://qiita.com/search?q=%s`と設定しました。アドレスバーに入力した文字がURLの`%s`の部分にセットされて遷移されます。
「Javascript」とアドレスバーに入力すると`https://qiita.com/search?q=Javascript`というアドレスに遷移されます。

ということでいろいろと設定することで素早く検索エンジンを切り替えて検索することが出来るようになります。

#### もっとアドレスバーをカスタマイズする

先程のURLに設定するのはURLでなくてもいいんです。

**Javascriptを設定すること**が出来ます。

Javascriptを設定することでどうなるかというと、あるキーワードを入力するとAmazon、楽天、ヤフオクで検索した結果を３つタブを開いて表示するなんてことも出来てしまいます。

私のおすすめの設定をここに紹介します。
下記のものをURLに設定して下さい

**Google翻訳（日本語→英語）**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'https://translate.google.co.jp/?hl=ja#ja/en/' + encodeURIComponent(inputStr);     window.open(url); }())
```

**Google翻訳（英語→日本語）**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'https://translate.google.co.jp/?hl=ja#en/ja/' + encodeURIComponent(inputStr);     window.open(url); }())
```

**Google検索（日本語のページ以外）**

```javascript
javascript:(function() {           var inputStr = decodeURIComponent('%s')              , url = 'https://www.google.co.jp/search?q=' + encodeURIComponent(inputStr) + '&lr=-lang_ja';     window.open(url);       } )();
```

**Google画像検索**

```javascript
javascript:(function() {      var inputStr = decodeURIComponent('%s')       , url = 'https://www.google.co.jp/search?newwindow=1&site=&tbm=isch&source=hp&q=' + encodeURIComponent(inputStr);      window.open(url);      } )();
```

**I'm feeling lucky**
※Googleで検索した１番最初にヒットしたページを開いた状態にしてくれる

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'https://www.google.co.jp/search?btnI&q=' + encodeURIComponent(inputStr);;     window.open(url); }())
```

**WikipediaとGoogleImageを同時に検索**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s') 	  , url1         = 'https://ja.wikipedia.org/wiki/' + inputStr        	  , url2         = 'http://www.google.co.jp/search?newwindow=1&site=&tbm=isch&source=hp&q=' + inputStr    	  , screenWidth  = (window.screen.width - 15) / 2    	  , screenHeight = window.screen.height - 95     	  , winName      = '_blank'    	  , winleft1     = 0;    	 	window.screenX >= window.screen.width ? winleft1 = window.screen.width : winleft1 = 0;       	 	var winOption1 = 'left=' + winleft1 + ',width=' + screenWidth + ',height=' + screenHeight + ',scrollbars=yes,resizeable=yes,menubar=yes,location=no'             	  , winOption2 = 'left=' + (winleft1 + screenWidth + 15) + ',width=' + (screenWidth - 15) + ',height=' + screenHeight + ',scrollbars=yes,resizeable=yes,menubar=yes,location=no';                 	 	window.open(url1, winName, winOption1);   	window.open(url2, winName, winOption2);   }())
```

**IT用語辞典**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'http://e-words.jp/?cx=partner-pub-1175263777233757%3Axelkt7-c6j8&cof=FORID%3A10&ie=Shift_JIS&q=' + encodeURIComponent(inputStr) + '&r=opensearch';     window.open(url); }())
```

**Wikipedia検索**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'https://ja.wikipedia.org/wiki/' + encodeURIComponent(inputStr);     window.open(url); }())
```

**スライド検索**
※SlideShareとSpeakerDeckの両方からスライドを検索する

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url1     = 'http://www.slideshare.net/search/slideshow?searchfrom=header&q=' + encodeURIComponent(inputStr)       , url2     = 'https://www.google.co.jp/search?q=site:https://speakerdeck.com/ ' + encodeURIComponent(inputStr);     window.open(url1);     window.open(url2); }())
```

**Amazon検索**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'https://www.amazon.co.jp/s/ref=nb_sb_noss_1?__mk_ja_JP=カタカナ&url=search-alias%3Daps&field-keywords=' + encodeURIComponent(inputStr);     window.open(url); }())
```

**Youtube検索**

```javascript
javascript:(function() {     var inputStr = decodeURIComponent('%s')       , url      = 'http://www.youtube.com/results?search_query=' + encodeURIComponent(inputStr);     window.open(url); }())
```

その他で知りたい方は[https://github.com/dodonki1223/ChromeSetting/blob/master/OmniBox/OmniBox_GitHub.txt](https://github.com/dodonki1223/ChromeSetting/blob/master/OmniBox/OmniBox_GitHub.txt)より確認してみて下さい

## ブックマークは整理できていますか？

#### ブックマークバーは無駄なく使えていますか？

<img width="215" alt="スクリーンショット 2019-01-23 9.37.59.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/7df8103c-2acd-0f6e-7695-6c6168ed39f5.png">

faviconでわかるブックマークの場合は名称をなくすことでブックマークバーを無駄なく使用することできます。

<img width="514" alt="スクリーンショット 2019-01-23 9.45.21.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/74650e7c-59fe-d346-26f3-b25aa2381b08.png">

<img width="131" alt="スクリーンショット 2019-01-23 9.46.11.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/4e11e74d-2452-aa43-18d8-ed391e43afbe.png">

これでスッキリしました。

#### ブックマークのフォルダ内は整理できていますか？

フォルダ内に仕切り線を入れるとすごくわかりやすくなります

<img width="401" alt="スクリーンショット 2019-01-23 9.47.31.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/cfbbb4c0-e238-567d-658e-51d348000afe.png">

入れ方は適当なURLをブックマークし、名称を仕切りになるようにするだけです。

<img width="513" alt="スクリーンショット 2019-01-23 9.51.23.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/0af52d0a-b225-f522-dca8-ed712c5188a0.png">

## Google検索は使いこなせていますか？

すこしChromeとはズレますが、Googleで検索する時、ただ文字だけを入力して検索していませんか？
検索する時、あるキーワードを追加するだけで自分が思い通りに検索することが出来ることを知っていますか？

#### ハイフン

特定の文字を除外して検索することが出来ます

```
-文字
```

除外キーワードなし

<img width="742" alt="スクリーンショット 2019-01-23 10.06.35.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/bba87886-d695-39ec-d2dd-b8465db2cdf5.png">

水族館が邪魔なので水族館を除外します

<img width="742" alt="スクリーンショット 2019-01-23 10.08.10.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/889b24ba-fae4-6661-39e4-d34e018700a7.png">

#### "文字"（ダブルクォーテーションで囲む）

特定の文字が必ず入ったページを検索する

```
"文字"
```

ライオンという文字があるページだけを検索する

<img width="742" alt="スクリーンショット 2019-01-23 10.10.29.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/9c84a55c-1d2b-b56c-6e18-fa5572dd8c99.png">

#### 特定の日付内で検索する

ツールの期間指定なしってところをクリックすると期間を指定することができます

<img width="767" alt="スクリーンショット 2019-01-23 10.13.06.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/f0a9bbef-68ac-9d4a-d547-a4cf226bc4d1.png">

１年以内を指定したので、2017年のページが表示されなくなりました。

<img width="731" alt="スクリーンショット 2019-01-23 10.14.53.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/88e24e01-4a93-f64b-640d-00ad509dbec4.png">

下記のような形にすれば期間を好きなように指定して検索することが出来ます

```
https://www.google.co.jp/search?q=文字&source=lnt&tbs=cdr:1,cd_min:2018/01/01,cd_max:2019/12/31
```

#### site:サイトのURL

特定のサイト内のページから検索する

```
site:対象のサイトのURL
```

今回は[サルでもわかるGit入門](https://backlog.com/ja/git-tutorial/)というサイトを例でやってみます。
logというキーワードをサルでもわかるGit入門というサイトから検索してみます。

<img width="761" alt="スクリーンショット 2019-01-23 10.23.47.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/861427d8-3813-06b4-98e0-ca27dce332f4.png">

これを応用するとあるサイト内で使用されている画像だけを検索することができます。
下記のURLを実行すると、サルでもわかるGit入門で使用されている画像を検索することが出来ます。

```
https://www.google.co.jp/search?newwindow=1&site=&tbm=isch&source=hp&q=site:https://backlog.com/ja/git-tutorial/
```

<img width="1419" alt="スクリーンショット 2019-01-23 10.31.20.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/e29b14ae-25aa-7579-9ee1-ea1f37547c59.png">

好きな芸能人のブログなんかで使用すればそのブログ内に載せた画像を一覧で見ることができます（笑）
ぜひネットストーカーをしたい人がいたら使って見てください。

#### 見れなくなってしまったページ諦めていませんか？

ページを開いた時、見れないことってありますよね？
URLの▼ボタンをクリックすると`キャッシュ`というリンクが出てくるのでこれをクリックするとGoogleがキャッシュしているのでそのページを確認することが出来ます。

<img width="786" alt="スクリーンショット 2019-01-25 19.04.18.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/87ef01ef-f73c-7255-c93a-0b1cb1f9b163.png">


## ブックマークレットを活用していますか？

ブックマークレット (Bookmarklet) とは、ユーザーがウェブブラウザのブックマークなどから起動し、なんらかの処理を行う簡易的なプログラムのことである（[Wikipediaより](https://ja.wikipedia.org/wiki/%E3%83%96%E3%83%83%E3%82%AF%E3%83%9E%E3%83%B%E3%82%AF%E3%83%AC%E3%83%83%E3%83%88)）
ブックマークからJavascriptを実行してブラウザを操作したり、好きなURLへ遷移したり出来ます。
ブックマークの利点はただのブックマークなので、拡張機能と違い、Chromeのメモリは全く食いません！！

私が使用している一部のブックマークレットを紹介します。

#### サイト内検索

`Google検索は使いこなせていますか？`のところでも紹介した`site:`ってキーワードを使用する方法です。今、見ているサイト内から検索したい時、わざわざ`site:`って打つのめんどくさいですよね。
下記のブックマークレットを登録すれば、ブックマークをクリックするだけで使用出来てしまいます。

入力BOXが表示されるのでそこに検索したい文字を入力することでで現在表示しているページから検索することが出来ます。

```javascript
javascript:(function(){      var inputStr = window.prompt(decodeURIComponent('%E3%82%B5%E3%82%A4%E3%83%88%E5%86%85%E6%A4%9C%E7%B4%A2'), decodeURIComponent('%E6%A4%9C%E7%B4%A2%E3%81%97%E3%81%9F%E3%81%84%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'));     if(inputStr == decodeURIComponent('%E6%A4%9C%E7%B4%A2%E3%81%97%E3%81%9F%E3%81%84%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84') || inputStr == '' || inputStr == null) return false;      var encodeStr    = encodeURIComponent(inputStr)       , pageLocation = location.href;      window.open('https://www.google.co.jp/search?q=' + encodeStr +' site:'+pageLocation); })();
```

#### Google期間指定検索

特定の期間内に更新されたサイトをGoogle検索します。
検索対象にする期間のFromとToを指定することで特定の期間内のサイトだけに絞ってGoogle検索することが出来ます。

```javascript
javascript:(function(){      var searchStr = window.prompt(decodeURIComponent('%E6%9C%9F%E9%96%93%E6%8C%87%E5%AE%9A%E6%A4%9C%E7%B4%A2'), decodeURIComponent('%E6%A4%9C%E7%B4%A2%E3%81%97%E3%81%9F%E3%81%84%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'));     if(searchStr == decodeURIComponent('%E6%A4%9C%E7%B4%A2%E3%81%97%E3%81%9F%E3%81%84%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84') || searchStr == '' || searchStr == null) return false;      var nowDate = getCurrentTime();     var startKikan = window.prompt(decodeURIComponent('%E6%9C%9F%E9%96%93%E6%8C%87%E5%AE%9A%E6%A4%9C%E7%B4%A2\n%E3%80%80%E2%80%BByyyy/mm/dd%E5%BD%A2%E5%BC%8F%E3%81%A7%E9%96%8B%E5%A7%8B%E6%97%A5%E4%BB%98%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'),nowDate );         if(!startKikan.match(/^\d{2}\/\d{2}\/\d{4}$/)) return false;      var endKikan = window.prompt(decodeURIComponent('%E6%9C%9F%E9%96%93%E6%8C%87%E5%AE%9A%E6%A4%9C%E7%B4%A2\n%E3%80%80%E2%80%BByyyy/mm/dd%E5%BD%A2%E5%BC%8F%E3%81%A7%E7%B5%82%E4%BA%86%E6%97%A5%E4%BB%98%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'),nowDate );         if(!endKikan.match(/^\d{2}\/\d{2}\/\d{4}$/)) return false;      window.open('https://www.google.co.jp/search?q=' + searchStr + '&source=lnt&tbs=cdr:1,cd_min:' + startKikan + ',cd_max:' + endKikan);      function getCurrentTime() {         var now = new Date();         var res = '' + padZero(now.getMonth() + 1) + '/' + padZero(now.getDate()) + '/' + now.getFullYear();         return res;     }      function padZero(num) {         var result;         if (num < 10) {             result = "0" + num;         } else {             result = "" + num;         }         return result;     }  })();
```

#### Yahoo路線情報検索

Yahoo路線情報検索から現在時刻から直近の３本、終電の３本を表示してくれるブックマークレットです。
乗車駅、降車駅、現在時刻を入力することで実行されます。

```javascript
javascript:(function(){      var departure = window.prompt(decodeURIComponent('Yahoo%E8%B7%AF%E7%B7%9A%E6%83%85%E5%A0%B1%E6%A4%9C%E7%B4%A2'), decodeURIComponent('%E5%87%BA%E7%99%BA%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'));     if(departure == decodeURIComponent('%E5%87%BA%E7%99%BA%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84') || departure == '' || departure == null) return false;      var arrival   = window.prompt(decodeURIComponent('Yahoo%E8%B7%AF%E7%B7%9A%E6%83%85%E5%A0%B1%E6%A4%9C%E7%B4%A2'), decodeURIComponent('%E5%88%B0%E7%9D%80%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'));     if(arrival == decodeURIComponent('%E5%88%B0%E7%9D%80%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84') || arrival == '' || arrival == null) return false;      var nowTime = getCurrentTime();     var rideTime = window.prompt(decodeURIComponent('Yahoo%E8%B7%AF%E7%B7%9A%E6%83%85%E5%A0%B1%E6%A4%9C%E7%B4%A2\n%E3%80%80%E4%B9%97%E8%BB%8A%E6%99%82%E9%96%93%E3%82%92%E3%80%8Cyyyy/mm/dd hh:mm%E3%80%8D%E5%BD%A2%E5%BC%8F%E3%81%A7%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84\n%E3%80%80%E2%80%BB%E6%97%A5%E4%BB%98%E3%83%BB%E6%99%82%E9%96%93%E3%81%AE%E3%83%81%E3%82%A7%E3%83%83%E3%82%AF%E3%81%AF%E3%81%97%E3%81%AA%E3%81%84%E3%81%AE%E3%81%A7%E6%AD%A3%E3%81%97%E3%81%84%E5%BD%A2%E5%BC%8F%E3%81%A7%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84'), nowTime);      var year  = rideTime.substr(0,4)       , month = rideTime.substr(5,2)       , day   = rideTime.substr(8,2)       , hour  = rideTime.substr(11,2)       , time1 = rideTime.substr(14,1)       , time2 = rideTime.substr(15,1);      var screenWidth  = (window.screen.width - 15) / 2       , screenHeight = window.screen.height - 95       , winName      = '_blank'       , winleft1     = 0;      window.screenX >= window.screen.width ? winleft1 = window.screen.width : winleft1 = 0;      var winOption1 = 'left=' + winleft1 + ',width=' + screenWidth + ',height=' + screenHeight + ',scrollbars=yes,resizeable=yes,menubar=yes,location=no'       , winOption2 = 'left=' + (winleft1 + screenWidth + 15) + ',width=' + (screenWidth - 15) + ',height=' + screenHeight + ',scrollbars=yes,resizeable=yes,menubar=yes,location=no';      window.open('http://transit.yahoo.co.jp/search/result?flatlon=&from=' + encodeURIComponent(departure) +                 '&tlatlon=&to=' + encodeURIComponent(arrival) + '&via=&via=&via=&y=' + year + '&m=' + month + '&d=' + day + '&hh=' + hour + '&m2=' + time2 + '&m1=' + time1 +                 '&type=1&ticket=ic&shin=1&ex=1&s=0&expkind=1&ws=2',winName,winOption1);     window.open('http://transit.yahoo.co.jp/search/result?flatlon=&from=' + encodeURIComponent(departure) +                 '&tlatlon=&to=' + encodeURIComponent(arrival) + '&via=&via=&via=&y=' + year + '&m=' + month + '&d=' + day + '&hh=' + hour + '&m2=' + time2 + '&m1=' + time1 +                 '&type=2&ticket=ic&shin=1&ex=1&s=0&expkind=1&ws=2',winName,winOption2);      function getCurrentTime() {         var now = new Date();         var res = "" + now.getFullYear() + "/" + padZero(now.getMonth() + 1) +                   "/" + padZero(now.getDate()) + " " + padZero(now.getHours()) + ":" + padZero(now.getMinutes());         return res;     }      function padZero(num) {         var result;         if (num < 10) {             result = "0" + num;         } else {             result = "" + num;         }         return result;     }  })();
```

#### GoogleMapルート検索

出発地点、到着地点を入力することでGoogleマップでルートを検索された状態を表示します。

```javascript
javascript:(function(){  var departure = window.prompt(decodeURIComponent('GoogleMap%E3%83%AB%E3%83%BC%E3%83%88%E6%A4%9C%E7%B4%A2'), decodeURIComponent('%E5%87%BA%E7%99%BA%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84')); if(departure == decodeURIComponent('%E5%87%BA%E7%99%BA%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84') || departure == '' || departure == null) return false;  var arrival   = window.prompt(decodeURIComponent('GoogleMap%E3%83%AB%E3%83%BC%E3%83%88%E6%A4%9C%E7%B4%A2'), decodeURIComponent('%E5%88%B0%E7%9D%80%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84')); if(arrival == decodeURIComponent('%E5%88%B0%E7%9D%80%E5%9C%B0%E7%82%B9%E3%82%92%E5%85%A5%E5%8A%9B%E3%81%97%E3%81%A6%E4%B8%8B%E3%81%95%E3%81%84') || arrival == '' || arrival == null) return false;  window.open('https://www.google.co.jp/maps/dir/' + departure + '/' + arrival + '/');  })();
```

#### 現在のURLの画像の一覧をGoogle画像検索で表示

このブックマークレットを芸能人のブログなどで実行するとそのブログ内の画像の一覧をGoogle画像検索で表示することが出来ます。

```javascript
javascript:(function() {          var url = 'https://www.google.co.jp/search?newwindow=1&site=&tbm=isch&source=hp&q=site:' + window.location.href;      window.open(url);      } )();
```

#### 現在開いているURLのFaviconを取得する

Favicon画像が欲しい時に使用します。

```javascript
javascript:(function(){  var url     = 'https://www.google.com/s2/favicons?domain=' + location.hostname       , screenWidth  = window.parent.screen.width / 2       , screenHeight = window.parent.screen.height / 2       , winName      = '_blank'       , option       = 'width=' + screenWidth + ',height=' + screenHeight + ',scrollbars=yes,resizeable=yes,menubar=no,location=no';          window.open(url, winName, option);  })();
```

#### 現在開いているURLを開くQRコードを作成する

パソコンで開いているURLをサクッとスマホで見たい時などに使用します。
そんな時あるのかって感じですが……。

```javascript
javascript:(function() {      var url          = 'http://chart.apis.google.com/chart?cht=qr&chs=300x300&chl=' + location.href       , screenWidth  = window.parent.screen.width / 2       , screenHeight = window.parent.screen.height / 2       , winName      = '_blank'       , option       = 'width=' + screenWidth + ',height=' + screenHeight + ',scrollbars=yes,resizeable=yes,menubar=no,location=no';      window.open(url, winName, option);  })();
```

## 拡張機能は使いこなせていますか？

拡張機能をアドレスバーの横にすべて表示していませんか？
表示する必要のない拡張機能は非表示にすることが出来ます。

<img width="380" alt="スクリーンショット 2019-01-25 12.19.00.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/24d8ce88-93bb-4038-8385-139c59c182a7.png">

拡張機能にショートカットキーを設定することが出来るのですが知っていますか？
下記のURLに遷移することで拡張機能のショートカットキー設定ページへ遷移します。

```
chrome://extensions/shortcuts
```

<img width="1440" alt="スクリーンショット 2019-01-25 12.22.41.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/6f4163fe-e7f4-ddfc-ed7a-a45d935f91f0.png">

ショートカットキーを設定することでわざわざアドレスバー横の拡張機能を押す必要がなくなります。

## ショートカットキーを設定すると活躍する拡張機能

先程紹介したショートカットキーを設定することによってものすごく活躍する拡張機能があるので紹介します。

#### Checker Plus for Gmail

[Checker Plus for Gmail](https://chrome.google.com/webstore/detail/checker-plus-for-gmail/oeopbcgkkoapgobdbedcemjljbihmemj?hl=ja)はGmailチェッカーです。
私の場合ですと、下記のようにショートカットキーを設定しています。

| 機能                 |  ショートカットキー | 
|:---------------------|:-------------------:|
| 拡張機能を有効にする |  ctrl + m           |

「ctrl + m」を押すと、
<img width="500" alt="tT9mqUVmjXlutyq1548387309_1548387409.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/18f6784b-0568-dbee-287a-cdd05b8ec8f2.png">

サクッとGmailの受信状況を確認することが出来ます。

#### Evernote Web Clipper

[Evernote Web Clipper](https://chrome.google.com/webstore/detail/evernote-web-clipper/pioclpoplcdbaefihamjohnefbikjilc?hl=ja)は現在表示しているページをEvernoteに簡単に保存出来る拡張機能です。

私の場合ですと、下記のようにショートカットキーを設定しています。

| 機能                 |  ショートカットキー | 
|:---------------------|:-------------------:|
| 拡張機能を有効にする |  ctrl + e           |

<img width="303" alt="スクリーンショット 2019-01-25 12.42.20.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/84c1a04e-8fb5-14db-3dca-9930dc76dbe4.png">

「ctrl + e」を押すだけで簡単にEvernoteに保存することが出来ます。

#### Save to Pocket

[Save to Pocket](https://chrome.google.com/webstore/detail/save-to-pocket/niloccemoadcdkdjlinkgdfekeahmflj?hl=ja)は現在表示しているページをPocketに簡単に保存出来る拡張機能です。

私の場合ですと、下記のようにショートカットキーを設定しています。

| 機能                 |  ショートカットキー | 
|:---------------------|:-------------------:|
| 拡張機能を有効にする |  ctrl + k           |

<img width="334" alt="スクリーンショット 2019-01-25 12.48.16.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/6512d36b-1e28-faf2-46d5-154893c5e640.png">

[Pockeｔ](https://getpocket.com/)とは今、見ているページを保存してあとで読めるツールになります。
ブックマークするほどじゃないけど、業務中に気になるページがあった時、Pocketに登録して帰宅時の電車の中で保存したページを読んだりする時に大活躍します。

## Popup my Bookmarks

[Popup my Bookmarks](https://chrome.google.com/webstore/detail/popup-my-bookmarks/mppflflkbbafeopeoeigkbbdjdbeifni?hl=ja)はブックマークをリスト形式で表示してくれる拡張機能です。またブックマーク内からurlとブックマーク名から検索することが出来ます。
何より一番のおすすめの機能はブックマークレットを実行することができることです。
この拡張機能を使用することにより、キーボードのみの操作で特定のブックマークレットを実行できます。

私の場合ですと、下記のようにショートカットキーを設定しています。

| 機能                 |  ショートカットキー | 
|:---------------------|:-------------------:|
| 拡張機能を有効にする |  ctrl + b           |

<img width="406" alt="スクリーンショット 2019-01-25 12.59.41.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/a2c69865-de13-fe0b-c604-18387230a4d7.png">

## キーボードだけで操作するためにvimiumを極めよう

やっと本題になります。
Chromeをキーボードだけで操作するためには[vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb)という拡張機能を入れる必要があります。

Qiitaでも**vimium**の記事がいくつか上がっています。

- [Chromeをvimライクに使えるようにするvimium](https://qiita.com/satoshi03/items/9fdfcd0e46e095ec68c1)
- [vim使ってるのにvimium使わないなんてもったいない！
](https://qiita.com/okamu_/items/d9ee9cdc1b5005a0cf1c)
- [Vimiumの基本的な使い方
](https://qiita.com/hiratekatayama/items/d202d4a0ad99b23e75ef)

上記の記事よりもう少しだけ細かく説明したいと思います。

#### vimiumの設定を変更する

vimiumをインストールしたら、アドレスバーのvimiumを右クリックしてオプションをクリックします。

<img width="332" alt="スクリーンショット 2019-01-25 15.02.55.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/e8ddf913-a94d-a03f-1b46-9f7e28349d27.png">

オプションをクリックすると下記のような画面が表示されます。

<img width="907" alt="スクリーンショット 2019-01-28 14.22.44.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/33d1fde3-e3d8-e9f6-07f1-888dd779d3a1.png">

「Custom Key mappings」と書かれている部分に自分だけのオリジナルの設定を記述していくことでChromeを好きなようにカスタマイズしていくことが出来ます。

#### vimiumの設定変更方法

vimiumの設定方法はすごく簡単です。
vimiumにあるコマンドに対してショートカットキーを割り振るだけです。
コマンドの説明は後で行います。

下記の形式を設定していくだけでOKです

```
"ダブルクォーテーションを記述するとコメント文になります。
map ショートカットキー vimiumのコマンド
```

ショートカットキーの部分には`a` 、`F(Shift+f)`、`<c-o>（Ctrl+o）`などが入ります
vimiumのコマンドの部分には`scrollDown`、`scrollFullPageDown`、`openCopiedUrlInNewTab`などが入ります

#### vimiumのコマンドの一覧を確認する

<img width="907" alt="スクリーンショット 2019-01-28 14.22.44_.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/8abf42e5-f171-9e52-1da0-e815682b1bb3.png">

`Show available commands.`というリンクをクリックすることでコマンドの一覧を確認することが出来ます。

<img width="852" alt="スクリーンショット 2019-01-25 15.38.48.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/ae93c2cf-04b5-2c81-6948-7cb212818e11.png">

コマンドの横にショートカットキー情報が表示されています。
`Scroll down`というコマンドには`k`というショートカットが設定されています。
カッコ内に書かれているものがコマンド名になります。

#### キーボードのみで扱うための設定

今回は私の設定を紹介します。

#### 設定の前に

vimiumにはデフォルトである程度ショートカットキーが設定されています。
すべての設定を自分の好きなように設定し直したいので`unmapAll`を記述しています。
この一文を一番上に入れることですべてのキー設定を解除出来ます。

`unmapAll`は公式サイトで`Unmaps all bindings. This is useful if you want to completely wipe Vimium's defaults and start from scratch with your own setup.`と書かれています。

```
"全てのキー設定を解除
unmapAll
```

#### フォーカスの設定

ページ内に検索BOXがあるとします。
その時、わざわざトラックパッドを動かして検索BOXにフォーカスを持っていくのはめんどくさいですよね？

下記の設定をすると、`Shift + i`のショートカットでページ内の一番最初の入力BOXにフォーカスが移ります。
５つ目の入力BOXにフォーカスを選択したい時は`5I`つまり`5 shift + i`を入力することで出来ます。

```
"----------- フォーカス関係 -----------
"最初の入力ボックスにフォーカスをセット 
"※２つ目の入力ボックスにフォーカスしたい時は「2I」と入力すること
map I focusInput
```

| 機能                       |  ショートカットキー | 
|:---------------------------|:-------------------:|
| 最初の入力BOXへフォーカス  |  Shift + i          |
| N番目の入力BOXへフォーカス |  n , Shift + i      |

#### スクロール関連の設定

Vimでいう、`h`,`j`,`k`,`l`ですね。
私の場合、Vimっぽいキー設定ではないのですが、右手だけですべてのスクロールができるように設定しています。

```
"----------- スクロール関係 -----------
"スクロールダウン：↓
map k scrollDown

"スクロールアップ：↑
map i scrollUp

"スクロールレフト：←
map j scrollLeft

"スクロールライト：→
map l scrollRight

"ページダウン
map o scrollFullPageDown

"ページアップ
map u scrollFullPageUp

"スクロールをトップへ
map h scrollToTop

"スクロールをボトムへ
map ; scrollToBottom
```

| 機能                       |  ショートカットキー | 
|:---------------------------|:-------------------:|
| スクロールダウン：↓       |  k                  |
| スクロールアップ：↑       |  i                  |
| スクロールレフト：←       |  j                  |
| スクロールライト：→       |  l                  |
| ページダウン：PageDown     |  o                  |
| ページアップ：PageUp       |  u                  |
| スクロールをトップへ：Home |  h                  |
| スクロールをボトムへ：End  |  ;                  |

キー配置は下記のようになります。

![スクロール.png](https://qiita-image-store.s3.amazonaws.com/0/299904/4926818d-f8aa-52a0-a7ff-4efb81358be2.png)

#### URL遷移関連

URLを遷移する関連の設定です。

```
"--------------- URL遷移 --------------
"戻る
map J goBack

"進む
map L goForward

"１つ上の階層のURLへ遷移
map gu goUp

"ルートURLへ遷移
map gU goToRoot
```

| 機能                    |  ショートカットキー | 
|:------------------------|:-------------------:|
| 戻る                    |  Shift + j          |
| 進む                    |  Shift + L          |
| １つ上の階層のURLへ遷移 |  g , u              |
| ルートURLへ遷移         |  g , Shift + u      |

`戻る`、`進む`は下記画像の`←`、`→`と同じ動作になります。

<img width="283" alt="スクリーンショット 2019-01-25 17.00.41.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/9cd369c9-0bcd-f728-8b1a-f39310840929.png">

`１つ上の階層のURLへ遷移`は`http://hogehoge/gorira/kirin`というページを参照している時、`http://hogehoge/gorira`へ遷移することを指します。

`ルートURLへ遷移`は`http://hogehoge/gorira/kirin`というページを参照している時、`http://hogehoge`へ遷移することを指します。

#### Hit a Hint関係

Hit a Hint機能とはページ内のリンクに対して、ショートカットキーを割り振る機能のことを言います。

```
"----------- Hit a Hint関係 -----------
"リンクを開く
map f LinkHints.activateMode

"リンクを新しいタブで開く
map F LinkHints.activateModeToOpenInNewTab

"リンクを連続して新しいタブで開く
map d LinkHints.activateModeWithQueue
```

| 機能                             |  ショートカットキー | 
|:---------------------------------|:-------------------:|
| リンクを開く                     |  f                  |
| リンクを新しいタブで開く         |  Shift + f          |
| リンクを連続して新しいタブで開く |  d                  |

![Jan-25-2019 17-17-41 (1).gif](https://qiita-image-store.s3.amazonaws.com/0/299904/732f5b8d-36fe-3199-a8db-c2df4297f1b6.gif)

gif画像を見てもらうとわかるようにリンクにショートカットが割り振られ、`ゴリラ - Wikipedia`には`AF`というショートカットが割り振られています。
キーボードで`af`と入力すると`ゴリラ - Wikipedia`へ遷移します。

#### ページ内ブックマーク機能

この機能は縦に長いページの時に有効です。
ページ内にブックマークをセットすることが出来るので、ブックマークをセットした箇所に即座に移動することが出来ます。

```
"----- ページ内ブックマークツール -----
"ページにマークをセット ※「m」の押下後、特定のキーでマークをセットする
map m Marks.activateCreateMode

"マークをセットしたところに移動する ※「Shift + m」の押下後、特定のキーを押下する
map M Marks.activateGotoMode
```

| 機能                               |  ショートカットキー       | 
|:-----------------------------------|:-------------------------:|
| ページ内にブックマークをセット     |  m , なんかのキー         |
| マークをセットしたところに移動する |  Shift + m , なんかのキー |

![Jan-25-2019 17-30-14 (1).gif](https://qiita-image-store.s3.amazonaws.com/0/299904/01a2a5e9-686b-7fe8-e04c-8f2d319c50b0.gif)

分類が表示されている箇所の時に`m`を押下後、`a`を押してブックマークします。
一番下まで移動した後、`Shift + m`を押下後`a`を押すことで分類の位置まで戻るようになります。

#### クリップボードツール

クリップボードを使用する機能になります。

```
"----- クリップボードツール -----
"現在のページURLをクリップボードにコピー
map e copyCurrentUrl

"ページ内のリンクのURLをクリップボードにコピー
map E LinkHints.activateModeToCopyLinkUrl

"クリップボードの内容で現在のタブでGoogle検索する
map p openCopiedUrlInCurrentTab

"クリップボードの内容で新しいタブでGoogle検索する
map P openCopiedUrlInNewTab
```

| 機能                                             |  ショートカットキー  | 
|:-------------------------------------------------|:--------------------:|
| 現在のページURLをクリップボードにコピー          |  e                   |
| ページ内のリンクのURLをクリップボードにコピー    |  Shift + e           |
| クリップボードの内容で現在のタブでGoogle検索する |  p                   |
| クリップボードの内容で新しいタブでGoogle検索する |  Shift + p           |

`e`を押すと下記の画像のようにchromeの下の方に`Yanked　https://hoge……`のような表示がされます。
表示されるとクリップボードに現在開いているURLがコピーされています。

<img width="389" alt="スクリーンショット 2019-01-25 17.38.46.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/cb7552da-a497-247c-9f47-753acc0e9f08.png">

`Shift + e`を押すと、Hit a Hint機能のようにページ内のリンクにショートカットが割り振られるのでそのショートカット通りにKeyを押すと、そのリンクのURLをクリップボードにコピー出来ます。

`p`を押すと、クリップボード内に`ゴリラ`という文字があった場合はGoogleで`ゴリラ`を検索しにいきます。
下記のような画像の結果になると思います。

<img width="1437" alt="スクリーンショット 2019-01-25 17.46.56.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/84e9d0e3-e933-7565-b37a-696ef460b53f.png">

#### タブツール

タブを操作する機能になります。

```
"------------- タブツール -------------
"現在のタブをリロード
map r reload

"新しいタブを開く
map t createTab

"次のタブへ移動
map . nextTab

"前のタブへ移動
map , previousTab

"最初のタブへ移動
map $, firstTab

"最後のタブへ移動j
map $. lastTab

"タブを１つ右へ移動
map > moveTabRight

"タブを１つ左へ移動
map < moveTabLeft

"タブの固定ON/OFF
map <c-o> togglePinTab

"タブを閉じる
map x removeTab

"閉じたタブを開く
map X restoreTab

"開いているタブを検索する
map T Vomnibar.activateTabSelection
```

| 機能                     |  ショートカットキー  | 
|:-------------------------|:--------------------:|
| 現在のタブをリロード     | r                    |
| 新しいタブを開く         | t                    |
| 次のタブへ移動           | .                    |
| 前のタブへ移動           | ,(カンマ)            |
| 最初のタブへ移動         | $ , ,(カンマ)        |
| 最後のタブへ移動         | $ , .                |
| タブを１つ右へ移動       | Shift + .            |
| タブを１つ左へ移動       | Shift + ,            |
| タブの固定ON/OFF         | Ctrl + o             |
| タブを閉じる             | x                    |
| 閉じたタブを開く         | Shift + x            |
| 開いているタブを検索する | Shift + t            |

`開いているタブを検索する`機能はよく利用するでしょう。

下記の画像のようにキーボードだけでタブを操作をすることができ、タブの切り替えは容易になります。

![Jan-25-2019 18-01-56 (1).gif](https://qiita-image-store.s3.amazonaws.com/0/299904/a8536e2b-66ae-0d0a-01de-7ab0892683ab.gif)

#### 履歴ツール

履歴の内容を検索することができる機能になります。

```
"------------- 履歴ツール -------------
"履歴を検索して開く
map y Vomnibar.activate

"履歴を検索して新しいタブで開く
map Y Vomnibar.activateInNewTab
```

![ezgif-1-433d4cc8f3fc (1).gif](https://qiita-image-store.s3.amazonaws.com/0/299904/393c2c76-7487-2fcc-1e62-5a446e882ee3.gif)

| 機能                           |  ショートカットキー  | 
|:-------------------------------|:--------------------:|
| 履歴を検索して開く             |  y                   |
| 履歴を検索して新しいタブで開く |  Shift + y           |

#### ページ内検索ツール

ページ内の文字列を検索する機能になります。

```
"--------- ページ内検索ツール ---------
"ページ内検索
map / enterFindMode

"検索にマッチしたものを次のフォーカスへ
map n performFind

"検索にマッチしたものを前のフォーカスへ
map N performBackwardsFind
```

| 機能                                   |  ショートカットキー  | 
|:---------------------------------------|:--------------------:|
| ページ内検索                           |  /                   |
| 検索にマッチしたものを次のフォーカスへ |  n                   |
| 検索にマッチしたものを前のフォーカスへ |  Shift + n           |

`/`を押下後、検索したい文字を入力しEnterを押すことで検索文字が決定されます。
決定後、`n`で次の一致した箇所に移動し、`Shift + n`で１つ前の一致した箇所に移動します。
マッチしているリンクで更にEnterを押すとそのリンクへ遷移します。

![Jan-25-2019 18-27-02 (1).gif](https://qiita-image-store.s3.amazonaws.com/0/299904/299f8cb9-0deb-5e97-2d73-08bce02ec046.gif)

#### ブックマーク検索ツール

ブックマーク内を即座に検索してそのブックマークのURLへ即座に遷移出来る機能になります。
ただしブックマークレットなどのJavascriptは実行出来ません。
なので私は代わりに先程紹介した[Popup my Bookmarks](https://chrome.google.com/webstore/detail/popup-my-bookmarks/mppflflkbbafeopeoeigkbbdjdbeifni?hl=ja)の方を使用しています。

```
"--------- ブックマークツール ---------
"ブックマークを開く
map b Vomnibar.activateBookmarks

"ブックマークを新しいタブで開く
map B Vomnibar.activateBookmarksInNewTab
```

| 機能                           |  ショートカットキー  | 
|:-------------------------------|:--------------------:|
| ブックマークを開く             |  b                   |
| ブックマークを新しいタブで開く |  Shift + b           |

#### ビジュアルモードツール

 キーボードだけでテキスト選択ができる機能になります。

 説明としては下記に書いてある通りです。
 この機能に関しては説明致しません。
 私自身、あまり使っていない機能なので……。
 
 [Visual Mode | Vimium - にほんご。](https://tr.you84815.space/vimium/visualMode.html)に書かれている説明をそのまま書いているだけです。
 
```
"------- ビジュアルモードツール -------
### <ビジュアルモード>
###   ビジュアルモードは、ページ内のテキストを選択するために使用します。
###   vを押すと、ビジュアルモードに入ります。
###   Vを押すと、行選択ビジュアルモードに入ります。
###   ビジュアルモードでは j、k、h、l、w、e、b等、 多くのVimライクな移動操作を使うことができます。 3eのようなカウンタもサポートしています。
### <キャレットモード>
###   キャレットモードは、ページ内のテキストを選択する際の開始点を変更するために使用します。
###   ビジュアルモードからcを押すとキャレットモードに入ります。 (または、ノーマルモードからvの後にcを押します。)
###   ビジュアルモードに入る際に、何も選択されていない場合はキャレットモードに入ります。 キャレットモードでは、vを押してビジュアルモードに入る前に、 
###   適切な位置にキャレット(開始点)を指定することができます。
###   vとcを使って、ビジュアルモードとキャレットモードを行き来することができます。
###   ビジュアルモードの終了
###   <esc> : ビジュアルモードも終了します。
###   y : 選択したテキストをクリップボードにコピーします。

"ビジュアルモード
map v enterVisualMode

"ビジュアルラインモード
map V enterVisualLineMode
```

| 機能                   |  ショートカットキー  | 
|:-----------------------|:--------------------:|
| ビジュアルモード       |  v                   |
| ビジュアルラインモード |  Shift + v           |

#### その他

ヘルプを表示する機能になります。
設定したショートカットキーを忘れた時などはこちらの機能を使用するといいでしょう。

```
"--------------- その他 ---------------
"ヘルプを表示
map ? showHelp
```

| 機能         |  ショートカットキー  | 
|:-------------|:--------------------:|
| ヘルプを表示 |  Shift + /           |

<img width="871" alt="スクリーンショット 2019-01-25 18.57.25.png" src="https://qiita-image-store.s3.amazonaws.com/0/299904/4fa196b9-ce87-7597-d5f0-eecc6a784f76.png">

#### 最終的に設定する内容

[https://github.com/dodonki1223/ChromeSetting/blob/master/Extension/Vimium.txt](https://github.com/dodonki1223/ChromeSetting/blob/master/Extension/Vimium.txt)

```
"全てのキー設定を解除
unmapAll

"----------- フォーカス関係 -----------
"最初の入力ボックスにフォーカスをセット 
"※２つ目の入力ボックスにフォーカスしたい時は「2I」と入力すること
map I focusInput

"----------- スクロール関係 -----------
"スクロールダウン：↓
map k scrollDown

"スクロールアップ：↑
map i scrollUp

"スクロールレフト：←
map j scrollLeft

"スクロールライト：→
map l scrollRight

"ページダウン
map o scrollFullPageDown

"ページアップ
map u scrollFullPageUp

"スクロールをトップへ
map h scrollToTop

"スクロールをボトムへ
map ; scrollToBottom

"--------------- URL遷移 --------------
"戻る
map J goBack

"進む
map L goForward

"１つ上の階層のURLへ遷移
map gu goUp

"ルートURLへ遷移
map gU goToRoot

"----------- Hit a Hint関係 -----------
"リンクを開く
map f LinkHints.activateMode

"リンクを新しいタブで開く
map F LinkHints.activateModeToOpenInNewTab

"リンクを連続して新しいタブで開く
map d LinkHints.activateModeWithQueue

"----- ページ内ブックマークツール -----
"ページにマークをセット ※「m」の押下後、特定のキーでマークをセットする
map m Marks.activateCreateMode

"マークをセットしたところに移動する ※「Shift + m」の押下後、特定のキーを押下する
map M Marks.activateGotoMode

"----- クリップボードツール -----
"現在のページURLをクリップボードにコピー
map e copyCurrentUrl

"ページ内のリンクのURLをクリップボードにコピー
map E LinkHints.activateModeToCopyLinkUrl

"クリップボードの内容で現在のタブでGoogle検索する
map p openCopiedUrlInCurrentTab

"クリップボードの内容で新しいタブでGoogle検索する
map P openCopiedUrlInNewTab

"------------- タブツール -------------
"現在のタブをリロード
map r reload

"新しいタブを開く
map t createTab

"次のタブへ移動
map . nextTab

"前のタブへ移動
map , previousTab

"最初のタブへ移動
map $, firstTab

"最後のタブへ移動
map $. lastTab

"タブを１つ右へ移動
map > moveTabRight

"タブを１つ左へ移動
map < moveTabLeft

"タブの固定ON/OFF
map <c-o> togglePinTab

"タブを閉じる
map x removeTab

"閉じたタブを開く
map X restoreTab

"開いているタブを検索する
map T Vomnibar.activateTabSelection

"------------- 履歴ツール -------------
"履歴を検索して開く
map y Vomnibar.activate

"履歴を検索して新しいタブで開く
map Y Vomnibar.activateInNewTab

"--------- ページ内検索ツール ---------
"ページ内検索
map / enterFindMode

"検索にマッチしたものを次のフォーカスへ
map n performFind

"検索にマッチしたものを前のフォーカスへ
map N performBackwardsFind

"--------- ブックマークツール ---------
"ブックマークを開く
map b Vomnibar.activateBookmarks

"ブックマークを新しいタブで開く
map B Vomnibar.activateBookmarksInNewTab

"------- ビジュアルモードツール -------
### <ビジュアルモード>
###   ビジュアルモードは、ページ内のテキストを選択するために使用します。
###   vを押すと、ビジュアルモードに入ります。
###   Vを押すと、行選択ビジュアルモードに入ります。
###   ビジュアルモードでは j、k、h、l、w、e、b等、 多くのVimライクな移動操作を使うことができます。 3eのようなカウンタもサポートしています。
### <キャレットモード>
###   キャレットモードは、ページ内のテキストを選択する際の開始点を変更するために使用します。
###   ビジュアルモードからcを押すとキャレットモードに入ります。 (または、ノーマルモードからvの後にcを押します。)
###   ビジュアルモードに入る際に、何も選択されていない場合はキャレットモードに入ります。 キャレットモードでは、vを押してビジュアルモードに入る前に、 
###   適切な位置にキャレット(開始点)を指定することができます。
###   vとcを使って、ビジュアルモードとキャレットモードを行き来することができます。
###   ビジュアルモードの終了
###   <esc> : ビジュアルモードも終了します。
###   y : 選択したテキストをクリップボードにコピーします。

"ビジュアルモード
map v enterVisualMode

"ビジュアルラインモード
map V enterVisualLineMode

"--------------- その他 ---------------
"ヘルプを表示
map ? showHelp
```

## まとめ

`command+l`でアドレスバーへフォーカスをあて、検索エンジンを切り替えて検索したり、ブックマークレットを使用したりして効率よく検索していく。
情報収集として、`ctrl+k`（Pocket）、`ctrl+e`（Evernote）などを使用し気になるWebページは保存していく。
普段の操作は基本的にvimiumを使用しキーボードのみでChromeを操作していく。
以上のことをやっていくだけでキーボードだけでChromeをほぼ操作することが出来るようになるでしょう！

## 最後に

いろいろと書いてきましたが如何がだったでしょうか？
そんなの知っているよという人、煽ってすいませんでした。

以上、ありがとうございました。
