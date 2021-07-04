# OSSノススメ

ヤマノススメというアニメのタイトルを真似ました（笑）
実は一度も見たことありません。すいません……。

さて、本題ですが皆さんはOSSを読むことはありますか？
自分は割と読むことがあります！

自分の経験上ですがOSSを読むことで得られるものがたくさんありました。
QiitaやZennの記事だけでなくOSSも参考にしましょう！

# なぜOSSを読むのか？

皆さんも一度はあると思うんですが
コードを書いている時、「どういう風に書くのが良いのだろうか？」と考えたことは無いでしょうか？
書けるんだが、本当にこの書きぶりでいいのだろうか……と思ったことはないでしょうか？

そうです！ そんな時こそ **OSSを読みましょう！**

## OSSを読むことによって解決すること

- こういう時はどのようにして書いたら良いんだろうか？
- 一般的にはどのように実装するのだろうか？
- なんか書けたけどもっとDryに書けるのではないだろうか？
- 自社のプロダクトに参考になるコードがない！？ どうしよう……

こんな疑問を解決する手助けになります！

## QiitaやZennの記事では駄目なの？

もちろん、QiitaやZennの記事で疑問を解決することもあります。
ただ、その記事が書かれた時期によって参考にすべきかどうかを考えないといけません。

アプリケーションは日々進化しているため、数年前の記事はもしかしたらもう使えないものになっているかもしれません。まずは一次情報を見に行く癖をつけると個人的にはよいと思っています。

自分は調べる時は必ず **一番初めは公式ドキュメント** を見に行くようにしています。後輩にもそう教えています。
大体が公式ドキュメントで事足りることが多いですが、ちょっとむずかしいことをしようとすると途端によくわからなくなることがあります。

そんな時に自分が公式ドキュメントの次に参考にするのが**OSSです**。そして **英語の記事** 、**QiitaやZennの記事** という順番が割と多いです。

**決してQiitaやZennの記事を否定しているわけではありません。自分もめちゃくちゃ参考にさせてもらっています！**

順番にすると以下のようになります。

1. 公式ドキュメント
2. OSS
3. 英語の記事
4. Qiita、Zenn

## 英語の記事の探し方

少しだけ横道ですが英語の記事の探し方もついでに紹介しておきます。

以前、自分が書いた記事で申し訳ないんですがChromeのアドレスバーのカスタマイズ方法を参考にして英語記事だけを簡単に検索できるようにします。

- [まだトラックパッドでChromeを操作しているの？ そろそろキーボードだけで操作しようぜ！！ Chromeを使い倒そう！！](https://qiita.com/dodonki1223/items/205a937c21030d1a511e)

検索エンジンの追加で以下のURLを設定しておくといつでもアドレスバーから簡単に英語記事だけを検索できるようになります。
**Qiitaで記事検索出来るようにする**というところを参考にしてください。

```javascript
javascript:(
  function() {
    const input = decodeURIComponent('%s');
    window.open(`https://www.google.co.jp/search?q=${encodeURIComponent(input)}&lr=-lang_ja`);
  }
)();
```

![search_before_destroy](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/00_search_before_destroy.gif)

# OSSの読み方

では実際にOSSを読む時はどうやって読めば良いのか？
自分の読み方を紹介します！

- 1つのプロジェクトだけでなく、**複数のプロジェクトを参考**にする
- OSS同士で**似たような書きぶり**があるのでそれを参考にする
- 中身を理解しようとするのではなく**書きぶりを見る**

## なぜ書きぶりを見るの？

OSSを読んでいて自分はよくあることなんですが、読んでも理解できないことがあります。
これは完全に自分の実力不足もあるんですが、処理をする時の前提条件などはOSSで把握するには時間がかかります……。

なので中身を理解しようとせずに書きぶりに注力します！
その書きぶりを元にさらにGoogleで検索することで理解が深まります。

**なんだこのメソッドは🤔　→　Google検索　→　おぉ！そんなものがあったのか😮**

みたいな気付きがよくありました。なのでOSSの中身を理解しなくても大丈夫です。
もちろん、中身を理解できる方は理解した上で読むと良いと思います！

# 実際にOSSを読んでみる

今回はRailsのOSSを読んでいこうと思います。まずはどのOSSを参考にしたらいんだろうかと迷いますよね……。
自分はGithubでスター数の多いプロジェクトを参考にすることが多いです。基本的には以下のリポジトリを参考にしています。

**2021/06/30 時点の情報**ですので古い可能性があります。

| リポジトリ                                          | Rails             |  Ruby                    | star   |
|:---------------------------------------------------:|:-----------------:|:------------------------:|:------:|
| [forem](https://github.com/forem/forem)             | 6.0.3             | 2.7.2                    | 16,947 |
| [gitlabhq](https://github.com/gitlabhq/gitlabhq)    | 6.1.3.2           | 2.7.2                    | 22,637 |
| [huginn](https://github.com/huginn/huginn)          | 6.0.3.1           | 2.5.0以上                | 31,735 |
| [postal](https://github.com/postalhq/postal)        | 5.2.5             | 2.3.0以上                | 10,553 |
| [mastodon](https://github.com/tootsuite/mastodon)   | 6.1.3             | 2.5.0 〜 3.1.0           | 24,375 |
| [discourse](https://github.com/discourse/discourse) | 最新              | 2.7.0以上                | 33,541 |
| [spree](https://github.com/spree/spree)             | 6.1 or 6.0 or 5.2 | 2.5 or 2.6 or 2.7 or 3.0 | 11,291 |

## before_destroy を調べてみる

以前、ある機能改修でのことです。
後輩に `before_destroy` を使用しましたと報告を受けました。ただ自分が `before_destroy` を使用したことがなかったのでこの `Active Record コールバック` は一体どういう時に使うべきなのだろうか……と思い OSS を読んでみることにしました。

### Google で単純に検索してみる

Googleで検索してみるとTOPには model の削除可否チェックが出てきています。

![search_before_destroy_for_google](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/01_search_before_destroy_for_google.png)

### forem の OSS で 検索してみる

GitHubの検索機能を使用し before_destory を検索します。
cache と名の付くメソッドを実行していることが多いようです。メソッド名から察するにデータの削除と一緒にキャッシュに対して何かを行うようなことが多そうです。

![search_before_destroy_for_github](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/02_search_before_destroy_for_github.png)

Google検索と比べてみましたが、検索結果が全然違います。
もしGoogleでのみ検索していた場合は、削除可否に使うのかと思ってしまってもおかしくないですね。

## ここで問題が……

本来なら複数の OSS から before_destory を検索したいと思いますよね。
ただ、GitHub の検索機能だとそれぞれの OSS から検索しないといけないため、すごく非効率です……。

そんなことでお困りのあなたに私が行っている検索方法を共有しようと思います！

# 複数の OSS を簡単に検索できるようにする

私が実際に行っている複数の OSS を検索する方法です。

## VS Code のワークスペース機能を使用して検索する 

そもそもワークスペースとは？
公式ドキュメントには以下のように書かれています！

> A Visual Studio Code "workspace" is the collection of one or more folders that are opened in a VS Code window (instance). In most cases, you will have a single folder opened as the workspace but, depending on your development workflow, you can include more than one folder, using an advanced configuration called Multi-root workspaces.

出典：[What is a VS Code "workspace"?](https://code.visualstudio.com/docs/editor/workspaces)

ウィンドウに複数のルートフォルダを開けるようにする機能です。
複数の OSS をワークスペースに追加すると以下のような感じになります。

![workspace](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/03_workspace.png)

実際に `before_destory` をワークスペースで検索してみると以下のようになります！

![search_workspace](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/04_search_workspace.png)

複数の OSS から検索できるようになりました！ おめでとうございます！

## ちょっと待って！

ローカルにそれぞれの OSS を git clone してきてそれをワークスペースに設定するのめんどくさくない？
OSS が更新したらどうすりゃいいんや……。

そんなことを思ったそこのあなた！
大丈夫です！ もっと簡単に取得できるようにしましょう！

## OSS を簡単に取得＆更新できる仕組みを構築

Rails に関しては私が作成した [rails_oss](https://github.com/dodonki1223/rails_oss) リポジトリを clone してきて `bash clone_and_update.bash` を実行するだけで簡単にローカルに検索できる環境が出来上がります！

![rails_oss_project](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/05_rails_oss_project.png)

リポジトリの clone と update が終わった後は `rails_oss.code-workspace` ファイルを開き `cmd + shift + f` で検索したい文字を打てば検索できるようになります！

このツールは設定された OSS を clone ＆ update し自動でワークスペースファイルを作成するツールになります！
動作させると以下のような感じで自動で clone と update が行われるようになります。

![sample_run_rails_oss](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/17/06_sample_run_rails_oss.png)

## rails_oss の仕組みは？

簡単な bash ファイルになります。
clone_and_update.bash は以下のように書かれています。

```bash
#!/bin/bash

declare -a cloneList=($(cat ./list/ssh))

echo '{
	"folders": [' > rails_oss.code-workspace

for ((i = 0; i < ${#cloneList[@]}; i++)) {
  project_name=$(echo "${cloneList[i]}" | cut -d '/' -f2 | sed -e 's/.git//g')
  project_dir="./${project_name}"

  echo '		{
			"path": "'${project_name}'"
		},' >> rails_oss.code-workspace

  if [ ! -d $project_dir ]; then
    echo $project_name clone start...
    git clone ${cloneList[i]}
  else
    echo $project_name clone skip...
    echo $project_name update
    cd $project_name
    git fetch
    git pull
    cd ../
  fi
}

echo '	],
	"settings": {}
}' >> rails_oss.code-workspace
```

どんなことをやっているか簡単に説明します。

### clone ＆ update するリポジトリのリストを取得する

./list/ssh にリポジトリのリストが書かれています。
リポジトリのリストを取得します。

```bash
declare -a cloneList=($(cat ./list/ssh))
```

./list/ssh には以下のようにリストを設定しています。

```
git@github.com:forem/forem.git
git@github.com:gitlabhq/gitlabhq.git
git@github.com:huginn/huginn.git
git@github.com:postalhq/postal.git
git@github.com:tootsuite/mastodon.git
git@github.com:discourse/discourse.git
git@github.com:spree/spree.git
```

### clone & update を行う

./list/ssh で取得したリスト分、繰り返し処理を行います。
フォルダが存在してなければ clone を行い、フォルダが存在していれば update を行います。

```bash
# ./list/ssh に追加したリポジトリ分繰り返す
for ((i = 0; i < ${#cloneList[@]}; i++)) {
  # git@github.com:oss_name/oss_name.git の形式からプロジェクト名を取得
  project_name=$(echo "${cloneList[i]}" | cut -d '/' -f2 | sed -e 's/.git//g')

  # プロジェクトのフォルダパスを取得
  project_dir="./${project_name}"

  # プロジェクトのディレクトリが存在するか
  if [ ! -d $project_dir ]; then
    # プロジェクトが存在しない場合は git clone を実行する
    echo $project_name clone start...
    git clone ${cloneList[i]}
  else
    # プロジェクトが存在している場合は git fetch & git pull を行う
    echo $project_name clone skip...
    echo $project_name update
    cd $project_name
    git fetch
    git pull
    cd ../
  fi
}
```

### ワークスペースファイルを自動生成する

一部説明に必要のない箇所を省いています。
./list/ssh ファイルに記載されている git clone 用の URL からワークスペースのファイルを自動生成します。

```bash
  ・
  ・
  ・
echo '{
	"folders": [' > rails_oss.code-workspace
  ・
  ・
  ・
for ((i = 0; i < ${#cloneList[@]}; i++)) {
  ・
  ・
  ・
  echo '		{
			"path": "'${project_name}'"
		},' >> rails_oss.code-workspace
  ・
  ・
  ・
}

echo '	],
	"settings": {}
}' >> rails_oss.code-workspace
```

### 新しく OSS を追加する

新しく OSS を追加したい場合は ./list/ssh ファイルに git clone の URL を追加します。
./list/ssh ファイルに追加するだけでワークスペースファイルも更新されるので特に気にする必要はありません。

# 最後に

[rails_oss](https://github.com/dodonki1223/rails_oss) の仕組みを使用すれば Rails 以外のプロジェクトにも応用が効きます！
あまり OSS を読んだことがない方もこの仕組みを元に OSS を読んでみるのはいかがでしょうか？
もしよろしければ皆さんのおすすめの　OSS　を教えてください！

素敵な OSS ライフを！
