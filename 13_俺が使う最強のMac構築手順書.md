# 俺が使う最強のMac構築手順書

Macを移行した時、今までの状態に戻すのにすごく苦労したのでちゃんとした手順書として作成しておく
本当はdotfilesを作ってコマンド１発で環境構築されるのが望ましいが後回しにしてしまっています……
dotfilesを作成する前に自分のMacで何が必要かを洗い出すためにも一旦ドキュメント化しました

新しいMacを買った時にいずれdotfilesに移行したいと思います

# こだわりポイント

## キー設定

特にこだわっているのがキーボードの設定でキーの設定をかなり追加していて他人が説明無しで使える代物では無いですが一応、公開しておきます
Macの操作をキーボードのホームポジションに近いキーだけで操作するため（ブラインドタッチのみ）のものです

例えばですがSlackの操作はキーボードだけで行うことが可能です（リアクション、スレッド表示などのあらゆる操作）。また以前書いた記事の [まだトラックパッドでChromeを操作しているの？ そろそろキーボードだけで操作しようぜ！！ Chromeを使い倒そう！！](https://qiita.com/dodonki1223/items/205a937c21030d1a511e) でもこのキー設定を導入することでさらにキーボードだけでの操作が行えるようになります

## MacOSの設定

MacOSの設定はめんどくさいのでコマンドだけで完結するようにしています
いずれdotfilesに移行するための布石としてコマンドで設定する方法をドキュメント化しました

# アプリのインストール

| アイコン                                                                                                                                                                                                                 | アプリ名               | 説明                         | URL                                                                                                                                              |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------|:-----------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------|
| <img width="100" alt="chrome"             src="https://www.google.co.jp/chrome/static/images/chrome-logo.svg">                                                                                                           | Google Chrome          | Web ブラウザ                 | [https://www.google.co.jp/chrome/](https://www.google.co.jp/chrome/)                                                                             |
| <img width="100" alt="intellij"           src="https://cdn.iconscout.com/icon/free/png-512/intellij-idea-569199.png">                                                                                                    | IntelliJ IDEA          | 統合開発環境                 | [https://www.jetbrains.com/idea/download/#section=mac](https://www.jetbrains.com/idea/download/#section=mac)                                     |
| <img width="100" alt="vscode"             src="https://upload.wikimedia.org/wikipedia/commons/9/9a/Visual_Studio_Code_1.35_icon.svg">                                                                                    | Visual Studio Code     | テキストエディタ             | [https://code.visualstudio.com/download](https://code.visualstudio.com/download)                                                                 |
| <img width="100" alt="tableplus"          src="https://tableplus.com/resources/favicons/apple-icon-180x180.png">                                                                                                         | TablePlus              | DB クライアント              | [https://tableplus.com/](https://tableplus.com/)                                                                                                 |
| <img width="100" alt="docker"             src="https://d1q6f0aelx0por.cloudfront.net/icons/docker-edition-apple6.png">                                                                                                   | Docker Desktop for Mac | 環境仮想化ツール　           | [https://hub.docker.com/](https://hub.docker.com/)                                                                                               |
| <img width="100" alt="iterm2"             src="https://upload.wikimedia.org/wikipedia/en/d/d7/ITerm2-icon.png">                                                                                                          | iTerm2                 | ターミナル                   | [https://iterm2.com/](https://iterm2.com/)                                                                                                       |
| <img width="100" alt="slack"              src="https://a.slack-edge.com/80588/marketing/img/meta/slack_hash_256.png">                                                                                                    | Slack                  | チャットツール               | [https://slack.com/intl/ja-jp/downloads/mac](https://slack.com/intl/ja-jp/downloads/mac)                                                         |
| <img width="100" alt="karabiner-elements" src="https://beadored.com/wp-content/uploads/2017/02/Karabiner-Elements.jpg">                                                                                                  | Karabiner-Elements     | キーボードカスタマイズツール | [https://pqrs.org/osx/karabiner/](https://pqrs.org/osx/karabiner/)                                                                               |
| <img width="100" alt="alfred"             src="https://www.alfredapp.com/media/logo4@2x.png">                                                                                                                            | Alfred                 | コマンドラインランチャー     | [https://www.alfredapp.com/](https://www.alfredapp.com/)                                                                                         |
| <img width="100" alt="spectacle"          src="https://static.macupdate.com/products/41147/m/spectacle-logo.webp?v=1568311675">                                                                                          | Spectacle              | ウィンドウ操作ツール         | [https://www.spectacleapp.com/](https://www.spectacleapp.com/)                                                                                   |
| <img width="100" alt="gitkraken"          src="https://www.gitkraken.com/img/glo/glo-git-gui.svg">                                                                                                                       | GitKraken              | Git GUI ツール               | [https://www.gitkraken.com/](https://www.gitkraken.com/)                                                                                         |
| <img width="100" alt="backup-and-sync"    src="https://www.concordcarlisle.org/cchstech/wp-content/uploads/sites/87/2017/11/Google-Backup-and-Sync.png">                                                                 | バックアップと同期     | Google ドライブ同期ツール    | [https://www.google.com/intl/ja_ALL/drive/download/backup-and-sync/](https://www.google.com/intl/ja_ALL/drive/download/backup-and-sync/)         |
| <img width="100" alt="googlejapanese"     src="https://www.google.co.jp/ime/images/product-icon.png">                                                                                                                    | Google 日本語入力      | IME ソフト                   | [https://www.google.co.jp/ime/](https://www.google.co.jp/ime/)                                                                                   |
| <img width="100" alt="xcode"              src="https://is1-ssl.mzstatic.com/image/thumb/Purple113/v4/8a/a7/f6/8aa7f6bf-d761-71b5-bc8b-66c831782e0d/Xcode-0-0-85-220-0-0-0-0-4-0-0-0-2x-sRGB-0-0-0-0-0.png/230x0w.png">   | Xcode                  | Apple 製品用開発ツール       | [https://apps.apple.com/jp/app/xcode/id497799835?mt=12](https://apps.apple.com/jp/app/xcode/id497799835?mt=12)                                   |
| <img width="100" alt="menumeters"         src="http://ragingmenace.com/software/menumeters/MenuMetersIcon.png">                                                                                                          | MenuMeters             | リソースモニター             | [https://member.ipmu.jp/yuji.tachikawa/MenuMetersElCapitan/](https://member.ipmu.jp/yuji.tachikawa/MenuMetersElCapitan/)                         |
| <img width="100" alt="gimp"               src="https://upload.wikimedia.org/wikipedia/commons/4/45/The_GIMP_icon_-_gnome.svg">                                                                                           | GIMP for macOS         | 画像編集ソフト 　            | [https://www.gimp.org/downloads/](https://www.gimp.org/downloads/)                                                                               |
| <img width="100" alt="giphy"              src="https://miro.medium.com/max/512/1*JWYsBBaBhQhl1yHbxDPJwg.png">                                                                                                            | GIPHY Capture          | GIF 作成ツール               | [https://giphy.com/apps/giphycapture](https://giphy.com/apps/giphycapture)                                                                       |
| <img width="100" alt="kindle"             src="https://images-na.ssl-images-amazon.com/images/I/71ZCJYuOqIL.png">                                                                                                        | Kindle for Mac         | 電子書籍リーダー　           | [https://www.amazon.co.jp/kindle-dbs/fd/kcp](https://www.amazon.co.jp/kindle-dbs/fd/kcp)                                                         |
| <img width="100" alt="keyboardcleantool"  src="https://folivora.ai/folivora/static/media/keyboardcleantool.1b5c8eb9.png">                                                                                                | KeyboardCleanTool      | キーボード掃除ツール　       | [https://folivora.ai/keyboardcleantool](https://folivora.ai/keyboardcleantool)                                                                   |
| <img width="100" alt="theunarchiver"      src="https://is1-ssl.mzstatic.com/image/thumb/Purple114/v4/e7/d9/ea/e7d9ea79-ebe0-02ec-8281-2e25dfd144a0/unarchiver.png/230x0w.png">                                           | The Unarchiver         | 解凍ソフト          　       | [https://apps.apple.com/jp/app/the-unarchiver/id425424353?mt=12](https://apps.apple.com/jp/app/the-unarchiver/id425424353?mt=12)                 |
| <img width="100" alt="zoom"               src="https://is3-ssl.mzstatic.com/image/thumb/Purple123/v4/01/c0/98/01c09897-1c7a-1210-6719-e29345f5901e/AppIcon-0-1x_U007emarketing-0-0-85-220-9.png/246x0w.png">             | ZOOM Cloud Meetings    | Web会議             　       | [https://apps.apple.com/us/app/zoom-us-cloud-video-meetings/id546505307](https://apps.apple.com/us/app/zoom-us-cloud-video-meetings/id546505307) |
| <img width="100" alt="blurred"            src="https://is2-ssl.mzstatic.com/image/thumb/Purple123/v4/9d/92/3e/9d923e23-27c4-a900-0203-7735c2527af4/AppIcon-0-0-85-220-0-0-0-0-4-0-0-0-2x-sRGB-0-0-0-0-0.png/246x0w.png"> | Blurred                | 使用しているウィンドウ可視化 | [https://apps.apple.com/app/blurred/id1497527363](https://apps.apple.com/app/blurred/id1497527363)         　　　　　　　　　　　　　　　        |

# Homebrew のインストール

- [公式サイト](https://brew.sh/index_ja)
  - Homebrew とは macOS 用パッケージマネージャー

```shell
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

# fish のインストール

- [公式サイト](http://fishshell.com/)
- [GitHub](https://github.com/fish-shell/fish-shell)

```shell
$ brew install fish
```

## fish用の設定を追加

fishの設定ファイルを追加する

```shell
$ touch ~/.config/fish/config.fish
```

<details>
<summary>設定内容</summary>
<div>

作成した設定ファイルに下記の内容を貼り付けること

```shell
#*****************************
#* エイリアスの独自設定
#*****************************
# ssh接続ツールのエイリアスを設定（独自ツール）
alias sshc='sh $HOME/.tool/ssh_connection/ssh_connection.sh'

# eroge_release_cmdのエイリアスを設定（独自ツール）
alias getchuya='cd $HOME/.tool/eroge_release_cmd/;bundle exec getchuya'

# qiita_command を使用してQiitaのトレンドを取得するエイリアス設定（独自ツール）
alias qiita='$HOME/.tool/qiita_command/qiita'

# hostsファイルをVSCodeで開く
alias hosts='sudo code /private/etc/hosts'

# .bashrcファイルをVSCodeで開く
alias bashrc='code ~/.bashrc'

# .bash_profileファイルをVSCodeで開く
alias bash_profile='code ~/.bash_profile'

# config.fishファイルをVSCodeで開く
alias config.fish='code ~/.config/fish/config.fish'

#*****************************
#* fishの色用定数
#*****************************
set normal (set_color normal)
set magenta (set_color magenta)
set yellow (set_color yellow)
set green (set_color green)
set red (set_color red)
set gray (set_color -o black)

#*****************************
#* Git用の設定
#*****************************
# Fish git prompt
set __fish_git_prompt_showdirtystate 'yes'
set __fish_git_prompt_showstashstate 'yes'
set __fish_git_prompt_showuntrackedfiles 'yes'
set __fish_git_prompt_showupstream 'yes'
set __fish_git_prompt_color_branch yellow
set __fish_git_prompt_color_upstream_ahead green
set __fish_git_prompt_color_upstream_behind red

# Status Chars
set __fish_git_prompt_char_dirtystate '⚡'
set __fish_git_prompt_char_stagedstate '→'
set __fish_git_prompt_char_untrackedfiles '☡'
set __fish_git_prompt_char_stashstate '↩'
set __fish_git_prompt_char_upstream_ahead '+'
set __fish_git_prompt_char_upstream_behind '-'

#*****************************
#* プロンプトの独自設定
#*****************************
# 左側
function fish_prompt
	set -l last_status $status

    set_color $fish_color_user
    printf 'ユーザー名 > %s%s' (basename (pwd)) (__fish_git_prompt)

    __fish_hg_prompt
    echo

    if not test $last_status -eq 0
        set_color $fish_color_error
    end

    echo -n '➤ '
    set_color normal
end
# 右側
function fish_right_prompt
    set_color yellow
    echo (date +'%Y-%m-%d %H:%M:%S')
end
```

</div>
</details>

# git のインストール

## Mac に標準でインストールされているが、Homebrew で管理させたいので、 Homebrew でインストールする

```shell
$ brew install git
```

ターミナルで下記のようにコマンドを打ち、結果が表示されれば OK です

```shell
$ git --version
git version 2.24.0
```

## hub（GitHubのコマンドラインツール）のインストール

詳しくは [hub](https://github.com/github/hub) を確認してください

```shell
$ brew install hub
```

## gitconfigの設定

#### ユーザー情報をセット

```shell
$ git config --global user.name "ユーザー名"
$ git config --global user.email 自分のメールアドレス
```

#### aliasの設定

下記記事の内容を参考にハッシュ値からプルリクを探すエイリアスを設定します

- [Commit Hash から、該当 Pull Request を見つける方法 - Qiita](https://qiita.com/awakia/items/f14dc6310e469964a8f7)

```shell
$ git config --global alias.showpr \!"f() { git log --merges --oneline --reverse --ancestry-path \$1...master | grep 'Merge pull request #' | head -n 1; }; f"
$ git config --global alias.openpr \!"f() { hub browse -- \`git log --merges --oneline --reverse --ancestry-path \$1...master | grep 'Merge pull request #' | head -n 1 | cut -f5 -d' ' | sed -e 's%#%pull/%'\`; }; f"
```

#### コミットテンプレートを設定

Emojiでコミットメッセージを飾れるようにコミットメッセージのテンプレートを設定する

```shell
$ touch ~/.commit_template
$ vim ~/.commit_template
```

以下の設定内容を貼り付けること

```shell

# ======== Emoji Prefix ========
# 🎉  :tada: Initial commit.
# ✨  :sparkles: New features.
# 🔧  :wrench: Changing configuration files.
# 💄  :lipstick: Updating the UI and style files.
# ✏  :pencil2: Fixing typos.
# 📝  :memo: Writing docs.
# 🔥  :fire: Removing code or files.
# 🐛  :bug: Fixing a bug.
# ♻  :recycle: Refactoring code.
# 🚨  :rotating_light: Removing linter warnings.
# 🚧  :construction: Work in progress.

# ==== Format ====
# :emoji: Subject
#
# Commit body...

# ==== The Seven Rules ====
# 1. Separate subject from body with a blank line
# 2. Limit the subject line to 50 characters
# 3. Capitalize the subject line
# 4. Do not end the subject line with a period
# 5. Use the imperative mood in the subject line
# 6. Wrap the body at 72 characters
# 7. Use the body to explain what and why vs. how
#
# How to Write a Git Commit Message http://chris.beams.io/posts/git-commit/

# for http://memo.goodpatch.co/2016/07/beautiful-commits-with-emojis/
```

下記コマンドを実行

```shell
$ git config --global commit.template ~/.commit_template
```

何か元ネタとなる記事があった気がするんですが忘れてしまいましたすいません。

## GitHubでssh接続するための準備

下記記事を参考に作業をするめる

- [GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~ - Qiita](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

公開鍵と秘密鍵を作成する

```shell
$ cd ~/.ssh
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/(username)/.ssh/id_rsa):id_git_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

公開鍵をGitHubにアップロードする

- [https://github.com/settings/ssh](https://github.com/settings/ssh)

画面右上の「Add SSH key」のボタンを押す

![add_ssh_key_button](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F40796%2Fe8dedf6e-5168-aaa9-65aa-47654b3cca3d.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&s=2f78302d86d7fbad77a4afa096eb402b)

「Title」に公開鍵名、「key」に公開鍵の中身を入れます

![add_an_ssh_key](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F40796%2Fa6082f2e-efbd-2528-25c2-241ecdd59a3c.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&s=c6b3de0d4d8cde1ed8ebf3ea0a6cd8e9)

```shell
$ pbcopy < ~/.ssh/id_git_rsa.pub
```

「key」の中に貼り付け、 `Add key` のボタンを押します

keyファイルを変更したためそれに対応するために `~/.ssh/config` の設定を変更します

```shell
# GitHub
Host github.com
    HostName github.com
    IdentityFile ~/.ssh/id_git_rsa
    User git
```

接続確認

```
$ ssh -T git@github.com
Hi ユーザー名! You've successfully authenticated, but GitHub does not provide shell access.
```

特にエラーがでなければ大丈夫です！

# フォントのインストール

- [MyricaM](https://myrica.estable.jp/myricamhistry/)
- [ファイルのダウンロード](https://github.com/tomokuni/Myrica/raw/master/product/MyricaM.zip)

解凍する時は右クリックから`このアプリケーションで開く` で `The Unarchiver` を選択して `shift-Jis` で解凍すること

# その他必要なものをインストール

```shell
$ brew install wget
$ brew install jq
$ brew install the_silver_searcher
$ brew install node-build
$ brew install golangci/tap/golangci-lint
$ brew install mysql@5.6
$ brew install circleci
$ brew install ffmpeg
$ brew tap heroku/brew && brew install heroku
```

# anyenv をマニュアルインストール

[anyenv](https://github.com/anyenv/anyenv) を入れることで `goenv`, `nodenv`, `phpenv`, `pyenv`, `rbenv` などの**env系を一括で管理できるようになる

## anyenv を clone する

```shell
$ git clone https://github.com/anyenv/anyenv ~/.anyenv
```

## anyenv にパスを通す

```shell
# bash_profileに設定を書き込む
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bash_profile

# 設定を再読込する
$ exec $SHELL -l
```

## anyenv をセットアップ

```shell
$ ~/.anyenv/bin/anyenv init
```

## anyenv-update のインストール

```shell
$ mkdir -p $(anyenv root)/plugins
$ git clone https://github.com/znz/anyenv-update.git $(anyenv root)/plugins/anyenv-update
```

実行する時は下記コマンドを実行すること

```shell
$ anyenv update
```

## その他

Go と node は入れたらインストールしたバージョンのものを global に設定すること

# git を使いやすくする

参考記事

- [「Git 補完をしらない」「git status を 1 日 100 回は使う」そんなあなたに朗報【git-completion と git-prompt】 - Qiita](https://qiita.com/varmil/items/9b0aeafa85975474e9b6)

## git-completion.bash のインストール

git 補完スクリプト、TAB キーで git のコマンドが補完される

```shell
$ wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -O ~/.git-completion.bash
$ chmod a+x ~/.git-completion.bash
$ echo "source ~/.git-completion.bash" >> ~/.bashrc
```

## git-prompt.sh のインストール

プロンプトに各種追加情報を表示可能にするスクリプト  
ブランチが `master` とか表示できるようになる

```shell
$ wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh -O ~/.git-prompt.sh
$ chmod a+x ~/.git-prompt.sh
$ echo "source ~/.git-prompt.sh" >> ~/.bashrc
```

## .bashrc に下記設定を追加

```shell
#***********************************************
#* Gitのリポジトリの時                         *
#*   Gitのリポジトリだとわかるように表示させる *
#*   Gitコマンドの補完機能を追加する           *
#***********************************************
# gitコマンドの補完スクリプトを読み込む
source ~/.git-completion.bash

# プロンプトに各種追加情報を表示可能にするスクリプトを読み込む、オプションは下記の通り
#   GIT_PS1_SHOWUPSTREAM
#      現在のブランチがupstreamより進んでいるとき">"を、遅れているとき"<"を、遅れてるけど独自の変更もあるとき"<>"を表示する。オプションが指>定できるけど(svnをトラックするかとか)
#   GIT_PS1_SHOWUNTRACKEDFILES
#      addされてない新規ファイルがある(untracked)とき"%"を表示する
#   GIT_PS1_SHOWSTASHSTATE
#      stashになにか入っている(stashed)とき"$"を表示する
#   GIT_PS1_SHOWDIRTYSTATE
#      addされてない変更(unstaged)があったとき"*"を表示する、addされているがcommitされていない変更(staged)があったとき"+"を表示する
source ~/.git-prompt.sh
GIT_PS1_SHOWDIRTYSTATE=true
```

## .bash_profile に下記設定を追加

```shell
# .bashrcファイルの読み込み
source ~/.bashrc

# Gitのリポジトリだとわかるように表示させる
# <PS1とは>
#   bashには、プロンプトを制御するために「PS1」という環境変数が使用されている
#   PS1変数は、exportコマンドを使いさまざまな特殊文字コードを利用すれば、表示形式を変更することが可能
#   変更方法はhttps://qiita.com/zaburo/items/9194cd9eb841dea897a0を参照
# export PS1='\[\033[32m\]\u@\h\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]\n\$ '
export PS1='\[\033[32m\]ユーザー名\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]\n\$ '
```

# 証明書設定

## .ssh フォルダを作成する

```shell
$ cd $HOME
$ mkdir .ssh
$ chmod 700 .ssh
$ cd .ssh
```

権限の変更は下記記事を参考に

- [Linux の権限確認と変更(chmod)（超初心者向け） - Qiita](https://qiita.com/shisama/items/5f4c4fa768642aad9e06)
- [chmod コマンド - Qiita](https://qiita.com/ntkgcj/items/6450e25c5564ccaa1b95)

## config ファイルの作成

```shell
$ touch config
```

設定方法は下記記事を参考に

- [~/.ssh/config について - Qiita](https://qiita.com/passol78/items/2ad123e39efeb1a5286b)

## 公開鍵認証ファイルの設定

権限の設定だけ気をつけるように

```shell
$ chmod 600 hogeFile
```

# 自作ツールのインストール＆設定

## 自作ツールを格納するディレクトリを作成する

```shell
$ mkdir $HOME/.tool
```

## デフォルトの ruby を変更する

デフォルトでシステムに ruby がインストールされているが `gem install bundler` を実行するとエラーとなる

- [gem install で permission エラーになった時の対応方法 - Qiita](https://qiita.com/nishina555/items/63ebd4a508a09c481150)

```shell
$ gem install bundler
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

rbenv で新しく ruby をインストールしそれを global に設定してあげる必要がある

### ruby のインストール

```
$ rbenv install 2.6.3
$ rbenv global 2.6.3
$ exec $SHELL -l
```

### bundler のインストール

```
$ gem install bundler
```

## qiita_trend_slack_notifier

Qiitaのトレンド記事をSlackに投稿するツールです
詳しくは [qiita_trend_slack_notifier](https://github.com/dodonki1223/qiita_trend_slack_notifier) を参照してください

![00_slack_notify_sample](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_trend_slack_notifier/00_slack_notify_sample.png)

### qiita_trend_slack_notifier のインストール

```shell
$ $HOME/.tool
$ git clone https://github.com/dodonki1223/qiita_trend_slack_notifier.git
$ cd qiita_trend_slack_notifier
$ bundle install --path vendor/bundle
```

### 設定を行う

- [qiita_trend_slack_notifier の設定を行う](https://github.com/dodonki1223/qiita_trend_slack_notifier#webhookurl%E3%81%A8qiita%E3%81%AB%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E3%81%99%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AE%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E3%81%A8%E3%83%91%E3%82%B9%E3%83%AF%E3%83%BC%E3%83%89%E3%81%AE%E8%A8%AD%E5%AE%9A%E3%82%92%E8%A1%8C%E3%81%86)

### 動作確認

下記コマンドを実行し Slack に Qiita のトレンドが投稿されたら完了です

```shell
$ ruby notify_trend.rb
```

## qiita_command

Qiitaのトレンド情報をコマンドで取得しコンソール上に表示することができるツールです

![qiita_command](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_command/00_sample.gif)

### qiita_command のインストール

```shell
$ $HOME/.tool
$ git clone git@github.com:dodonki1223/qiita_command.git
$ bundle install
```

### 設定を行う

- [qiita_command の設定を行う](https://github.com/dodonki1223/qiita_command#qiita%E3%81%AE%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E6%83%85%E5%A0%B1%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)

### 動作確認

下記コマンドを実行し Qiita のトレンド情報が取得できたら完了です

```shell
$ ./qiita
```

## eroge_release_cmd

[げっちゅ屋](http://www.getchu.com/top.html?gc=gc)の[発売日リストページ](http://www.getchu.com/all/price.html?genre=pc_soft&gage=&gall=all)をスクレイピングしてその内容をコマンドで確認することができるツールです
詳しくは[eroge_release_cmd](https://github.com/dodonki1223/eroge_release_cmd)を参照してください

![00_eroge_release_cmd](https://raw.githubusercontent.com/dodonki1223/image_garage/master/eroge_release_cmd/00_eroge_release_cmd.gif)



### eroge_release_cmd のインストール

```shell
$ $HOME/.tool
$ git clone https://github.com/dodonki1223/eroge_release_cmd.git
$ cd eroge_release_cmd
```

### 設定を行う

- [Google スプレッドシートの設定](https://github.com/dodonki1223/eroge_release_cmd#google%E3%82%B9%E3%83%97%E3%83%AC%E3%83%83%E3%83%89%E3%82%B7%E3%83%BC%E3%83%88%E3%81%AE%E8%A8%AD%E5%AE%9A)
- [Google スプレッドシートへの書き込みの認証を行う](https://github.com/dodonki1223/eroge_release_cmd#3-google%E3%82%B9%E3%83%97%E3%83%AC%E3%83%83%E3%83%89%E3%82%B7%E3%83%BC%E3%83%88%E3%81%B8%E3%81%AE%E6%9B%B8%E3%81%8D%E8%BE%BC%E3%81%BF%E3%81%AE%E8%AA%8D%E8%A8%BC%E3%82%92%E8%A1%8C%E3%81%86)

### 動作確認

下記コマンドを実行しデータを取得できたら完了です

```shell
$ bundle exec getchuya
```

## ssh_connection

複数のサーバーへssh接続する時、接続先名を忘れてしまうのでコマンド1つ覚えればssh接続できるようになるツールです
詳しくは [大吾「ワシ、ssh接続忘れるんだが……きさぁーまは？」、ノブ「貴様！？」](https://qiita.com/dodonki1223/items/16c0907a94dbb605a0fb) を参照してください

![ssh_connection_sample](https://raw.githubusercontent.com/dodonki1223/image_garage/master/ShellScript/ssh_connection/ssh_connection_sample.gif)

### ssh_connection のインストール

```shell
$ cd $HOME/project
$ git clone https://github.com/dodonki1223/ShellScript.git
$ cd ShellScript
$ cp -r ssh_connection $HOME/.tool/
```

### 設定ファイルにサーバーの情報セット

- [ssh_connection](https://github.com/dodonki1223/ShellScript/tree/master/ssh_connection)

```shell
$ cd $HOME/.tool/ssh_connection
$ vim server_connection.conf
```

# VSCode の code コマンドをインストール

- [The Visual Studio Code command-line options](https://code.visualstudio.com/docs/editor/command-line#_common-questions)

> On macOS, you need to manually run the Shell Command: Install 'code' command in PATH command (available through the Command Palette ⇧⌘P). Consult the macOS specific setup topic for details.

1. Command + Shift + P
2. Shell で検索
3. インストール `Shell Command: Install code command in PATH`

# .bash_profile、bashrc に設定を追加

## .bash_profile と.bashrc の違いってそもそも何？

.bash_profile は `ログイン時に1回、実行される`
よく設定するのは……

- 環境変数（export）

.bashrc は `シェル起動時に１回、実行される`
よく設定するのは……

- エイリアス
- シェル関数
- コマンドラインの補完

## .bashrc

下記設定を貼り付けてください

<details>
<summary>.bashrcの内容</summary>
<div>

```shell
#*****************************
#* エイリアスの独自設定
#*****************************
# llコマンドを設定
alias ll='ls -l'

# laコマンドを設定
alias la='ls -la'

# ssh接続ツールのエイリアスを設定（独自ツール）
alias sshc='sh $HOME/.tool/ssh_connection/ssh_connection.sh'

# eroge_release_cmdのエイリアスを設定（独自ツール）
alias getchuya='cd $HOME/.tool/eroge_release_cmd/;bundle exec getchuya'

# hostsファイルをVSCodeで開く
alias hosts='sudo code /private/etc/hosts'

# .bashrcファイルをVSCodeで開く
alias bashrc='code ~/.bashrc'

# .bash_profileファイルをVSCodeで開く
alias bash_profile='code ~/.bash_profile'

#***********************************************
#* Gitのリポジトリの時
#*   Gitのリポジトリだとわかるように表示させる
#*   Gitコマンドの補完機能を追加する
#***********************************************
# gitコマンドの補完スクリプトを読み込む
source /usr/local/etc/bash_completion.d/git-completion.bash

# プロンプトに各種追加情報を表示可能にするスクリプトを読み込む、オプションは下記の通り
#   GIT_PS1_SHOWUPSTREAM
#      現在のブランチがupstreamより進んでいるとき">"を、遅れているとき"<"を、遅れてるけど独自の変更もあるとき"<>"を表示する。オプションが指定できるけど(svnをトラックするかとか)
#   GIT_PS1_SHOWUNTRACKEDFILES
#      addされてない新規ファイルがある(untracked)とき"%"を表示する
#   GIT_PS1_SHOWSTASHSTATE
#      stashになにか入っている(stashed)とき"$"を表示する
#   GIT_PS1_SHOWDIRTYSTATE
#      addされてない変更(unstaged)があったとき"*"を表示する、addされているがcommitされていない変更(staged)があったとき"+"を表示する
source /usr/local/etc/bash_completion.d/git-prompt.sh
GIT_PS1_SHOWDIRTYSTATE=true

#***********************************************
#* コマンド履歴検索に前方一致検索を追加
#***********************************************
# コマンドの前方検索（ctrl + s）の追加
# 端末のロックを無効化する
stty stop undef

#***********************************************
#* コマンドの履歴をタブ関係なく共有する
#***********************************************
function share_history {
    history -a # 履歴ファイルに現在のセッションの履歴を追加する
    history -c # 履歴一覧から全ての項目を削除する
    history -r # 履歴ファイルを読み込み、内容を履歴一覧に追加する
}
# share_history関数ををプロンプト毎に自動実施
PROMPT_COMMAND='share_history'
# .bash_history追記モードは不要なのでOFFに
shopt -u histappend
```

</div>
</details>

## .bash_profile

<details>
<summary>.bash_profileの内容</summary>
<div>

```shell
#*******************************
#* .bashrcファイルの読み込み
#*******************************
source ~/.bashrc

#*******************************
#* ユーザー名表示のカスタマイズ
#*******************************
# Gitのリポジトリだとわかるように表示させる
# <PS1とは>
#   bashには、プロンプトを制御するために「PS1」という環境変数が使用されている
#   PS1変数は、exportコマンドを使いさまざまな特殊文字コードを利用すれば、表示形式を変更することが可能
#   変更方法はhttps://qiita.com/zaburo/items/9194cd9eb841dea897a0を参照
# export PS1='\[\033[32m\]\u@\h\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]\n\$ '
export PS1='\[\033[32m\]ユーザー名\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]\n\$ '

#*******************************
#* ツールの設定
#*******************************
# anyenvの設定
export PATH="$HOME/.anyenv/bin:$PATH"
eval "$(anyenv init -)"

# Starshipの設定
eval "$(starship init bash)"

#*******************************
#* PATHを通す
#*******************************
# IntelliJのパスを通す
export PATH="/usr/local/bin/idea:$PATH"

# mysqlにパスを通す
export PATH="/usr/local/opt/mysql@5.6/bin:$PATH"

#*******************************
#* その他
#*******************************
# macOS Catalina が別のシェルを使う設定になっている場合に bash シェルを呼び出すと、デフォルトのインタラクティブシェルは
# zsh になっているというメッセージが表示されます。この警告を非表示にするには、以下のコマンドを「~/.bash_profile」または
# 「~/.profile」に追加できます。
# 設定していないと下記のメッセージが表示される
# The default interactive shell is now zsh.
# To update your account to use zsh, please run `chsh -s /bin/zsh`.
# For more details, please visit https://support.apple.com/kb/HT208050.
export BASH_SILENCE_DEPRECATION_WARNING=1
```

</div>
</details>

# crontabの設定

自作ツールが常に実行されるようにcrontabに設定を追加

```shell
$ crontab -e
```

下記設定を貼りつけること

```shell
# cronの結果を確認する時は下記コマンドを実行しよう
# cat /var/mail/takagiyuuki
# tail -f -n 1000 /var/mail/takagiyuuki

# 毎日１７時３０分にQiitaのトレンドをSlackに通知する
30 17 * * * /bin/bash -cl 'cd $HOME/.tool/qiita_trend_slack_notifier/ && ruby notify_trend.rb'

# ローカルのMacのcronでRubyのプログラムを実行するとき、文字コードでハマったOrz
# 下記のようなエラーが発生してハマったよOrz

# stty: stdin isn't a terminal
# /Users/takagiyuuki/.tool/eroge_release_cmd/vendor/bundle/ruby/2.6.0/gems/inifile-3.0.0/lib/inifile.rb:522:in `===': invalid byte sequence in US-ASCII (ArgumentError)

# cronでプログラムを実行するときのlocaleとローカルPCのlocaleが合わなかったため上記のエラーが発生した
# US-ASCIIだと日本語が解釈されずに落ちた的な……
# 実行するときに文字コードをUTF8で実行するようにしたため解決！！

# 参考記事
# https://www.mk-mode.com/blog/2013/11/26/linux-cron-locale-behavior/#

# 「-Ku」はUTF8で実行するって意味（-kが文字コード指定）

# 金曜日の１７時００分に今月のエロゲの発売リストを更新する
0 17 * * 5 /bin/bash -cl 'cd $HOME/.tool/eroge_release_cmd/ && ruby -Ku getchuya -c -s --simple --clear_cache'

# 金曜日の１７時１０分に来月のエロゲの発売リストを更新する
10 17 * * 5 /bin/bash -cl 'cd $HOME/.tool/eroge_release_cmd/ && ruby -Ku getchuya -y $(date -v+1m +\%Y\%m) -c -s --simple --clear_cache'
```

# vimの設定

## Molokai（カラースキーム）をインストール

```shell
$ mkdir ~/.vim
$ cd ~/.vim
$ mkdir colors
$ git clone https://github.com/tomasr/molokai
$ mv molokai/colors/molokai.vim ~/.vim/colors/
```

## 自分のVim設定を反映させる

```shell
$ touch ~/.vimrc
$ vim ~/.vimrc
```

下記設定を貼り付けること

```shell
 " -----------------------------------
 " エディタ関連
 " ----------------------------------
 " テーマをmolokaiに変更
 colorscheme molokai
 " 行番号を表示
 set number
 " シンタックスハイライト
 syntax on
 " インデントをスペース４つ
 set tabstop=4
 " オートインデント
 set smartindent
  " -----------------------------------
 " 検索関連
 " ----------------------------------
 " 大文字/小文字区別なく検索
 set ignorecase
 " 検索文字列に大文字が含まれている場合は区別して検索
 set smartcase
 " 検索時に最後まで行ったら最初に戻る
 set wrapscan
  " -----------------------------------
 " その他
 " ----------------------------------
 " タイトルの表示
 set title
```

# Macの設定

Macの設定を `defaults` コマンドを使用して設定していきます  
コマンドで設定する場合は一度ログアウトする必要があります  

## defaultsコマンドの調べ方

下記記事を参考にすると良いでしょう

- [システム環境設定をターミナル（defaultsコマンド）から設定する方法（一般）](https://ottan.xyz/system-preferences-terminal-defaults-2-4643/)

```shell
# 現在の設定を出力
$ defaults read > defaults_before.txt

# Macに設定を画面から行う

# 設定変更後の設定を出力
$ defaults read > defaults_after.txt
```

`defaults_before.txt` と `defaults_after.txt` のファイルを比較して変わっている項目を抜き出しそれを設定するコマンドを作り上げればOKです

## キーのリピートをシステム環境設定の限界値を超えて設定する

下記コマンドをターミナルで実行する

```shell
$ defaults write -g InitialKeyRepeat -int 13
$ defaults write -g KeyRepeat -int 1
```

- `InitialKeyRepeat`
  - リピート入力認識までの時間
- `KeyRepeat`
  - キーのリピート

-int は値の型を表す。1 あたり 15 ms。つまり、

```
InitialKeyRepeat: 13 * 15 ms = 150 ms
KeyRepeat: 1 * 15 ms = 15ms
```

となる。 ちなみにシステム環境設定からの設定の限界は下記の通り。

```
InitialKeyRepeat: 15 * 15 ms = 225 ms
KeyRepeat: 2 * 15 ms = 30ms
```

## 3 本指ドラッグを有効にする

### 画面上から設定する場合

システム環境設定 → アクセシビリティ → ポインタコントロール → トラックパッドオプション → ドラッグを有効にする（ドロップダウンリスト）→ ３本指のドラッグ

### コマンドで設定する場合

３本指ドラッグの設定をコマンドで行います
設定には接続機器用と本体用の設定があります

３本指ドラッグの接続機器用を設定するコマンドは下記になります

```shell
$ defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag -bool true
```

設定されたかどうかを確認するコマンドは下記になります

```
$ defaults read com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag
```

３本指ドラッグの本体用を設定するコマンドは下記になります

```shell
$ defaults write com.apple.AppleMultitouchTrackpad TrackpadThreeFingerDrag -bool true
```

設定されたかどうかを確認するコマンドは下記になります

```
$ defaults read com.apple.AppleMultitouchTrackpad TrackpadThreeFingerDrag
```

## ポインタの移動を速くする

### 画面上から設定する場合

システム環境設定 → トラックパッド → 奇跡の速さを MAX に

### コマンドで設定する場合

ポイントの移動の速さをMAX設定にするコマンドを下記です

```shell
$ defaults write -g com.apple.trackpad.scaling 3
```

設定されたかどうかを確認するコマンドを下記になります

```shell
$ defaults read -g com.apple.trackpad.scaling
```

## Finder 設定

Finderを使いやすいように独自のキー設定を行います

### Finder の独自ショートカットキーを設定するコマンド

Finder の独自ショートカットキーを設定するコマンドは下記になります

```shell
$ defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "項目名" -string "ショートカット"
```

設定されたかどうかを確認するコマンドは下記になります  
Finder に独自に設定されたショートカットキーがすべて表示されます

```
$ defaults read com.apple.Finder NSUserKeyEquivalents
```

### 設定するキーの一覧とコマンド

| キー             | 結果          |
|:-----------------|:-------------:|
| 新規 Folder      | ctrl + K      |
| ここに項目を移動 | ctrl + ⌘ + V |
| ゴミ箱に入れる   | ctrl + d      |
| 情報を見る       | ctrl + l      |

キーを設定するコマンドを下記になります

```shell
# 「新規フォルダ」のショートカットキー設定
$ defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "新規フォルダ" -string "^k"
# 「ここに項目を移動」のショートカットキー設定
$ defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "ここに項目を移動" -string "@^v"
# 「ゴミ箱に入れる」のショートカットキー設定
$ defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "ゴミ箱に入れる" -string "^d"
# 「情報を見る」のショートカットキー設定
$ defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "情報を見る" -string "^l"
```

キーの設定を確認するコマンドは下記になります

```shell
# Finderの独自ショートカットキー設定の内容を確認
$ echo Finderの独自キー設定：$(defaults read com.apple.Finder NSUserKeyEquivalents)
```

### Finderの見た目を変更する

![01_finder_appearance](https://raw.githubusercontent.com/dodonki1223/image_garage/master/my_manual/01_finder_appearance.png)

設定するコマンドは下記になります

```shell
# フルパス表示に変更する
$ defaults write com.apple.finder _FXShowPosixPathInTitle -bool true; killall Finder
# ステータスバーを表示
$ defaults write com.apple.finder ShowStatusBar -bool true; killall Finder
# パスバーを表示
$ defaults write com.apple.finder ShowPathbar -bool true; killall Finder
```

設定が反映されているか確認するコマンドは下記になります

```shell
$ defaults read com.apple.finder _FXShowPosixPathInTitle
$ defaults read com.apple.finder ShowStatusBar
$ defaults read com.apple.finder ShowPathbar
```

### .DS_Storeファイルの作成を抑制する

設定するコマンドは下記になります

```shell
$ defaults write com.apple.desktopservices DSDontWriteNetworkStores true
```

設定が反映されているか確認するコマンドは下記になります

```shell
$ defaults read com.apple.desktopservices DSDontWriteNetworkStores
```

## メニューバーの見た目設定

![02_menu_bar_appearance](https://raw.githubusercontent.com/dodonki1223/image_garage/master/my_manual/02_menu_bar_appearance.png)

設定するコマンドは下記になります

```shell
# バッテリー残量を％表記に
$ defaults write com.apple.menuextra.battery ShowPercent -string "YES"
# 日付、曜日、時間の表記に
$ defaults write com.apple.menuextra.clock DateFormat -string 'EEE d MMM HH:mm'
```

設定が反映されているか確認するコマンドは下記になります

```shell
# バッテリー残量の表記確認
$ defaults read com.apple.menuextra.battery
# 日付、曜日、時間の表記確認
$ defaults read com.apple.menuextra.clock
```

## コントロール間のフォーカス移動をキーボードで操作をONにする

できるようになることは[Mac のアクセシビリティショートカット - キーボードをマウスのように使う](https://support.apple.com/ja-jp/HT204434#fullkeyboard)を参照してください
設定するコマンドは下記になります

```shell
$ defaults write NSGlobalDomain AppleKeyboardUIMode -int 2
```

設定が反映されているか確認するコマンドは下記になります

```shell
$ defaults read NSGlobalDomain AppleKeyboardUIMode
```

## 次のウインドウを操作対象にするのショートカットを変更する

同じウィンドウ同士で切り替えができるようになります
詳しくは [Mac向けアクティブウィンドウの切り替えショートカット](https://nishinatoshiharu.com/window-select-shortcut/)を参照すると良いでしょう

| キー                           | 結果        |
|:-------------------------------|:-----------:|
| 次のウインドウを操作対象にする |  ⌥ + tab   |

次のウインドウを操作対象にするのショートカットキーを設定するコマンドを下記です

```shell
# 次のウインドウを操作対象にするのショートカットキー設定
$ defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 27 "<dict><key>enabled</key><true/><key>value</key><dict><key>parameters</key><array><integer>65535</integer><integer>48</integer><integer>524288</integer></array><key>type</key><string>standard</string></dict></dict>"

# 次のウインドウを操作対象にするのショートカットキーを確認する（27をチェックすること）
$ defaults read com.apple.symbolichotkeys AppleSymbolicHotKeys
```

## 通知センターを表示のショートカットを変更する

| キー               | 結果        |
|:-------------------|:-----------:|
| 通知センターを表示 | fn + space  |

通知センターを表示のショートカットキーを設定するコマンドを下記です

```shell
# 通知センターを表示のショートカットキー設定
$ defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 163 "<dict><key>enabled</key><true/><key>value</key><dict><key>parameters</key><array><integer>32</integer><integer>49</integer><integer>8388608</integer></array><key>type</key><string>standard</string></dict></dict>"

# 通知センターを表示のショートカットキーを確認する（163をチェックすること）
$ defaults read com.apple.symbolichotkeys AppleSymbolicHotKeys
```

## Spotlightを無効化する

### 画面上から設定する場合

システム環境設定→Spotlight→キーボードショートカット...

- Spotlight検索を表示
- Finderの検索ウインドウを表示

上記チェックボックスをOFFにする

### コマンドで設定する場合

```shell
# Spotlight検索を表示を無効化
$ defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 64 "<dict><key>enabled</key><false/><key>value</key><dict><key>parameters</key><array><integer>65535</integer><integer>49</integer><integer>1048576</integer></array><key>type</key><string>standard</string></dict></dict>"

# Finderの検索ウインドウを表示を無効化
$ defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 65 "<dict><key>enabled</key><false/><key>value</key><dict><key>parameters</key><array><integer>65535</integer><integer>49</integer><integer>1572864</integer></array><key>type</key><string>standard</string></dict></dict>"

# Spotlight検索を表示、Finderの検索ウインドウを表示が無効化になっているか確認する（64、65をチェックすること）
$ defaults read com.apple.symbolichotkeys AppleSymbolicHotKeys
```

## 全てのコマンド

一括で実行する時は下記を全てコピーしてターミナルで実行してください

```shell
defaults write -g InitialKeyRepeat -int 13
defaults write -g KeyRepeat -int 1
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag -bool true
defaults write com.apple.AppleMultitouchTrackpad TrackpadThreeFingerDrag -bool true
defaults write -g com.apple.trackpad.scaling 3
defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "新規フォルダ" -string "^k"
defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "ここに項目を移動" -string "@^v"
defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "ゴミ箱に入れる" -string "^d"
defaults write com.apple.Finder NSUserKeyEquivalents -dict-add "情報を見る" -string "^l"
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true; killall Finder
defaults write com.apple.finder ShowStatusBar -bool true; killall Finder
defaults write com.apple.finder ShowPathbar -bool true; killall Finder
defaults write com.apple.menuextra.battery ShowPercent -string "YES"
defaults write NSGlobalDomain AppleKeyboardUIMode -int 2
defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 27 "<dict><key>enabled</key><true/><key>value</key><dict><key>parameters</key><array><integer>65535</integer><integer>48</integer><integer>524288</integer></array><key>type</key><string>standard</string></dict></dict>"
defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 64 "<dict><key>enabled</key><false/><key>value</key><dict><key>parameters</key><array><integer>65535</integer><integer>49</integer><integer>1048576</integer></array><key>type</key><string>standard</string></dict></dict>"
defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 65 "<dict><key>enabled</key><false/><key>value</key><dict><key>parameters</key><array><integer>65535</integer><integer>49</integer><integer>1572864</integer></array><key>type</key><string>standard</string></dict></dict>"
defaults write com.apple.symbolichotkeys AppleSymbolicHotKeys -dict-add 163 "<dict><key>enabled</key><true/><key>value</key><dict><key>parameters</key><array><integer>32</integer><integer>49</integer><integer>8388608</integer></array><key>type</key><string>standard</string></dict></dict>"
```

# karabiner-elements の設定

[karabiner-elements](https://karabiner-elements.pqrs.org/)はキーボードのキー設定を追加、書き換えられるツールです
英数キーとかなキーを使用して通常の約3倍以上のキー設定を追加しています
この設定によりホームポジションでいろいろなキーを打つことができます（数字やファンクションキーなど）

## 設定内容

<details>
<summary>設定内容</summary>
<div>

Simple Modifications で下記の通りキーを変更しています

| キー |     結果     |
| :--- | :----------: |
| 英数 | right_option |
| かな |      fn      |

### メニューバー、Dock へのフォーカスするキー設定

| キー                    |      結果      |
| :---------------------- | :------------: |
| かな + left_control + w | メニューバーへ |
| かな + left_control + e |    Dock へ     |

### Finder 独自のキー設定

| キー                          |                 結果                 |
| :---------------------------- | :----------------------------------: |
| left_command + e              |            Finder を開く             |
| かな + w                      |           ファイル名の変更           |
| left_control + t              |            新規タブを開く            |
| left_control + w              |           現在のタブを開く           |
| left_control + n              |            新規タブを開く            |
| left_control + f              |         検索ウィンドウを開く         |
| left_control + left_shift + l | 隠しファイルの表示・非表示の切り替え |
| かな + h                      |        1 番上へカーソルを移動        |
| かな + ;                      |        1 番下へカーソルを移動        |
| かな + 英数 + u               |             アイコン表示             |
| かな + 英数 + i               |              リスト表示              |
| かな + 英数 + o               |              カラム表示              |
| かな + 英数 + j               |            ギャラリー表示            |
| 英数 + d                      |            ファイルの削除            |
| 英数 + f                      |            ファイルの検索            |

Karabiner とは別で Mac のキーボードの設定で下記設定を OS 側の機能で追加する

| キー                  |        結果        |
| :-------------------- | :----------------: |
| control + l           |     情報を見る     |
| control + k           | 新規フォルダを作成 |
| control + d           |   ゴミ箱に入れる   |
| control + command + v |  ここに項目を移動  |

### Chrome 独自のキー設定

| キー                          |           結果           |
| :---------------------------- | :----------------------: |
| かな + a                      |         リロード         |
| left_control + l              | アドレスバーへフォーカス |
| left_control + t              |         新規タブ         |
| left_control + w              |       タブを閉じる       |
| left_control + n              |      新規ウィンドウ      |
| left_control + left_shift + n |  シークレットウィンドウ  |
| left_control + j              |       ダウンロード       |
| left_control + f              |           検索           |
| left_control + left_shift + i |  Developer Tool の表示   |

### カーソルの移動系のキー設定

| キー     |   結果   |
| :------- | :------: |
| かな + j |    ←     |
| かな + l |    →     |
| かな + i |    ↑     |
| かな + k |    ↓     |
| かな + h |   Home   |
| かな + ; |   End    |
| かな + u |  PageUp  |
| かな + o | PageDown |

### 遠いキーをホームポジションで押せるキー設定

| キー     | 結果 |
| :------- | :--: |
| かな + p |  \_  |
| かな + n |  ¥   |
| かな + m |  @   |
| かな + , |  [   |
| かな + . |  ]   |

### ファンクションキー設定

| キー     | 結果 |
| :------- | :--: |
| かな + q |  F1  |
| かな + w |  F2  |
| かな + e |  F3  |
| かな + r |  F4  |
| かな + a |  F5  |
| かな + s |  F6  |
| かな + d |  F7  |
| かな + f |  F8  |
| かな + z |  F9  |
| かな + x | F10  |
| かな + c | F11  |
| かな + v | F12  |

### 数値キー設定

| キー     | 結果 |
| :------- | :--: |
| 英数 + p |  0   |
| 英数 + u |  1   |
| 英数 + i |  2   |
| 英数 + o |  3   |
| 英数 + j |  4   |
| 英数 + k |  5   |
| 英数 + l |  6   |
| 英数 + m |  7   |
| 英数 + n |  8   |
| 英数 + , |  9   |

### Command + Q、Command + W、アクティビティモニタを起動、強制終了ウィンドウを表示、ESC、Tab、Shift+Tab

| キー                         |            結果            |
| :--------------------------- | :------------------------: |
| 英数 + q                     |      left_commnad + Q      |
| 英数 + w                     |      left_command + W      |
| 英数 + control + command + e | アクティビティモニタを起動 |
| 英数 + command + e           |  強制終了ウィンドウを表示  |
| 英数 + e                     |            ESC             |
| 英数 + r                     |            TAB             |
| 英数 + t                     |        Shift + TAB         |

### Enter、BackSpace、DELETE、単語選択、スクリーンショット

| キー     |        結果        |
| :------- | :----------------: |
| 英数 + a |       Enter        |
| 英数 + s |     BackSpace      |
| 英数 + d |       DELETE       |
| 英数 + f |      単語選択      |
| 英数 + g | スクリーンショット |

### 元に戻す、切り取り、コピー、貼り付け

| キー     |   結果   |
| :------- | :------: |
| 英数 + z | 元に戻す |
| 英数 + x | 切り取り |
| 英数 + c |  コピー  |
| 英数 + v | 貼り付け |

### その他

| キー         |               結果               |
| :----------- | :------------------------------: |
| 英数 + space |     CapsLock キーの切り替え      |
| かな + g     | 選択している文字を Google で検索 |
| command + l  |     スクリンーセーバーを起動     |

</div>
</details>

## 設定ファイルを追加する

```shell
$ touch $HOME/.config/karabiner/karabiner.json
$ code $HOME/.config/karabiner/karabiner.json
```

karabiner.json に下の設定をコピーして貼り付けください

<details>
<summary>設定内容</summary>
<div>

```json
{
  "global": {
    "check_for_updates_on_startup": true,
    "show_in_menu_bar": true,
    "show_profile_name_in_menu_bar": false
  },
  "profiles": [
    {
      "complex_modifications": {
        "parameters": {
          "basic.simultaneous_threshold_milliseconds": 50,
          "basic.to_delayed_action_delay_milliseconds": 500,
          "basic.to_if_alone_timeout_milliseconds": 1000,
          "basic.to_if_held_down_threshold_milliseconds": 500
        },
        "rules": [
          {
            "description": "メニューバーへフォーカス、Dockへフォーカス",
            "manipulators": [
              {
                "from": {
                  "key_code": "w",
                  "modifiers": {
                    "mandatory": ["fn", "left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f2",
                    "modifiers": ["left_control"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "e",
                  "modifiers": {
                    "mandatory": ["fn", "left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f3",
                    "modifiers": ["left_control"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "[Finder]Finderを表示、ファイル名の変更、新規タブを開く、タブを閉じる、新規ウィンドウを開く、１番下の要素へ、１番上の要素へ",
            "manipulators": [
              {
                "from": {
                  "key_code": "e",
                  "modifiers": {
                    "mandatory": ["left_command"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "shell_command": "open -a 'finder'"
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "w",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "return_or_enter"
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "t",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "t",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "w",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "w",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "n",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "n",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "h",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "up_arrow",
                    "modifiers": ["left_option"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "semicolon",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "down_arrow",
                    "modifiers": ["left_option"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "[Finder]表示の切り替え（アイコン、リスト、カラム、ギャラリー）、ファイルの削除、ファイルの検索、隠しファイルの表示の切り替え",
            "manipulators": [
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "u",
                  "modifiers": {
                    "mandatory": ["fn", "option"]
                  }
                },
                "to": [
                  {
                    "key_code": "1",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "i",
                  "modifiers": {
                    "mandatory": ["fn", "option"]
                  }
                },
                "to": [
                  {
                    "key_code": "2",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "o",
                  "modifiers": {
                    "mandatory": ["fn", "option"]
                  }
                },
                "to": [
                  {
                    "key_code": "3",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "j",
                  "modifiers": {
                    "mandatory": ["fn", "option"]
                  }
                },
                "to": [
                  {
                    "key_code": "4",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "d",
                  "modifiers": {
                    "mandatory": ["right_option"]
                  }
                },
                "to": [
                  {
                    "key_code": "d",
                    "modifiers": ["left_control"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "f",
                  "modifiers": {
                    "mandatory": ["left_control"]
                  }
                },
                "to": [
                  {
                    "key_code": "f",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.apple\\.finder$"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "l",
                  "modifiers": {
                    "mandatory": ["left_control", "left_shift"]
                  }
                },
                "to": [
                  {
                    "key_code": "period",
                    "modifiers": ["left_command", "left_shift"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "[Chrome]リロード、アドレスバーへフォーカス、新規タブ、タブを閉じる、新規ウィンドウ",
            "manipulators": [
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "a",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "r",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "t",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "t",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "w",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "w",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "n",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "n",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "[Chrome]シークレットウィンドウ、ダウンロード、検索、DeveloperToolの起動、Home、End",
            "manipulators": [
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "n",
                  "modifiers": {
                    "mandatory": ["left_control", "left_shift"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "n",
                    "modifiers": ["left_command", "left_shift"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "j",
                  "modifiers": {
                    "mandatory": ["fn", "left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "left_arrow",
                    "modifiers": ["left_control"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "l",
                  "modifiers": {
                    "mandatory": ["fn", "left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "right_arrow",
                    "modifiers": ["left_control"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "l",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "l",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "j",
                  "modifiers": {
                    "mandatory": ["left_control"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "l",
                    "modifiers": ["left_command", "left_option"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "f",
                  "modifiers": {
                    "mandatory": ["left_control"]
                  }
                },
                "to": [
                  {
                    "key_code": "f",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "conditions": [
                  {
                    "bundle_identifiers": ["^com\\.google\\.Chrome"],
                    "type": "frontmost_application_if"
                  }
                ],
                "from": {
                  "key_code": "i",
                  "modifiers": {
                    "mandatory": ["left_control", "left_shift"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "i",
                    "modifiers": ["left_command", "left_option"]
                  }
                ],
                "type": "basic"
              }
            ]
          },

          {
            "description": "カーソル移動系(→、←、↑、↓、Home、End、PageUp、PageDown)の設定",
            "manipulators": [
              {
                "from": {
                  "key_code": "j",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "left_arrow"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "k",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "down_arrow"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "i",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "up_arrow"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "l",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "right_arrow"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "o",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "page_down"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "u",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "page_up"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "semicolon",
                  "modifiers": {
                    "mandatory": ["fn", "left_control"]
                  }
                },
                "to": [
                  {
                    "key_code": "down_arrow",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "semicolon",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "end"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "h",
                  "modifiers": {
                    "mandatory": ["fn", "left_control"]
                  }
                },
                "to": [
                  {
                    "key_code": "up_arrow",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "h",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "home"
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "「_」、「¥」、「@」、「[」、「]」",
            "manipulators": [
              {
                "from": {
                  "key_code": "p",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "international1"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "n",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "international3"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "m",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "open_bracket"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "comma",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "close_bracket"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "period",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "backslash"
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "「F1」、「F2」、「F3」、「F4」、「F5」、「F6」、「F7」、「F8」、「F9」、「F10」、「F11」、「F12」",
            "manipulators": [
              {
                "from": {
                  "key_code": "q",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f1"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "w",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f2"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "e",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f3"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "r",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f4"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "a",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f5"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "s",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f6"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "d",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f7"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "f",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f8"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "z",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f9"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "x",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f10"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "c",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f11"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "v",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "f12"
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "数値キーの設定（１〜９）",
            "manipulators": [
              {
                "from": {
                  "key_code": "u",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "1"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "i",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "2"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "o",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "3"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "p",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "0"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "j",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "4"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "k",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "5"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "l",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "6"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "n",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "7"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "m",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "8"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "comma",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "9"
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "Command+Q、Command+W、アクティビティモニタを起動、強制終了ウィンドウを表示、ESC、TAB、Shift+TAB",
            "manipulators": [
              {
                "from": {
                  "key_code": "q",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "q",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "w",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "w",
                    "modifiers": ["left_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "e",
                  "modifiers": {
                    "mandatory": ["right_option", "left_control", "command"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "shell_command": "open -a 'Activity Monitor'"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "e",
                  "modifiers": {
                    "mandatory": ["right_option", "command"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "escape",
                    "modifiers": ["option", "command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "e",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "escape"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "r",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "tab"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "t",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "tab",
                    "modifiers": ["left_shift"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "Eneter、BackSpace、DELETE、単語選択、スクリーンショット",
            "manipulators": [
              {
                "from": {
                  "key_code": "a",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "return_or_enter"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "s",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "delete_or_backspace"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "d",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "delete_forward"
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "f",
                  "modifiers": {
                    "mandatory": ["right_option"]
                  }
                },
                "to": [
                  {
                    "key_code": "left_arrow",
                    "modifiers": ["left_option"]
                  },
                  {
                    "key_code": "right_arrow",
                    "modifiers": ["left_option", "left_shift"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "c",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "c",
                    "modifiers": ["right_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "g",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "5",
                    "modifiers": ["right_shift", "right_command"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "元に戻す、切り取り、コピー、貼り付け",
            "manipulators": [
              {
                "from": {
                  "key_code": "z",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "z",
                    "modifiers": ["right_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "x",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "x",
                    "modifiers": ["right_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "c",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "c",
                    "modifiers": ["right_command"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "v",
                  "modifiers": {
                    "mandatory": ["right_option", "right_control"]
                  }
                },
                "to": [
                  {
                    "key_code": "v",
                    "modifiers": ["right_command", "right_control"]
                  }
                ],
                "type": "basic"
              },
              {
                "from": {
                  "key_code": "v",
                  "modifiers": {
                    "mandatory": ["right_option"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "v",
                    "modifiers": ["right_command"]
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "CapsLockキーの切り替え",
            "manipulators": [
              {
                "from": {
                  "key_code": "spacebar",
                  "modifiers": {
                    "mandatory": ["right_option"]
                  }
                },
                "to": [
                  {
                    "key_code": "caps_lock"
                  }
                ],
                "type": "basic"
              }
            ]
          },
          {
            "description": "選択している文字をGoogleで検索する（規定のブラウザ）",
            "manipulators": [
              {
                "from": {
                  "key_code": "g",
                  "modifiers": {
                    "mandatory": ["fn"],
                    "optional": ["any"]
                  }
                },
                "to": [
                  {
                    "key_code": "c",
                    "modifiers": ["left_command"]
                  },
                  {
                    "shell_command": "open https://www.google.co.jp/search?q=$(pbpaste)"
                  }
                ],
                "type": "basic"
              }
            ]
          }
        ]
      },
      "devices": [
        {
          "disable_built_in_keyboard_if_exists": false,
          "fn_function_keys": [],
          "identifiers": {
            "is_keyboard": true,
            "is_pointing_device": false,
            "product_id": 630,
            "vendor_id": 1452
          },
          "ignore": false,
          "manipulate_caps_lock_led": true,
          "simple_modifications": []
        }
      ],
      "fn_function_keys": [
        {
          "from": {
            "key_code": "f1"
          },
          "to": {
            "consumer_key_code": "display_brightness_decrement"
          }
        },
        {
          "from": {
            "key_code": "f2"
          },
          "to": {
            "consumer_key_code": "display_brightness_increment"
          }
        },
        {
          "from": {
            "key_code": "f3"
          },
          "to": {
            "key_code": "mission_control"
          }
        },
        {
          "from": {
            "key_code": "f4"
          },
          "to": {
            "key_code": "launchpad"
          }
        },
        {
          "from": {
            "key_code": "f5"
          },
          "to": {
            "key_code": "illumination_decrement"
          }
        },
        {
          "from": {
            "key_code": "f6"
          },
          "to": {
            "key_code": "illumination_increment"
          }
        },
        {
          "from": {
            "key_code": "f7"
          },
          "to": {
            "consumer_key_code": "rewind"
          }
        },
        {
          "from": {
            "key_code": "f8"
          },
          "to": {
            "consumer_key_code": "play_or_pause"
          }
        },
        {
          "from": {
            "key_code": "f9"
          },
          "to": {
            "consumer_key_code": "fastforward"
          }
        },
        {
          "from": {
            "key_code": "f10"
          },
          "to": {
            "consumer_key_code": "mute"
          }
        },
        {
          "from": {
            "key_code": "f11"
          },
          "to": {
            "consumer_key_code": "volume_decrement"
          }
        },
        {
          "from": {
            "key_code": "f12"
          },
          "to": {
            "consumer_key_code": "volume_increment"
          }
        }
      ],
      "name": "Default profile",
      "selected": true,
      "simple_modifications": [
        {
          "from": {
            "key_code": "japanese_eisuu"
          },
          "to": {
            "key_code": "right_option"
          }
        },
        {
          "from": {
            "key_code": "japanese_kana"
          },
          "to": {
            "key_code": "fn"
          }
        }
      ],
      "virtual_hid_keyboard": {
        "country_code": 0
      }
    }
  ]
}
```

</div>
</details>

# Spectacleの設定

|   動作           | キー  |
|:----------------:|:-----:|
| Center           | ^⌥C  |
| Fullscreen       | ^⌥F  |
| Left Half        | ^⌘← |
| Right Half       | ^⌘→ |
| Top Half         | ^⌘→ |
| Bottom Half      | ^⌘↓ |
| Next Display     | ^⌥↑ |
| Previous Display | ^⌥↓ |

# Blurredの設定

- Blurred level
  - `40%` に設定
- Blur mode
  - `Single` に設定
- `Enable Blurred` をチェック
- `Start Blurred when login` をチェック
- `Open Preferences Window when login` をチェック

# バックアップと同期の設定

下記記事を参考にGoogleドライブのフォルダ名を `GoogleDrive` に変更する

- [いまさらながら、「Googleバックアップと同期」のフォルダ名が気に食わない](http://wigwamania.hatenablog.com/entry/2018/02/08/191657)

## 変更方法

`Google ドライブ` というフォルダ名を変更する

![20180208190624.png](https://cdn-ak.f.st-hatena.com/images/fotolife/w/wigwamania/20180208/20180208190624.png)

フォルダ名を `GoogleDrive` に変更してしまう

![20180208191345.png](https://cdn-ak.f.st-hatena.com/images/fotolife/w/wigwamania/20180208/20180208191345.png)

アラートが表示されるので `GoogleDrive` フォルダを指定すれば完了です

![20180208191508.png](https://cdn-ak.f.st-hatena.com/images/fotolife/w/wigwamania/20180208/20180208191508.png)

# Alfred の設定

設定ファイルを読み込めるように以下の順で画面を操作

Advanced→Set preferences folder...

- `GoogleDrive/00_license/Alfred/setting` を設定する

## Workflows

Workflowsは以下の4つをインストールする

### [AWS Console Services - Alfred Workflow](https://github.com/rkoval/alfred-aws-console-services-workflow)

AWSのサービスへすぐに飛ぶことができる

![demo.gif](https://raw.githubusercontent.com/rkoval/alfred-aws-console-services-workflow/master/demo.gif)

### [GitHub Workflow for Alfred 3](https://github.com/gharlan/alfred-github-workflow)

GitHubのリポジトリなどへすぐに飛ぶことができる

![screenshot.gif](https://raw.githubusercontent.com/gharlan/alfred-github-workflow/master/screenshot.png)

### [caniuse](https://github.com/willfarrell/alfred-caniuse-workflow)

caniuse.com から検索が簡単にできます

![caniuse-browser.png](https://raw.githubusercontent.com/willfarrell/alfred-caniuse-workflow/master/screenshots/caniuse-browser.png)

### [MDN Search](https://github.com/gilbarbara/alfred-workflows/tree/master/mdn-search)

MDNの検索が簡単にできます

![screenshot.png](https://raw.githubusercontent.com/gilbarbara/alfred-workflows/master/mdn-search/screenshot.png)

# VSCodeの設定

## 設定ファイルgithubで管理できるようにする

こちらの記事を参考にしてGit管理下とする

- [どこでも同じ設定でVisualStudioCodeを使う方法 - Qiita](https://qiita.com/canpok1/items/a7c4c96e3c1c2a1cc532)

```shell
# シンボリックリンクを作成
ln -fnsv $HOME/Library/ApplicationSupport/Code/User $HOME/project/vscode_setting

# テストディレクトリを作成して.gitの設定をコピーする
git clone --no-checkout https://github.com/ユーザー名/vscode_setting.git $HOME/project/test
mv $HOME/project/test/.git/ $HOME/project/vscode_setting/.git/

# まっさらな状態のjsonファイルを削除する
cd $HOME/project/vscode_setting
rm keybindings.json settings.json

# 最新の状態を取得し余分なファイルを削除する
git pull origin mater
# 余分なファイルを削除する
rm -rf $HOME/project/test/
```

# まとめ

少し長くなりましたが如何がだったでしょうか？

私はMacに[Karabiner-Elements](https://karabiner-elements.pqrs.org/)がインストールされていないとパフォーマンスが50%ぐらい落ちそうです
[Karabiner-Elements](https://karabiner-elements.pqrs.org/)は素晴らしいソフトです！！！！！！！

何かオススメの設定やソフトがありましたらぜひ教えて下さい！
