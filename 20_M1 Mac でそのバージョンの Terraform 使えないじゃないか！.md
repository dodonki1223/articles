Terraform で **バージョン 0.12.5** を使って勉強するぞと思ったが M1 Mac で動かないじゃないか！
どうしたらいいんや……と困っていた時に解決した方法になります。

# Terraform の勉強しよう

なんで Terraform を勉強しようと思ったのか少しだけ昔話しをさせて下さい。

よぉーし **AWS の勉強するぞ！** と思っていざ勉強しようと思っても何をしたらいいんだろうかと思ったことはありませんか？
そこでまずは資格の勉強を始めてみました。
そして約１ヶ月後……。無事、資格を取得しました！
資格を取得したし何かシステムを構築しようと思ったけど…… あれ？ 自分何も構築できない。

そんな時に Terraform の勉強をしたんですが、これが自分にはすごくハマりました！
勉強した時に使用した書籍は「[実践Terraform　AWSにおけるシステム設計とベストプラクティス](https://nextpublishing.jp/book/10983.html)」になります。

<img width="40%" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/8aa7cb42-ad72-0380-3c28-e63cb4c3b785.jpeg">　　
画像は　https://nextpublishing.jp/book/10983.html より引用

自分で AWS のリソースを作り、削除を繰り返しながらよく使われている一般的なリソースを作っていくことで自分の中に色々と知見がたまっていき、いざ欲しいと思った構成を調べながら構築していくことがなんとなくできるようになっていました。

# M1 Macで動かない

[実践Terraform　AWSにおけるシステム設計とベストプラクティス](https://nextpublishing.jp/book/10983.html) はすごく良かったのですが書籍で使用されている **バージョンは 0.12.5** です。
初めてこの書籍を元に Terraform で AWS の構築をしていた時は当時の最新バージョンで行っていました。
しかし、バージョン違いによって書籍通りに進まないことが多く結構苦労しました。

Terraform をまずは知るという意味だとバージョンは揃えて実践し無駄なことを考えなくて済むようにした方が良いでしょう。
しかし残念ながら**バージョン 0.12.5** は M1 Mac に対応したものがありません。

![01_not_installed](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/20/01_not_installed.png)

not found と言われインストールできないのです。

## どうにかして動かす

[M1 Macでtfenvを使うと特定のVersionのTerraformのダウンロードに失敗する](https://dev.classmethod.jp/articles/tfenv-on-m1-mac/) こんな記事を見つけました。
AMD 版のバイナリを手動でダウンロードしてくれば使えるそうです！

## asdf でバージョン 0.12.5 をインストールする

[M1 Macでtfenvを使うと特定のVersionのTerraformのダウンロードに失敗する](https://dev.classmethod.jp/articles/tfenv-on-m1-mac/) の記事では [tfenv](https://github.com/tfutils/tfenv) を使用した方法でしたが自分は [asdf](https://github.com/asdf-vm/asdf) を使用しているので [asdf](https://github.com/asdf-vm/asdf) での方法をこの記事に書こうと思います。

### バイナリを手動で取得する

AMD 版のバイナリを手動でダウンロードしてきます。

```shell
$ wget https://releases.hashicorp.com/terraform/0.12.5/terraform_0.12.5_darwin_amd64.zip

--2022-04-03 23:35:41--  https://releases.hashicorp.com/terraform/0.12.5/terraform_0.12.5_darwin_amd64.zip
releases.hashicorp.com (releases.hashicorp.com) をDNSに問いあわせています... 151.101.193.183, 151.101.1.183, 151.101.65.183, ...
releases.hashicorp.com (releases.hashicorp.com)|151.101.193.183|:443 に接続しています... 接続しました。
HTTP による接続要求を送信しました、応答を待っています... 200 OK
長さ: 16778594 (16M) [application/zip]
`terraform_0.12.5_darwin_amd64.zip' に保存中

terraform_0.12.5_darwin_ 100%[==================================>]  16.00M  7.36MB/s 時間 2.2s

2022-04-03 23:35:43 (7.36 MB/s) - `terraform_0.12.5_darwin_amd64.zip' へ保存完了 [16778594/16778594]
```

### バイナリを asdf の管理している場所へコピーする

まずはダウンロードした zip ファイルを解凍します。

```shell
$ unzip terraform_0.12.5_darwin_amd64.zip

Archive:  terraform_0.12.5_darwin_amd64.zip
  inflating: terraform
``` 

本来 asdf が特定のバージョンをインストールした際に作成されるディレクトリを手動で作成する。

```shell
$ mkdir -p ~/.asdf/installs/terraform/0.12.5/bin
```

作成したディレクトリに terraform のバイナリを移動する。

```shell
$ mv terraform ~/.asdf/installs/terraform/0.12.5/bin/
```

必要のなくなったものを削除する。

```shell
$ rm terraform_0.12.5_darwin_amd64.zip
```

### asdf にバージョン ０.１２.５ を認識させる

reshim コマンドを実行し手動で追加した terraform のバイナリを認識させる。

```shell
$ asdf reshim terraform 0.12.5
```

これは何を行っているのかというと `~/.asdf/shims/terraform`　ファイルにバージョンを追加する処理になります。
上記コマンドを実行すると　`# asdf-plugin: terraform 0.12.5` な記述が追加されます。

下記のような感じで表示されていれば問題ないでしょう。

```shell
$ cat ~/.asdf/shims/terraform
!/usr/bin/env bash
# asdf-plugin: terraform 1.1.0
# asdf-plugin: terraform 0.12.5
exec /opt/homebrew/opt/asdf/libexec/bin/asdf exec "terraform" "$@"
```

プラグインのバージョン一覧にも 0.12.5 が表示されることを確認します。

```shell
$ asdf list terraform
  0.12.5
  1.1.0
```

これでインストール完了です。お疲れさまでした。

# 最後に

M1 Mac の asdf で バージョン 1.0.1 以前の Terraform を使用する方法を記事にしました。M1 Mac を日々使いつつ Intel Mac との違いに戸惑いながらもなんとか使えている状況です。

M1 Mac 辛いですね……。
