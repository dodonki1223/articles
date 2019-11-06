# 声優名で話しかけると出演するゲームを教えてくれるLINE BOTを作ったったー

![sample_screen.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/750fb569-7034-768b-e907-567dd6d43d84.png)

友達登録してくれると喜びます！！

| <a href="https://line.me/R/ti/p/%40kox6824y"><img height="36" border="0" alt="友だち追加" src="https://scdn.line-apps.com/n/line_add_friends/btn/ja.png"></a> | ![qr_code.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/035207c6-8c20-59c3-9547-538ca283a07b.png) |
|:---:|:---:|

# どんなBOTなの？

声優名で話しかけると出演するゲームを５件まで教えてくれます！！
話しかけ方として以下の３つがあります。

- 声優名
- 先月の声優名
- 来月の声優名

例えば「風音」さんの出演しているゲームを調べたい時はBOTに対して

- 風音
- 先月の風音
- 来月の風音

と話しかけます。

![Screenshot_20190412-093431.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/4963be43-0ab2-f5dd-9978-ace68be461e5.png)

実行タイミングが4月12日(金)なので`風音`の場合は2019年4月に発売するゲームを教えてくれ、同様に`先月の風音`の場合は2019年3月で`来月の風音`の場合は2019年5月の出演するゲームを教えてくれます。  
出演するゲームがない場合は出演する予定はありませんと教えてくれます。

声優名以外にも「リスト」と話しかけるとゲームの発売リストページを教えてくれます。
話しかけ方は声優名と同じで以下の３つがあります。

- リスト
- 先月のリスト
- 来月のリスト

![sample_list_screen.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/1bd581bc-a0b4-3edb-d8e7-babf8bd3c00c.png)  

# 仕組みは？

Ruby（スクレイピングツール）とGASを使用して作成しています。  
ざっくりとした図は下記のとおりです。  

![プログラム詳細.001.jpeg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/32e73a42-212f-beff-a452-a33f83207abe.jpeg)

## スクレイピングツール

[げっちゅ屋](http://www.getchu.com/top.html?gc=gc)の[発売日リスト](http://www.getchu.com/all/price.html?genre=pc_soft&year=2019&month=3&gage=&gall=all)とゲームの紹介ページをスクレイピングしその結果をコマンドラインにて表示、絞り込みできるツールになります。  

![sample.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/cf385d76-545e-7bd9-c8be-3c1c856d70a3.gif)
[https://github.com/dodonki1223/eroge_release_cmd](https://github.com/dodonki1223/eroge_release_cmd)
  

このスクレイピングツールでゲームの発売リスト情報を取得しCSVに出力します。  
出力したCSVの内容を元にGoogleスプレッドシートへ書き込みを行います。

![Googleスプレッドシート](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/1a8f6680-c8fd-98ea-bf7e-245a29cb7131.png)

## GAS

[https://github.com/dodonki1223/eroge_release_bot](https://github.com/dodonki1223/eroge_release_bot)

GASはLINE BOTと連携してLINE BOTからの入力を受けとり、その内容に合致したものをLINE BOTに返す役割をしています。

# 作成理由は？

Rubyの勉強のため、Rubyを使って何か作成しようと思い、作ったのがこのLINE BOTです。最初はRubyによるスクレイピングツールだけのつもりだったのですがどうせならLINE BOTも作ってしまえってことで作成しました（笑）
これを作成する前はRuby初心者です。一度もRubyを書いたことありませんでした。

# スクレイピングツールの開発について

[今回作成したスクレイピングツールのソースはこちら](https://github.com/dodonki1223/eroge_release_cmd)

## 基本的なRuby構文を書く時に参考にしたこと

- [オブジェクト指向スクリプト言語 Ruby リファレンスマニュアル (Ruby 2.6.0)](https://docs.ruby-lang.org/ja/latest/doc/index.html)
- [rubocop-hq/rubocop: A Ruby static code analyzer and formatter, based on the community Ruby style guide.](https://github.com/rubocop-hq/rubocop)
- [Cookpadのコーディング規約](https://github.com/cookpad/styleguide/blob/master/ruby.ja.md)
 
公式サイトこそ正義だと思っているので、必ず公式サイトのリファレンスを読んでプログラムを書いていきました。
公式サイトの内容がよくわからない時はブログやQiitaの記事を参考に開発しました。
`rubocop`というLinterツールのgemを入れ、コードは基本的にコイツに監視してもらい、その他でどう書いたらいいのだろうかというところはCookpadのコーディング規約を参考にしました。

## RSpecを書く時に参考にしたこと

- [Publisher: RSpec - Relish](https://relishapp.com/rspec)
- [Better Specs { rspec guidelines with ruby }](http://www.betterspecs.org/jp/)

今まで働いてきたところではテストコードを書く文化に触れたことがなかったので勉強のために特に力を入れたのがRspecです。
RSpecの独特な記述に苦しまれつつ、書いていました……。慣れてくるといいものですね。
先程でも述べたように公式サイトこそ正義なので必ず公式サイトを読むようにしました。

`Better Specs`というRSpecのベスト・プラクティスを教えてくれるサイトを特に参考にしました。ただこの`Better Specs`は難しいことはあまり書かれていなくて基本的なことが多い印象です。

- [rubocop/spec at master · rubocop-hq/rubocop](https://github.com/rubocop-hq/rubocop/tree/master/spec)

応用的な書き方は`rubocop`内のRSpecを参考にしました。
Mockの作り方やコードの簡略化にすごく参考になりました。

- [使えるRSpec入門・その3「ゼロからわかるモック（mock）を使ったテストの書き方」 - Qiita](https://qiita.com/jnchito/items/640f17e124ab263a54dd)

少し古い記事ですが、Mockの考え方は[@jnchito](https://qiita.com/jnchito)さんの記事を参考にしました。すごくわかりやすくまとめてあり勉強になりました。

## スクレイピング

スクレイピングは[Nokogiri](https://nokogiri.org/)を使用して行いました。スクレイピング関連の記事は探せばいろいろと出てくるのですが、スクレイピングのRSpecコードのサンプルが無くて困りました。
なので今回はスクレイピングのやり方については特に説明しません。興味がある人は[Github](https://github.com/dodonki1223/eroge_release_cmd/tree/master/eroge_release/getchuya_scraping)のソースを見て下さい。

スクレイピングのRSpecコードで重要だと思ったのはサイトの構造が変わり、値が取得できなくなることだと思ったので、テストは最小限に値が取得できるかどうかをテストしているだけです。  
サイト構造が変わったら結局スクレイピングコードを作り直さないといけないと思うので、これぐらいで良いと思いました。

```ruby
    let(:target_year_month) { '201902' }
    let!(:release_list_scraping) { described_class.new(target_year_month) }

    describe '#scraping' do
      let(:release_list) do
        VCR.use_cassette 'release_list' do
          release_list_scraping.scraping
        end
      end
      let(:first_elemet) { release_list[0] }

      it { expect(first_elemet).to include(:release_date, :title, :introduction_page, :id, :brand_name, :price) }

      it { expect(first_elemet[:release_date]).not_to eq '' }
      it { expect(first_elemet[:title]).not_to eq '' }
      it { expect(first_elemet[:introduction_page]).not_to eq '' }
      it { expect(first_elemet[:id]).not_to eq '' }
      it { expect(first_elemet[:brand_name]).not_to eq '' }
      it { expect(first_elemet[:price]).not_to eq '' }
    end
  end
```

[VCR](https://github.com/vcr/vcr)というRSpecのWebmockを簡単に作成できるgemを使用しています。「[VCRを使うとRSpecのWebmockの作成が超絶楽になった！ | 酒と涙とRubyとRailsと](https://morizyun.github.io/blog/webmock-vcr-gem-rails/)」のサイトで、ものすごくわかりやすく説明してくれているので、見てみて下さい。

１回目は時間がかかりますが、２回目以降はVCRのおかげで高速でテストが実行できます。

## コマンドライン引数

VB.NETでコマンドライン引数を制御するプログラムを作成していた時は自作していたのですが、Rubyだと簡単にコマンドライン引数を制御することができるクラスがあったのですごく助かりました。
この辺を自分で実装するとすごくめんどくさい記憶があるので簡単に実装できて驚きました。下記の記事がものすごく參考になりました。

- [class OptionParser (Ruby 2.6.0)](https://docs.ruby-lang.org/ja/latest/class/OptionParser.html)
- [コマンドライン引数によるオプションに対応する (optparse) | まくまくRubyノート](https://maku77.github.io/ruby/io/optparse.html)

```ruby
  # コマンドライン引数クラス
  #   コマンドから受け取ったコマンドライン引数をパースして
  #   プログラムから扱えるようにする機能を提供する
  class CommandLineArg
    attr_accessor :options

    # コンストラクタ
    #   コマンドライン引数を受け取れるキーワードの設定、ヘルプコマンドが実行
    #   された時のメッセージの設定
    #   コマンドライン引数の値を取得するようのHash変数の作成
    def initialize
      # コマンドライン引数の値をセットするHash変数
      @options = {}
      OptionParser.new do |opt|
        # ヘルプコマンドを設定
        opt.on('-h', '--help', 'Show this help') do
          puts opt
          exit
        end

        # げっちゅ屋のrobots.txtの内容を表示するコマンドを設定
        opt.on('--robots', 'Display contents of robots.txt') do
          puts GetchuyaScraping.robots
          exit
        end

        # 値を受け取る系のコマンドライン引数を設定する
        opt.on('-y', '--year_month [YEAR_MONTH]', 'Set Target Year And Month') { |v| set_command_line_arg_value(v, :year_month, '年月') }
        opt.on('-v', '--voice_actor [VOICE_ACTOR]', 'Narrow down by voice actor name') { |v| set_command_line_arg_value(v, :voice_actor, '声優名') }
        opt.on('-t', '--title [TITLE]', 'Filter by title') { |v| set_command_line_arg_value(v, :title, 'タイトル名') }
        opt.on('-b', '--brand_name [BRAND_NAME]', 'Narrow down by brand_name') { |v| set_command_line_arg_value(v, :brand_name, 'ブランド名') }

        # true、falseを受け取るコマンドライン引数を設定する
        # デフォルト値はすべてfalseとし、受け取ったものにはtrueをセットする
        @options[:csv]              = false
        @options[:json]             = false
        @options[:open]             = false
        @options[:spreadsheet]      = false
        @options[:open_spreadsheet] = false
        @options[:clear_cache]      = false
        @options[:simple]           = false
        opt.on('-o', '--open [OPEN]', 'Open game page in browser') { @options[:open] = true }
        opt.on('-c', '--csv [CSV]', 'Create a csv file') { @options[:csv] = true }
        opt.on('-j', '--json [JSON]', 'Create a json file') { @options[:json] = true }
        opt.on('-s', '--spreadsheet [SPREADSHEET]', 'Write to spreadsheet from CSV') { @options[:spreadsheet] = true }
        opt.on('--open_spreadsheet [OPEN_SPREADSHEET]', 'Open spreadsheet page in browser') { @options[:open_spreadsheet] = true }
        opt.on('--clear_cache [CLEAR_CACHE]', 'Clear the cache') { @options[:clear_cache] = true }
        opt.on('--simple [SIMPLE]', 'Display results in a simplified way') { @options[:simple] = true }

        # コマンドラインをparseする
        opt.parse!(ARGV)
      end
    end

    # 対象のコマンドライン引数が存在するか？
    def has?(name)
      @options.include?(name)
    end

    # 対象のコマンドライン引数の値を取得する
    #   対象のコマンドライン引数の値が存在しない場合は空文字を返す
    def get(name)
      return '' unless has?(name)

      @options[name]
    end

    private

    # コマンドライン引数をインスタンス変数にセットする
    #   もし対象のコマンドライン引数がnilの時はメッセージを表示して処理を終了する
    def set_command_line_arg_value(value, key, param_name)
      if value.nil?
        puts "#{param_name}のパラメータを指定して下さい"
        exit
      end
      # 配列かどうかを取得し、配列の時は配列をセットそうでない時は値をそのままセットする
      is_array = value.split(',').count > 1
      set_value = is_array ? value.split(',') : value
      @options[key] = set_value
    end
  end
```

## Googleスプレッドシートの操作

Googleスプレッドシートのシートを削除したり、新規に作成したりするのに[google-drive-ruby](https://github.com/gimite/google-drive-ruby)というgemを使用しました。
使い方についてはドキュメント通りなのですが、ドキュメント以外のことをやろうと思った時、あまりサンプルがなかったのでGithubのソースを見てどんなことができるのか確認しながら作成しました。

```ruby
# コンストラクタ
#   IDを引数で受け取り、もしIDが存在しなかった場合はGoogleスプレッドシートが見つかりません例外が
#   発生する
#   ※必ず存在するGoogleスプレッドシートがあること前提です
def initialize(spreadsheet_id)
  # jsonファイルのconfigファイルからGoogleDrive::Sessionを作成する
  # https://github.com/gimite/google-drive-ruby/blob/master/doc/authorization.mdに
  # google_drive_config.jsonファイル作成方法が書かれています
  @session = GoogleDrive::Session.from_config(CONFIG_FILE_PATH)
  begin
    @spreadsheet = @session.spreadsheet_by_key(spreadsheet_id)
  rescue Google::Apis::ClientError => e
    puts "指定されたIDのスプレッドシートが見つかりませんでした\n#{e.message}(#{e.class})"
    raise
  end
end
```

```ruby
# @spreadsheetは「GoogleDrive::Spreadsheet」です

# Googleスプレッドシートのワークシートをタイトルから削除する
#   ワークシートが見つからなかった場合はメッセージを表示
def delete_worksheet_by_title(title)
  # 削除対象のワークシートを取得
  target_worksheet = @spreadsheet.worksheet_by_title(title)

  # 削除対象のワークシートが見つからなかった場合はメッセージを表示して処理を終了する
  if target_worksheet.nil?
    puts "ワークシート名が「#{title}」のワークシートが見つかりませんでした"
    puts 'ワークシートの削除が出来ませんでした'
    return
  end

  # ワークシートの削除処理を実行
  target_worksheet.delete
end
```

```ruby
# @spreadsheetは「GoogleDrive::Spreadsheet」です

# Googleスプレッドシートのワークシートをタイトルから取得する
#   ワークシートが存在しない時は作成したワークシートを@worksheetにセットする存在する時はそのワー
#   クシートを@worksheetにセットする
def get_worksheet_by_title_not_exist_create(title)
  @worksheet = @spreadsheet.worksheet_by_title(title).nil? ? @spreadsheet.add_worksheet(title) : @spreadsheet.worksheet_by_title(title)
  @worksheet
end
```

```ruby 
@worksheetは「GoogleDrive::Worksheet」です

# GoogleスプレッドシートへCSVファイルから書き込みを行なう
def write_by_csv(file_path)
  begin
    # CSVファイルの読み込み
    csv_data = CSV.read(file_path)
  rescue StandardError => e
    # 例外メッセージとバックトレースを表示して処理を終了する
    puts "CSVファイルが見つかりませんでした\n#{e.message}(#{e.class})"
    raise
  end
  # CSVファイルからデータを取得し、スプレッドシートへ書き込む
  csv_data.each_with_index do |data, row|
    data.each_with_index do |value, cell|
    # google-drive-rubyの仕様で1行目は1から、1セル目は1からなので行とセルにそれぞれ+1する
    @worksheet[row + 1, cell + 1] = value
    end
  end
  @worksheet.save
end
```

[google-drive-ruby](https://github.com/gimite/google-drive-ruby)のコードのRSpecはMockを多用して書きました。
量が多いのでリンクだけ貼っておきます。興味がある人は読んで見て下さい。

- [spreadsheet_helper.rb](https://github.com/dodonki1223/eroge_release_cmd/blob/master/spec/support/spreadsheet_helper.rb)
- [spreadsheet_spec.rb](https://github.com/dodonki1223/eroge_release_cmd/blob/master/spec/eroge_release/spreadsheet/spreadsheet_spec.rb)
- [spreadsheet_writer_spec.rb](https://github.com/dodonki1223/eroge_release_cmd/blob/master/spec/eroge_release/spreadsheet/spreadsheet_writer_spec.rb)

# LINE BOTの開発について

## LINE BOT

LINEの社員さんが書かれているQiitaの記事を參考にLINEの設定を行いました。
Node.jsのプログラム開発以外はこちらを參考にしました。

- [LINEのBot開発 超入門（前編） ゼロから応答ができるまで - Qiita](https://qiita.com/nkjm/items/38808bbc97d6927837cd)

## LINE BOTとGASとの連携

下記記事を參考にしました。

- [Google Apps ScriptでLINE BOTつくったら30分で動かせた件 - Qiita](https://qiita.com/hakshu/items/55c2584cf82718f47464)
- [Google Spreadsheet を簡易 Webサーバーとして動かして、手軽にWebHookを受け取る方法 - Qiita](https://qiita.com/kunichiko/items/7f64c7c80b44b15371a3)

LINE BOTとGASの連携は本当に簡単です。１時間もあれば簡単に連携することができました。

LINE Developersの設定で`Webhook送信`を`利用する`にし忘れることがあります。
`利用する`にしてないとLINEから文字を入力してもうんともすんとも反応しないので必ず`利用する`になっているか確認して下さい。

![LineImage](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/fcb6578a-77ec-bd84-027d-a5089bbaee94.png)

## GASについて

今回のGASの役割はLINEからの入力を受け取り、受け取った内容でスプレッドシート内を検索し合致した内容をLINEに返すことをしています。

[今回作成したGASのソースはこちら](https://github.com/dodonki1223/eroge_release_bot)

## LINEからPOSTされた時に実行される処理

GASをWEBアプリケーションとして公開しPOSTを受け取る時は`doPost(e)`を定義しなくてはいけません。その辺のことは[Web Apps  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/guides/web)に書いてあるので読んでみると良いかもしれないです。
つまりLINE BOTを作るだけなら`doPost(e)`メソッドだけ用意してあげればOKです。
今回作成したBOTの`doPost(e)`メソッドの中身を紹介していきます。

```JavaScript
function doPost(e) {
  // LineBotからPostされたデータを取得
  // Webhookイベントオブジェクトの返す値について
  // →https://developers.line.biz/ja/reference/messaging-api/#webhook-event-objects
  var replyToken  = JSON.parse(e.postData.contents).events[0].replyToken,　 // WebHookで受信した応答用Token
      userMessage = JSON.parse(e.postData.contents).events[0].message.text; // ユーザーのメッセージを取得（声優名）

  // 対象の月を取得する
  var targetMonth = getTargetMonth(userMessage);
  
  // 対象のシートを取得
  var yearMonth = getYearMonth(targetMonth),
      sheet     = getSheet(yearMonth);
  
  // 声優名を取得する
  var voiceActorName = getVoiceActorName(userMessage)
  
  // 声優名から対象のスプレッドシートの行を取得
  var foundRows = getRowsByVoiceActor(sheet, voiceActorName);

  // LineにPostする
  if (voiceActorName == 'リスト') {
    var year  = yearMonth.slice(0,4),
        month = yearMonth.slice(4);
    UrlFetchApp.fetch(config.LinePostUrl, createRequest(replyToken, createListPagePostMessage(year, month)));
  } else {
    if (foundRows.length == 0) {
      UrlFetchApp.fetch(config.LinePostUrl, createRequest(replyToken, createNotExistsPostMessage(targetMonth, voiceActorName)));
    } else {
      UrlFetchApp.fetch(config.LinePostUrl, createRequest(replyToken, createPostMessages(sheet, foundRows)));
    }
  }
  return ContentService.createTextOutput(JSON.stringify({'content': 'post ok'})).setMimeType(ContentService.MimeType.JSON);
}
```

もう少し細かく確認していきましょう。

### LINEからPOSTされた情報を受け取る

LINEからは下記のような感じでPOSTデータが返ってきます。
詳しくは[Messaging APIリファレンス](https://developers.line.biz/ja/reference/messaging-api/#webhook-event-objects)を読んで見て下さい。

```json
{
  "destination": "xxxxxxxxxx", 
  "events": [
    {
      "replyToken": "0f3779fba3b349968c5d07db31eab56f",
      "type": "message",
      "timestamp": 1462629479859,
      "source": {
        "type": "user",
        "userId": "U4af4980629..."
      },
      "message": {
        "id": "325708",
        "type": "text",
        "text": "Hello, world"
      }
    },
    {
      "replyToken": "8cf9239d56244f4197887e939187e19e",
      "type": "follow",
      "timestamp": 1462629479859,
      "source": {
        "type": "user",
        "userId": "U4af4980629..."
      }
    }
  ]
}
```

LINEからの入力情報を取得する

```JavaScript
  // LineBotからPostされたデータを取得
  var replyToken  = JSON.parse(e.postData.contents).events[0].replyToken,　 // WebHookで受信した応答用Token
      userMessage = JSON.parse(e.postData.contents).events[0].message.text; // ユーザーのメッセージを取得（声優名）
```

`replyToken`はイベントへの応答に使用するトークンです。
`userMessage`はLINEから入力された声優名が入ります。

### シートを取得

Googleスプレッドシートには年月の単位でシートができています。
シート名で取得できるようにしています。

![名称未設定.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a5f66a71-136b-1767-eb7f-bdd4b5a89282.png)

シートの取得方法

```JavaScript
  // 対象の月を取得する
  // 先月、来月などの文字から対象月を取得する
  // 2019年4月16日に実行した場合は下記のように判断する
  // 声優花子　　　：今月 → 「 0」で返ってくる
  // 先月の声優花子：先月 → 「-1」で返ってくる
  // 来月の声優花子：来月 → 「+1」で返ってくる
  var targetMonth = getTargetMonth(userMessage);
  
  // 対象のシートを取得
  // 対象の月から年月を取得し、対象のシートを取得する
  // 2019年4月16日に実行した場合は下記のようにシートを判断する
  // 声優花子　　　：201904
  // 先月の声優花子：201903
  // 来月の声優花子：201905
  // yearMonthには「201904」、「201903」、「201905」などの文字列が取得されます
  var yearMonth = getYearMonth(targetMonth),
      sheet     = getSheet(yearMonth);
```

年月の情報を取得するメソッド群

```JavaScript
// 検索する対象月の値定数
var targetMonth = {
  LastMonth          : -1,
  NextMonth          : 1,
  CurrentMonth       : 0,
  LastMonthString    : '先月の',
  NextMonthString    : '来月の',
  CurrentMonthString : '今月の'
}

// 対象の月情報を取得する
// 文字列に「先月の」、「来月の」などの文字列でどの月が対象か判断する
// 「0」か「1」か「-1」を返す
function getTargetMonth(message) {
  if (message.indexOf(targetMonth.LastMonthString) != -1) {
    return targetMonth.LastMonth
  } else if (message.indexOf(targetMonth.NextMonthString) != -1) {
    return targetMonth.NextMonth;
  } else {
    return targetMonth.CurrentMonth;
  }
}

// 年月の文字列を返す
// addMonthの値はgetTargetMonthで取得した値が来る想定です
// YYYYMMDD形式の文字をを返す
// YYYYMMDD形式がシート名となっているため
function getYearMonth(addMonth) {
  var date = new Date();

  // addMonthの内容により◯ヶ月前、◯ヶ月後の情報をセットする
  date.setMonth(date.getMonth() + addMonth);

  // 月は-1で取得されるため、+1する
  var year  = date.getFullYear(), 
      month = date.getMonth() + 1;

  // 月が１桁時は前０を付加して年月の文字列を返す
  return year + ('0' + month).slice(-2);
}
```

シートを取得するメソッド

```JavaScript
// シート名から対象のシートオブジェクトを取得しています
// SheetNameには「201904」、「201903」、「201905」がくる想定です
function getSheet(SheetName) {
  var ss    = SpreadsheetApp.getActiveSpreadsheet(),
      sheet = ss.getSheetByName(SheetName);
  return sheet;
}
```

### 声優名から合致するデータを取得する

声優名から対象の行を抽出する

```JavaScript
  // 声優名を取得する
  var voiceActorName = getVoiceActorName(userMessage)
  
  // 声優名から対象のスプレッドシートの行を取得
  var foundRows = getRowsByVoiceActor(sheet, voiceActorName);
```

声優名を取得するメソッド

```JavaScript
// LINE BOTから受け取った文字列から「先月の」、「来月の」の文字列を排除した
// 文字列を受け取る
// 下記のように感じなります
// 声優花子　　　：声優花子
// 先月の声優花子：声優花子
// 来月の声優花子：声優花子
function getVoiceActorName(message) {
  replaceMessage = message.replace(targetMonth.LastMonthString, '');
  return replaceMessage.replace(targetMonth.NextMonthString, '');
}
```

声優名から対象の行を取得するメソッド

```JavaScript
// 声優名から対象の行を取得するメソッド
function　getRowsByVoiceActor(sheet, voiceActorName) {
  // 声優名列はI列なのでI列を対象に最終行までのRangeオブジェクトを取得します
  var startRow  = 2,
      searchRangeArea = 'I' + startRow + ':I' + sheet.getLastRow(),
      range     = sheet.getRange(searchRangeArea),
      foundRows = [];

  // Rangeのスタートが２行目のため、Rangeの行数＋１する
  var searchMaxRow = range.getNumRows() + 1;
  
  // 声優名列から対象の声優が出演しているか検索し結果を配列にセット
  // １行ずつ確認していき、対象の声優の文字列がある時は出演していると判断する
  // 声優名列には「声優花子、声優ゴリラ、声優キリン」のような文字列にLINE BOTからの文字列が
  // 含まれているか判断しているだけなので結構ゆるい感じで判断しています　
  for(var row = startRow; row <= searchMaxRow; row++) {
    cellValue = sheet.getRange("I" + row).getValue();
    if (cellValue.indexOf(voiceActorName) != -1) 
      foundRows.push(row);
  }

  // APIの制限で５件までしか送信できないため、最初から５件のみを取り出す
  // リクエストボディのところに５件までと書かれています
  // https://developers.line.biz/ja/reference/messaging-api/#anchor-6640e4a392930e46edb1c15c1d6817ee2356f75e
  return foundRows.slice(0, 5);
}
```

### LINEにPOSTする

UrlFetchAppを使用してLINEにPOSTします。
詳しくは[Class UrlFetchApp  |  Apps Script  |  Google Developers](https://developers.google.com/apps-script/reference/url-fetch/url-fetch-app)を確認して下さい。

リストとそうでない時で処理を切り分けています。
さらに入力された声優名の行が存在する時としない時で切り分けています。

```JavaScript
  // config.LinePostUrlにはLINEにPOSTするURLが入っています。
  // 「https://api.line.me/v2/bot/message/reply」になります。

  // LineにPostする
  if (voiceActorName == 'リスト') {
    // 年月の文字列から年と月を切り分ける
    var year  = yearMonth.slice(0,4),
        month = yearMonth.slice(4);
    UrlFetchApp.fetch(config.LinePostUrl, createRequest(replyToken, createListPagePostMessage(year, month)));
  } else {
    if (foundRows.length == 0) {
      UrlFetchApp.fetch(config.LinePostUrl, createRequest(replyToken, createNotExistsPostMessage(targetMonth, voiceActorName)));
    } else {
      UrlFetchApp.fetch(config.LinePostUrl, createRequest(replyToken, createPostMessages(sheet, foundRows)));
    }
  }
```

```JavaScript
// POSTのリクエストを作成している箇所です
// callbackにはLINEのメッセージオブジェクトの配列が入ります
// メッセージオブジェクトについては
// https://developers.line.biz/ja/reference/messaging-api/#message-objects
// を読んで下さい
function createRequest(replyToken, callback) {
  return {
    'headers': {
      'Content-Type': 'application/json; charset=UTF-8',
      'Authorization': 'Bearer ' + config.LineAccessToken,
    },
    'method': 'post',
    'payload': JSON.stringify({
      'replyToken': replyToken,
      'messages': callback,
    }),
  }
}
```

メッセージオブジェクトを作成している箇所です
[メッセージオブジェクトについてはこちらを読んで下さい](https://developers.line.biz/ja/reference/messaging-api/#message-objects)

```JavaScript
// 対象の声優がゲームに出演している時
function createPostMessages(sheet, rows) {
  // 行数分の配列を作成します
  return rows.map(function(row) {
    return {
      'type': 'text',
      'text': postMessage(sheet, row),
    }
  });
}

 // 対象の声優がゲームに出演していなかった時
function createNotExistsPostMessage(month, voiceActorName) {
  return [{
    'type': 'text',
    'text': notExistPostMessage(month, voiceActorName),
  }]
}

// リストページを返す時
function createListPagePostMessage(year, month) {
  return [{
    'type': 'text',
    'text': listPagePostMessage(year, month),
  }]
}
```

メッセージ作成メソッド群

```JavaScript
// 対象の声優がゲームに出演している時のメッセージの時
function postMessage(sheet, row) {
  // 対象の行データを全て取得する
  var rowValues        = sheet.getRange(row, 1, 1, maxColumnsCount).getValues()[0];
  
  // メッセージに表示するための情報を取得します
  var releaseDate      = Utilities.formatDate(rowValues[columns.ReleaseDate], "JST", "yyyy/MM/dd"),
      title            = rowValues[columns.Title],
      price            = rowValues[columns.Price],
      introductionPage = rowValues[columns.IntroductionPage],
      brandPage        = rowValues[columns.BrandPage];

  var message = releaseDate + '\n' +
                title + '\n' +
                price + '\n' +
                introductionPage + '\n' +
                '\n' +
                brandPage;
  
  return message;
}

// 対象の声優がゲームに出演していなかった時のメッセージの時
function notExistPostMessage(month, voiceActorName) {
  var message = '';
  
  if (month == targetMonth.LastMonth) {
    message = targetMonth.LastMonthString + voiceActorName;
  } else if (month == targetMonth.NextMonth) {
    message = targetMonth.NextMonthString + voiceActorName;
  } else {
    message = targetMonth.CurrentMonthString + voiceActorName;
  }
  
  message = message + 'はゲームに出演する予定はありません'
  
  return message;
}


// リストページのメッセージの時
function listPagePostMessage(year, month) {
  var message = year + '年' + month + '月の発売リストページです\n' + 
               'http://www.getchu.com/all/price.html?genre=pc_soft&year=' + year + 
               '&month=' + month + 
               '&gage=&gall=all';
  return message;
}
```

## Slackに通知する

LINE BOTだけのつもりだったのですが、スクレイピングツールで書き込んだ内容が正しいのかどうかを判断するため、Slackにも通知されるようにしました。GASのいいところは定期実行が簡単に指定できるので、決まった時間にSlackに通知させることは容易です。

![Slack通知](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/87d06089-aab7-b7ae-7863-1185c93dc40c.png)

Slack通知を行うためのコードが下記になります。Slack通知でめんどくさいのが[Attachements](https://api.slack.com/docs/message-attachments)の作成です。
逆に言うと[Attachements](https://api.slack.com/docs/message-attachments)を頑張って作成するといい感じでSlackに通知されるようになります。[Attachements](https://api.slack.com/docs/message-attachments)は通知される時のメッセージのデザインを細かく設定できるものです。

```JavaScript
// 発売リスト一覧のメッセージをSlackに送信する
function releaseListSendMessage() {
  // 送信するメッセージ情報を作成する
  var yearMonth = getYearMonth(0),
      sheet     = getSheet(yearMonth),
      foundRows = getAllRows(sheet);
  var attachments = (function(sheet, rows) {
    // 対象のデータを全て取得しそのデータからAttachementを作成する
    var rowsCount = rows[rows.length - 1] - 1,
        values    = sheet.getRange(2, 1, rowsCount, maxColumnsCount).getValues();
        
    Logger.log(values);
        
    return values.map(function(value) {
      var brandName        = value[columns.BrandName],
          title            = value[columns.Title],
          introductionPage = value[columns.IntroductionPage],
          releaseDate      = Utilities.formatDate(value[columns.ReleaseDate],"JST","yyyy/MM/dd"),
          price            = value[columns.Price],
          voiceActors      = value[columns.VoiceActor],
          packageImage     = value[columns.PackageImage];
      return createAttachement(brandName, title, introductionPage, releaseDate, price, voiceActors, packageImage);
    });
  }(sheet, foundRows));
  
  // メッセージを作成する
  var year    = yearMonth.slice(0,4),
      month   = yearMonth.slice(4),
      message = year + '年' + month + '月の発売リスト';
      
  // Slackにメッセージを送信する　
  sendMessage(message, 
              config.SlackPostChannel, 
              '発売リストくん', 
              config.SlackPostUserIcon, 
              attachments);
}

// Attachementを作成する
function createAttachement(brandName, title, introductionPage, releaseDate, price, voiceActors, image) {
  return {
      "author_name": brandName,
      "color": "#a8bdff",
      "title": title,
      "title_link": introductionPage,
      "text": releaseDate + '\n' + 
              price + '\n' + 
              voiceActors,
      "image_url": image
  }
}

// Slackのあるチャンネルにメッセージを送信する
function sendMessage(message, channel, username, iconUrl, attachments) {
  var payload = {
    channel: channel,
    text: message,
    username: username,
    icon_url: iconUrl,
    attachments: attachments,
  };
  
  var option = {
    'method': 'post',
    'payload': JSON.stringify(payload),
    'contentType': 'application/x-www-form-urlencoded; charset=utf-8',
    'muteHttpExceptions': true
  };
  
  // Slackにメッセージを送信
  var response = UrlFetchApp.fetch(config.SlackWebHookUrl, option);
}
```

設定方法としては、スクリプトエディタからトリガーを設定するだけです。

![sample_trigger_button.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/169e5af4-c083-ebbd-8f3e-8d7c688f92c6.png)

トリガー設定画面
設定は関数単位で指定できるので`releaseListSendMessage`を設定します。

![torigger_scrreen](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/375591d5-a87e-fa02-dbba-2da112d936e7.png)

![sample_trigger_page.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a999068e-79fa-d969-556a-2f3c35695325.png)

この設定で１週間ごと、毎週金曜日にSlackに通知されるようになります。

# まとめ
Googleスプレッドシートへの書き込みがまだ手動なので今後はここを自動化したいと思っています。

開発していて一番時間がかかったのはRubyのRSpecです。１ヶ月以上戦っていた気がします。LINE BOTとGASに関しては触ったことがなかったのですが１０時間ぐらいでできました。
１０時間ぐらいなので休みの日をかけてLINE BOTを余裕で作成できるレベルです。

皆さんも気軽にLINE BOTを作成してみるのものいいかもしれません！
