# ちょっとそこのあなた！ 自分だけのオリジナルLGTMをチームで使ってみませんか？

GitHubなどでPullRequestでコードレビューした後、LGTMとコメントする文化があります。  
おもしろい画像なんかが貼ってあるとほっこりしますね！

<img width="1076" alt="スクリーンショット 2019-05-16 17.15.20.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a53dd37f-a605-b7c3-3405-d29f0ba21105.png">

# LGTMとは？

`「Looks good to me」`の略で、[「良いと思う」「問題ないと思う」](http://d.hatena.ne.jp/keyword/LGTM)という意味。

# 自分だけのLGTM

LGTM関連のツールやサービスは世の中に結構あります

- [LGTM.in](https://www.lgtm.in/)
- [LTTM](https://chrome.google.com/webstore/detail/lttm/jdidcgkdggndpodjbipodfefnpgjooeh?hl=ja)
- [LGTM画像を検索してコピペできるツールを作った[GIPHY] | ひま缶](https://himakan.net/tool/lgtm_search_copy)

どれもすごく良いのですが私が求めているものとなんか違う……。  
アニメのgif画像をLGTMに使いたいんだ！！

Google画像検索で検索するのはめんどくさい……ということで自分だけのオリジナルのLGTMが作成できるツールを開発しました。

![sample_create_LGTM.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/01d29371-0890-3785-88a9-33bc75021500.gif)

ブックマークレットを実行することで、LGTM用の文言をクリップボードにコピーしてくれます。

# 仕組み

LGTM用の文言をブックマークレットとGASを使用し自動生成してくれるツールになります。  
Googleスプレッドシートに設定した画像からランダムでLGTM用の文言を自動生成しクリップボードにコピーします。 実行すると右下にクリップボードにコピーされた画像を表示してくれます。  
他のツールとの違いは`自分のお気に入りの画像を使用してLGTMが作成`できるようになることです！！

画像自体はGoogleスプレッドシートで管理

<img width="1433" alt="スクリーンショット 2019-05-16 17.30.30.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/bfa64239-cbd6-ba21-3559-bd39c510abbb.png">

クリップボードにコピーされる文言は下記のような感じになります。

```
# LGTM

![ポプテピピック（俺だ俺だ俺だ）](https://media2.giphy.com/media/EtFNtaj8sQKuA/giphy.gif?cid=e1bb72ff5c9057fd6276716c552fafa2)
```

# 使用方法

## GoogleスプレッドシートにLGTMで使用する画像の設定

新規でGoogleスプレッドシートを作成します。
シート名は`LGTM`にしてください。
GoogleスプレッドシートにLGTMで使用したい画像のURLを設定します。

下記のような感じで設定します。  
image列は別に設定しなくても良いです。必要なのはID〜description列までです。

| ID  |  URL             | description       | image       |
|:----|:----------------:|:-----------------:|:-----------:|
| 1   |  LGTM画像１のURL |  LGTM画像１の説明 | =IMAGE(B2)  |
| 2   |  LGTM画像２のURL |  LGTM画像２の説明 | =IMAGE(B3)  |
| 3   |  LGTM画像３のURL |  LGTM画像３の説明 | =IMAGE(B4)  |
| 4   |  LGTM画像４のURL |  LGTM画像４の説明 | =IMAGE(B5)  |

## Googleスプレッドシートの内容をJSONで取得できるようにする

Googleスプレッドシートからスクリプトエディタを開きます。


<img width="885" alt="sample_script_editor.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a517a95c-f1ab-4e08-8fc9-c31dded8daf9.png">

スクリプトエディタに下記のソースをコピーし貼り付け保存します。

[https://github.com/dodonki1223/CreateLGTM/blob/master/LGTM.gs](https://github.com/dodonki1223/CreateLGTM/blob/master/LGTM.gs)

<img width="1177" alt="スクリーンショット 2019-05-16 17.41.10.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/41cb8270-d539-1c7a-3f32-7d6df65f151f.png">

Webアプリケーションとして公開する

<img width="624" alt="sample_release.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a34c5958-df27-415c-9cfb-e64db7a4dc54.png">

`現在のウェブアプリケーションのURL`に表示されているURLをコピーして下さい。  
更新ボタンを押すことでWebアプリケーションとして公開されます。

<img width="457" alt="sample_release_setting.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/b0d4fcc9-7d78-9527-a639-1a36b4020eec.png">

`現在のウェブアプリケーションのURL`をブラウザで叩くと下記のようにJSONを取得できていれば大丈夫です。

<img width="783" alt="sample_get_json.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/9b1b82bf-7dde-89e8-6e91-3caef97f1d45.png">

## 自動生成用のブックマークレットを作成する

[https://github.com/dodonki1223/CreateLGTM/blob/master/bookmarklet.js](https://github.com/dodonki1223/CreateLGTM/blob/master/bookmarklet.js)のソースをコピーし[２行目](https://github.com/dodonki1223/CreateLGTM/blob/2a6c9c8718ab620ac78bc34884947dbe92b7bf62/bookmarklet.js#L2)のURLの部分を先程コピーした`現在のウェブアプリケーションのURL`に書き換えたのちブックマークレットとして保存すれば設定完了です。

```JavaScript
javascript:(function(){
  const GAS_API_URL = 'https://script.google.com/macros/s/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/exec';

  let script = document.createElement('script');
  script.src = GAS_API_URL + '?callback=copyLgtm';
  document.body.appendChild(script);
  document.body.removeChild(script);

  window.copyLgtm = function(data) {
    let json = JSON.stringify(data);
    let jsonParse = JSON.parse(json);

    execCopy(jsonParse.data.lgtm);
    displayCopyImg(jsonParse.data.lgtm_url, jsonParse.data.description);
  };

  window.execCopy = string => {
    let copyElement = document.createElement('div');
    copyElement.style.cssText = 'position: fixed; right: 200%;';

    let pre = document.createElement('pre');
    pre.style.cssText = '-webkit-user-select: auto; user-select: auto;';

    copyElement.appendChild(pre).textContent = string;

    document.body.appendChild(copyElement);
    document.getSelection().selectAllChildren(copyElement);
    document.execCommand('copy');

    document.body.removeChild(copyElement);
  };

  window.displayCopyImg = (lgtmImgUrl, description) => {
    let displayElement = document.createElement('div');
    displayElement.style.cssText = 'position: fixed; bottom: 1%; right: 1%; z-index: 9999;';

    let p = document.createElement('p');
    p.textContent = description;
    p.style.cssText = 'position: absolute; top: 0; left: 0.5em; margin: 0; color :white; font-weight: bold;';
    displayElement.appendChild(p);

    let img = document.createElement('img');
    img.src = lgtmImgUrl;
    img.style.width = (window.parent.screen.width * 0.2) + 'px';
    displayElement.appendChild(img);

    document.body.appendChild(displayElement);
    setTimeout(() => document.body.removeChild(displayElement), 3000);
  };
})();
```

## 特定の画像を指定する

URLパラメータにシート名やIDを指定することで特定の画像を指定してLGTMの文言を作成することができます。  

[https://github.com/dodonki1223/CreateLGTM/blob/bc64d09d7997d17117878362dfea4409336432d8/bookmarklet.js#L5](https://github.com/dodonki1223/CreateLGTM/blob/bc64d09d7997d17117878362dfea4409336432d8/bookmarklet.js#L5)の箇所にURLパラメータを追加することで特定の画像を指定することができます。

修正前

```JavaScript
  script.src = GAS_API_URL + '?callback=copyLgtm';
```

修正後

```JavaScript
  script.src = GAS_API_URL + '?callback=copyLgtm&sheet=interesting&id=2';
```

この場合ですと、`interesting`シートの`id`が２の画像でLGTMの文言を作成してくれます。
sheetの指定が無いと`LGTM`のシートが選択されます。また`id`の指定が無いとランダムに選択される仕様になっています。

# GASについて

## URLパラメータの取得

GASではURLパラメータはものすごく簡単に取得できます。

[Web Apps  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/guides/web) こちらのドキュメントに書かれています。

下記のURLパラメータの場合は

```
https://script.google.com/macros/s/xxxxxxxxxx/exec?callback=copyLgtm&sheet=gorira&id=kirin
```

callbackを取得

```JavaScript
e.parameter.callback
=> copyLgtm
```

sheetを取得

```JavaScript
e.parameter.sheet
=> gorira
```

idを取得

```JavaScript
e.parameter.id
=> kirin
```

本当に簡単ですね！

## ランダム値の取得

ランダム値はMath.random()を使用し取得しています。  
Googleスプレッドシートのヘッダー行を除く（２行目〜最終行まで）の間のランダム値を取得するようにしています。  
最終行は`Sheet`クラスを使用することで簡単に取得できます。  

[Class Sheet  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getlastrow) より

```JavaScript
var ss = SpreadsheetApp.getActiveSpreadsheet();
var sheet = ss.getSheets()[0];

// This logs the value in the very last cell of this sheet
var lastRow = sheet.getLastRow();
var lastColumn = sheet.getLastColumn();
var lastCell = sheet.getRange(lastRow, lastColumn);
Logger.log(lastCell.getValue());
```

[Math.random() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/random#Getting_a_random_integer_between_two_values_inclusive) を参考にメソッドを作成しました。最終行はメソッドの呼び出し側で取得するようにしています。

```JavaScript
/**
 * ランダム値を作成する
 * １行目はヘッダーのため、２行目から最終行までのランダムの値を作成し返す
 * @param {Integer} [lastRow] - 最終行
 * @return {Integer} ２行目から最終行までのランダム値
 */
function createRandomValue(lastRow) {
  // 1行目はヘッダー行のため2行目から開始する
  var min = 2,
      max = lastRow;
  
  // randomの値を作成
  var randomValue = Math.floor(Math.random() * (max + 1 - min)) + min;
  Logger.log(randomValue);
  
  return randomValue;
}
```

## JSONP

ブックマークレットからJSONで取得しようとすると`Cross-Origin Read Blocking (CORB) blocked cross-origin response`のエラーがconsoleに吐かれてデータを取得できないのでJSONPを使用しています。

エラーの内容についてはこちらの記事がすごく参考になりました。  

[Cross-Origin Read Blocking (CORB) とは - ASnoKaze blog](https://asnokaze.hatenablog.com/entry/2018/04/10/205717)

> JSONP (JSON with padding) とは、scriptタグを使用してクロスドメインな（異なるドメインに存在する）データを取得する仕組みのことである。 

[Wikipediaより](https://ja.wikipedia.org/wiki/JSONP)

使用するには注意が必要です。

> JSONPでは、クロスサイトリクエストフォージェリ（英: cross-site request forgery、CSRF）に対する脆弱性に注意が必要である。 このscriptタグを使う方法では同一生成元ポリシーが適用されず、またサーバのエンドポイントは外部に公開されているため、悪意のあるサイトがJSONデータを取得するといったことが可能であることから、機密情報や個人情報などのデータを取り扱うには不向きである。 また、scriptタグを埋め込む側においても、リモートサイトから任意の内容のデータをページに差し込むことが可能であるため、そのリモートサイトが悪意のあるサイトである場合やJavaScriptインジェクションに対する脆弱性がある場合は、その脆弱性を突かれることで、アカウント情報を盗まれたり、元のサイトも影響を受けたりする可能性がある。

[Wikipediaより](https://ja.wikipedia.org/wiki/JSONP)

今回はブックマークレットで使用するだけで取得するデータも機密情報では無いので気にしないことにします。

# ブックマークレットについて

## JSONデータを取得する

下記の記事を參考にJSONを取得する仕組みを作成しました。

[面倒な手続き不要！「Web API」の超お手軽活用術をJavaScriptコード付きで一挙大公開！ - paiza開発日誌](https://paiza.hatenablog.com/entry/2016/06/21/%E9%9D%A2%E5%80%92%E3%81%AA%E6%89%8B%E7%B6%9A%E3%81%8D%E4%B8%8D%E8%A6%81%EF%BC%81%E3%80%8CWeb_API%E3%80%8D%E3%81%AE%E8%B6%85%E3%81%8A%E6%89%8B%E8%BB%BD%E6%B4%BB%E7%94%A8%E8%A1%93%E3%82%92JavaScript)

```JavaScript
  const GAS_API_URL = 'https://script.google.com/macros/s/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/exec';

  let script = document.createElement('script');
  script.src = GAS_API_URL + '?callback=copyLgtm';
  document.body.appendChild(script);
  document.body.removeChild(script);

  window.copyLgtm = function(data) {
    let json = JSON.stringify(data);
    let jsonParse = JSON.parse(json);

    execCopy(jsonParse.data.lgtm);
    displayCopyImg(jsonParse.data.lgtm_url, jsonParse.data.description);
  };
```

## クリップボードにコピーする

下記の記事を参考にクリップボードにコピーする仕組みを作成しました。

[JavaScriptでクリップボードに文字をコピーする(ブラウザ) - Qiita](https://qiita.com/simiraaaa/items/2e7478d72f365aa48356)

下記の手順でクリップボードに文字をコピーしています

- 画面外にpreタグを作成しコピーする文字列をセットする  
- 選択してexecCommand('copy')を実行してクリップボードにコピーする  
- 作成したpreタグを削除

```JavaScript
  window.execCopy = string => {
    let copyElement = document.createElement('div');
    copyElement.style.cssText = 'position: fixed; right: 200%;';

    let pre = document.createElement('pre');
    pre.style.cssText = '-webkit-user-select: auto; user-select: auto;';

    copyElement.appendChild(pre).textContent = string;

    document.body.appendChild(copyElement);
    document.getSelection().selectAllChildren(copyElement);
    document.execCommand('copy');

    document.body.removeChild(copyElement);
  };
```

## コピーされたLGTM用の画像を画面に表示する

下記の手順で画面にLGTM用の画像を表示しています

- imgタグを作成する
- imgタグにGoogleスプレッドシートから取得したimgのURLをセット
- imgタグを画面に表示し３秒後に削除する

```JavaScript
  window.displayCopyImg = (lgtmImgUrl, description) => {
    let displayElement = document.createElement('div');
    displayElement.style.cssText = 'position: fixed; bottom: 1%; right: 1%; z-index: 9999;';

    let p = document.createElement('p');
    p.textContent = description;
    p.style.cssText = 'position: absolute; top: 0; left: 0.5em; margin: 0; color :white; font-weight: bold;';
    displayElement.appendChild(p);

    let img = document.createElement('img');
    img.src = lgtmImgUrl;
    img.style.width = (window.parent.screen.width * 0.2) + 'px';
    displayElement.appendChild(img);

    document.body.appendChild(displayElement);
    setTimeout(() => document.body.removeChild(displayElement), 3000);
  };
```

# 最後に

これで自分の好きな画像でLGTMができるようになりました。
素敵なLGTMライフを！！

GitHubではブックマークレットを実行できないので、別のサイトでブックマークレットを実行して下さい:disappointed_relieved:
