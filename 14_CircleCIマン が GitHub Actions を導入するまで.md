# CircleCIマン が GitHub Actions を導入するまで

普段は **CircleCI** でCI/CDを構築していた自分が何もわからない状態から **GitHub Actions** をどうやって導入したかを紹介しようと思います

自作のRubyのコマンドラインツールにCIを導入した時の話になります

# 自作のRubyのコマンドラインツールについて

[qiita_command](https://github.com/dodonki1223/qiita_command) という Qiitaのトレンド情報（Daily, Weekly, Monthly）をコマンドラインで簡単に見れるツールです

![sample](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_command/00_sample.gif)

# GitHub Actionsの初回導入

とりあえず簡素な状態でGitHub Actionsが動く状態まで持っていきます

## テンプレートの選択

まず始めに **Actions** をクリックし **Ruby** の **Set up this workflow** をクリックします

リポジトリの内容にあったWorkflowsテンプレートを表示してくれるので良さそうなものを選択し導入します

![00_get_started_with_github_actions](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/01_select_template/00_get_started_with_github_actions.png)

## Workflowsのファイルをリポジトリに追加

当たり前だがCircleCIとymlファイルの中身が違う
見てもよくわからない……

とりあえず、 **RSpecが実行される** ように **Run tests の部分を変更する**

### 変更前

```yml
    - name: Run tests
      run: bundle exec rake
```

### 変更後

```yml
    - name: Run tests
      run: bundle exec rspec
```

リポジトリに **.github/workflows** 配下にGitHub Actionsのファイルが出来上がることがわかります

![00_edit_new_file](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/02_add_workflows_to_repository/00_edit_new_file.png)

最後に **Start commit** をクリックして **Commit new file** をクリックしてリポジトリにコミットします

![01_first_commit](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/02_add_workflows_to_repository/01_first_commit.png)

## 実行結果を確認する

再度、 **Actions** をクリックし Workflows の一覧から先程作成したWorkflowsのコミットの **Create ruby.yml** をクリックします

![00_created_workflows](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/03_confirm_run_result/00_created_workflows.png)

左側の **test** をクリックし詳細を確認します

![01_before_click_test](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/03_confirm_run_result/01_before_click_test.png)

CIが実行中の場合です

![02_click_test](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/03_confirm_run_result/02_click_test.png)

CIが完了すると以下のような画面になります

![03_test_complted](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/03_confirm_run_result/03_test_complted.png)

以上で簡単に RSpec を実行するだけの CI を実装することができました

## workflows ファイルの中身を確認してみる

### name、jobs について

**name** が上に表示され、 **jobs** の内容が name 配下に表示されます  
jobs は複数追加することができます

![00_workflows_name_check](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_check_workflows/00_workflows_name_check.png)


### on について

GitHub Actions が実行されるトリガーを設定することができます（いろいろなイベントに対して [トリガー](https://docs.github.com/ja/free-pro-team@latest/actions/reference/events-that-trigger-workflows) を実行できる）  
下の例だと main ブランチでpush、pull request が行われた時に実行されるように設定されています

```yml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
```

CircleCIでもブランチによるCIの制御を行うことができるが push や pull request で制御を行うことができない
CircleCIだと以下のようになる

```yml
workflows:
  build-deploy:
    jobs:
      - build_server_pdfs:
          filters:
            branches:
              only: main
```

![01_workflows_on](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_check_workflows/01_workflows_on.png)

### runs-on について

ここで実行する環境を指定します  
ubuntu の最新バージョンになります

```yml
runs-on: ubuntu-latest
```

CircleCIで言うところの下記のような記述の一部と同じになります  
GithubAcitonsではOS寄りな環境設定になりますが CircleCI では言語に寄っていることが多い気がします
Docker Image の **circleci/ruby:2.6.0** が何で作られているかによってOSが決まります

```yml
executors:
  base:
    docker:
      - image: circleci/ruby:2.6.0
```

![02_workflows_runs-on](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_check_workflows/02_workflows_runs-on.png)

### uses について

uses を使用することにより、再利用可能なコードを宣言することができる  
with を使用して設定を追加することができる

- [actions/checkout@v2](https://github.com/actions/checkout)はリポジトリをチェックアウトしてくれる
- [ruby/setup-ruby](https://github.com/ruby/setup-ruby)はビルド済みのRubyをダウンロードしPATHに追加して Ruby を使えるようにしてくれる

```yml
steps:
- uses: actions/checkout@v2
- name: Set up Ruby
  uses: ruby/setup-ruby@ec106b438a1ff6ff109590de34ddc62c540232e0
  with:
    ruby-version: 2.6
```

CircleCI で言うところの **CircleCI Orbs** に近い気がします

```yml
orbs:
  slack: circleci/slack@3.4.2
・
・
・
workflows:
  version: 2.1
  main:
    jobs:
      - slack/approval-notification:
          message: ':circleci-pass: Slackへメッセージを送付します'
```

![03_workflows_uses](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_check_workflows/03_workflows_uses.png)

### run について

これは CircleCI と同じでコマンドを実行できます

```yml
- name: Install dependencies
  run: bundle install
- name: Run tests
  run: bundle exec rspec
```

![04_workflows_run](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_check_workflows/04_workflows_run.png)

コマンドの実行履歴で **name** で設定した部分が GitHub Actions に表示されます

![05_workflows_run_name](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_check_workflows/05_workflows_run_name.png)

# 自分の思い通りにGitHub Actionsを設定する

とりあえずGitHub Actionsというものを雰囲気分かってもらえたと思います  
最小構成で実装することができたが他のプロジェクトではどのように設定しているのだろうか……

## 先人の知恵をお借りする

[BestGems.org](https://bestgems.org/) という Ruby gems のダウンロードランキングを確認することができるサイトから総ダウンロード数TOP10のプロジェクトを参考にしてみたいと思います  

![00_bestgems](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/05_wisdom_of_anestors/00_bestgems.png)

| ランキング  | 名前                                                              | GitHub Actions使用有無  |
|:-----------:|:------------------------------------------------------------------|:-----------------------:|
| 1           | [rspec-expectations](https://github.com/rspec/rspec-expectations) | ❌                      |
| 2           | [rspec-core](https://github.com/rspec/rspec-core)                 | ❌                      |
| 3           | [rspec-mocks](https://github.com/rspec/rspec-mocks)               | ❌                      |
| 4           | [diff-lcs](https://github.com/halostatue/diff-lcs)                | ⭕                      |
| 5           | [rspec-support](https://github.com/rspec/rspec-support)           | ❌                      |
| 6           | [rspec](https://github.com/rspec/rspec)                           | ❌                      |
| 7           | [bundler](https://github.com/rubygems/bundler)                    | ⭕                      |
| 8           | [multi_json](https://github.com/intridea/multi_json)              | ❌                      |
| 9           | [rack](https://github.com/rack/rack)                              | ⭕                      |
| 10          | [rake](https://github.com/ruby/rake)                              | ⭕                      |

※ランキングは 2020年10月16日のものです

[diff-lcs](https://github.com/halostatue/diff-lcs)、[bundler](https://github.com/rubygems/bundler)、[rack](https://github.com/rack/rack)、[rake](https://github.com/ruby/rake) を参考にして作成していきたいと思います

### ファイル構成を確認する

プロジェクトごとファイル構成を確認する

#### diff-lcs

ci.yml は複数のOS、複数のRubyのバージョンでのテストを実行しているようだ

```
diff-lcs
 └ .github
     └ workflows
         └ ci.yml
```

#### bundler

主にテストをOSごと実行する Workflow に Linter を実行する Workflow に分けているようだ


```
bundler
 └ .github
     └ workflows
         ├ jruby.yml
         ├ ubuntu-bundler3.yml
         ├ ubuntu-lint.yml
         ├ ubuntu.yml
         └ windows.yml
```

#### rack

development.yml は複数のOS、複数のRubyのバージョンでのテストを実行しているようだ

```
rack
 └ .github
     └ workflows
         └ development.yml
```

#### rake

主にテストをOSごと、複数のRubyバージョンでのテストを実行しているようだ

```
rake
 └ .github
     └ workflows
         ├ macos.yml
         ├ test.yml
         └ windows.yml
```

### ファイルの中身を参考にする

ファイルの中身を確認し参考になりそうな部分を確認してみる

#### rake/.github/workflows/test.yml

下記のようにすると

```yml
runs-on: ${{ matrix.os }}
strategy:
  matrix:
    os: [ 'ubuntu-latest', 'macos-latest', 'windows-latest' ]
    ruby: [ 2.6, 2.5, 2.4, 2.3, 2.2, jruby, jruby-head, truffleruby, ruby-head ]
・
・
・
steps:
- uses: ruby/setup-ruby@v1
  with:
    ruby-version: ${{ matrix.ruby }}
```

OSとRubyのバージョンを配列で宣言して１つのファイルでCIを実行することができるようだ

```yml
name: ubuntu

on: [push, pull_request]

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ 'ubuntu-latest', 'macos-latest', 'windows-latest' ]
        ruby: [ 2.6, 2.5, 2.4, 2.3, 2.2, jruby, jruby-head, truffleruby, ruby-head ]
        exclude:
          - os: windows-latest
            ruby: truffleruby
          - os: windows-latest
            ruby: jruby-head
          - os: windows-latest
            ruby: jruby
    steps:
    - uses: actions/checkout@v2
    - uses: ruby/setup-ruby@v1
      with:
        ruby-version: ${{ matrix.ruby }}
    - name: Install dependencies
      run: gem install minitest
    - name: Run test
      run: ruby -Ilib exe/rake
```

#### diff-lcs/.github/workflows/ci.yml

下記のようにすると

```yml
runs-on: ${{ matrix.os }}-latest
```

runs-onの指定の時 **-latest** で宣言するとOSの指定をOS名だけで宣言することができるらしい
つまりOS名とバージョンの指定を分離することができる

```yml
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.
# This workflow will download a prebuilt Ruby version, install dependencies and run tests with Rake
# For more information see: https://github.com/marketplace/actions/setup-ruby-jruby-and-truffleruby

name: CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu
          - macos
          - windows
        ruby:
          - 2.5
          - 2.6
          - 2.7
          - head
          - debug
          - mingw
          - mswin
          - jruby
          - jruby-head
          - truffleruby
          - truffleruby-head
        exclude:
          - os: macos
            ruby: mingw
          - os: macos
            ruby: mswin
          - os: ubuntu
            ruby: mingw
          - os: ubuntu
            ruby: mswin
          - os: windows
            ruby: debug
          - os: windows
            ruby: truffleruby
          - os: windows
            ruby: truffleruby-head
    runs-on: ${{ matrix.os }}-latest
    continue-on-error: ${{ endsWith(matrix.ruby, 'head') || matrix.ruby == 'debug' || matrix.os == 'windows' }}
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
          bundler-cache: true
      - name: Install dependencies
        run: bundle install
      - name: Run tests
        run: bundle exec ruby -S rake
```

## workflows のファイルを編集する

上記情報を元にファイルを編集していく

### 複数OS、複数Rubyバージョンで実行できるようにする

- workflows 名を **CI** に変更
- OSを **ubuntu、macos** で実行できるようにする
- Rubyのバージョンを **2.7、2.6** で実行できるようにする
- uses で使用している **ruby/setup-ruby** のバージョンを **v1** にする

```yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  ci:
    strategy:
      matrix:
        os: [ubuntu, macos]
        ruby: [2.7, 2.6]
    runs-on: ${{ matrix.os }}-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
      - name: Install dependencies
        run: bundle install
      - name: Run tests
        run: bundle exec rspec
```

### ブランチでの絞り込みをなくす

- push と pull_request のイベント時に実行されるよう **on** 句を変更する

```yml
on: [push, pull_request] 
```

<details>
<summary>on句を上記の書き方に変更する</summary>
<div>

```yml
name: CI

on: [push, pull_request]

jobs:
  ci:
    strategy:
      matrix:
        os: [ubuntu, macos]
        ruby: [2.7, 2.6]
    runs-on: ${{ matrix.os }}-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
      - name: Install dependencies
        run: bundle install
      - name: Run tests
        run: bundle exec rspec
```

</div>
</details>

### 静的解析ツールを実行する

- RuboCopが実行されるようにする

```yml
- name: Run Rubocop
  run:  bundle exec rubocop
```

<details>
<summary>上記設定を追加する</summary>
<div>

```yml
name: CI

on: [push, pull_request]

jobs:
  ci:
    strategy:
      matrix:
        os: [ubuntu, macos]
        ruby: [2.7, 2.6]
    runs-on: ${{ matrix.os }}-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
      - name: Install dependencies
        run: bundle install
      - name: Run Rubocop
        run:  bundle exec rubocop
      - name: Run tests
        run: bundle exec rspec
```

</div>
</details>

### テストのカバレッジ結果を見れるようにする

CircleCIだと **store_artifacts** を使用するとアーティファクトのアップロードができるようになります  
同じことをGitHub Actionsでもできるようにします  
公式のドキュメントの以下の記事を参考にテストのカバレッジをアーティファクトとしてアップロードされるようにしようと思います

- [ワークフロー データをアーティファクトとして保存する - GitHub Docs](https://docs.github.com/ja/free-pro-team@latest/actions/guides/storing-workflow-data-as-artifacts)

記事を参考に下記の設定を追加します

```yml
- name: Archive code coverage results
  uses: actions/upload-artifact@v2
  with:
    name: code-coverage-report
    path: coverage
```

<details>
<summary>設定追加後</summary>
<div>

```yml
name: CI

on: [push, pull_request]

jobs:
  ci:
    strategy:
      matrix:
        os: [ubuntu, macos]
        ruby: [2.7, 2.6]
    runs-on: ${{ matrix.os }}-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
      - name: Install dependencies
        run: bundle install
      - name: Run Rubocop
        run:  bundle exec rubocop
      - name: Run tests
        run: bundle exec rspec
      - name: Archive code coverage results
        uses: actions/upload-artifact@v2
        with:
          name: code-coverage-report
          path: coverage
```

</div>
</details>

### Slack通知を実装する

CircleCI の場合は Orb を使用することでSlack通知を行うことができるようになる  

GitHub Actionsでは同じように uses を使用して行うことができるようだ
幾つかSlack通知が行えるものがあるようだが今回はドキュメントもしっかりとある [action-slack](https://github.com/marketplace/actions/action-slack) を使用して実装する

ドキュメントを参考に以下を追加する

```yml
- name: Github Actions notify to Slack
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    fields: repo,message,commit,author,action,eventName,ref,workflow,job,took
    mention: 'here'
    if_mention: failure
  env:
    GITHUB_TOKEN: ${{ github.token }}
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
    MATRIX_CONTEXT: ${{ toJson(matrix) }}
  if: always()
```
　
このままでは以下の部分が設定されてないので読み取ることができない

```yml
secrets.SLACK_WEBHOOK_URL
```

以下の公式の記事を参考にSlackの **WEBHOOK_URL** をリポジトリに設定します
CircleCI の環境変数を設定することと同じことをしています

- [Encrypted secrets - GitHub Docs](https://docs.github.com/ja/free-pro-team@latest/actions/reference/encrypted-secrets)

下記の画面みたいな表示になっていれば **SLACK_WEBHOOK_URL** 設定は大丈夫です

![00_complete_slack_webhook_url](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/06_edit_workflows/00_complete_slack_webhook_url.png)

<details>
<summary>上記設定が完了したら設定を追加します</summary>
<div>

```yml
name: ci

on: [push, pull_request]

jobs:
  ci:
    strategy:
      matrix:
        os: [ubuntu, macos]
        ruby: [2.7, 2.6]
    runs-on: ${{ matrix.os }}-latest
    steps:
      - uses: actions/checkout@v2
      - name: set up ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: ${{ matrix.ruby }}
      - name: install dependencies
        run: bundle install
      - name: run rubocop
        run:  bundle exec rubocop
      - name: run tests
        run: bundle exec rspec
      - name: archive code coverage results
        uses: actions/upload-artifact@v2
        with:
          name: code-coverage-report
          path: coverage
      - name: github actions notify to slack
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          fields: repo,message,commit,author,action,eventname,ref,workflow,job,took
          mention: 'here'
          if_mention: failure
        env:
          github_token: ${{ github.token }}
          slack_webhook_url: ${{ secrets.slack_webhook_url }}
          matrix_context: ${{ tojson(matrix) }}
        if: always()
```

</div>
</details>

# 実際に実行する

上記で編集した workflows のファイルを push することで GitHub Actions が実行されるようになります

## 詳細を確認していく

OS、RubyのバージョンごとCIが実行されるようになっています

![00_run_start](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/07_run_after_edited/00_run_start.png)　

完了すると **Artifacts** にテストのカバレッジ結果が zip で圧縮されてダウンロードできるようになっています  
CircleCIと違って画面から結果を確認することはできません

![01_complete_run](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/07_run_after_edited/01_complete_run.png)　

また１つのCIが完了するごとにSlackに完了通知が飛びます

![02_notify_slack](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/07_run_after_edited/02_notify_slack.png)　

これで一通りやりたかったことができるようになりました！

# 応用編：Gemの自動アップデート用のプルリクをGitHub Actionsで自動化する

応用編として、Gemの自動アップデート用のプルリクを自動で作成する workflows を作ろうと思います

## SSH で GitHub Actions に入る

CIを作成していく段階で個人的に必須な SSH で接続する機能は CircleCI では当たり前だが GitHub Actions ではどうするのか……  
公式では用意されていないようなので uses を使用して行うことができる  
下記の記事を参考にするとよい

- [GitHub ActionsにSSHで入る(簡単3ステップ) - Qiita](https://qiita.com/shonansurvivors/items/cb8902acfe5c3a1b3ca0)

## スケジュールで実行するにはどうしたよいのか？

CircleCI だと[トリガー](https://circleci.com/docs/ja/2.0/triggers/)を使用することでスケジュールされたCIを実行することができる  
GitHub Actions では [on.schedule](https://docs.github.com/ja/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#onschedule) を使用することで実現できそうです

CircleCI でも GitHub Actions のどちらも cron 形式でスケジュールを設定することができます
[crontab guru](https://crontab.guru/) のサイトを参考にしてスケジュールを設定するとよいでしょう

こんな感じで設定する

```yml
on:
  schedule:
    # * はYAMLに置ける特殊文字なので、この文字列は引用符で囲まなければならない
    - cron:  '*/15 * * * *'
```

schedule の詳しい仕様については公式のドキュメントを参考にすること

- [ワークフローをトリガーするイベント - GitHub Docs](https://docs.github.com/ja/enterprise-server@2.22/actions/reference/events-that-trigger-workflows)

CircleCI と同じで GitHub Actions も cron は UTC で設定する必要があります

## 手順を考える

以下の手順をCIで実行することができれば実現できそうである

- ① Gitの設定を行う（メールアドレス、ユーザー名）
- ② Gem Update 用のブランチを作成する
- ③ bundle update を行う
- ④ commit して push する
- ⑤ プルリクエストを自動で作成する
- ⑥ Slackに完了通知を行う

## 実装する

上記で考えた手順を元に実際に実装していく

### 手順以外の実装

毎月の１日の９時に実行されるようにする  
cron 式で設定した以外は先程、作成したものとほぼ同じである

<details>
<summary>設定内容</summary>
<div>

```yml
name: GemUpdate

on:
  schedule:
    - cron: '0 0 1 * *'

jobs:
  create-gem-update:
    strategy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.7
      - name: Install dependencies
        run: bundle install
```

</div>
</details>

### ① Gitの設定を行う（メールアドレス、ユーザー名）

git checkout ができているので git はおそらくインストールされている前提ですすめる  
**インストールされているかなど調査する時は直接 ssh で接続して確認すると良い**

複数行の時はCircleCIと同じく **|** を使用するようだ

```yml
- name: Settings Git
  run: |
    git config --global user.email ${{ secrets.MAIL_ADDRESS }}
    git config --global user.name "dodonki1223"
```

**secrets.MAIL_ADDRESS** こちらの設定は **SLACK_WEBHOOK_URL** と同じように設定します  
この場合はメールアドレス直打ちでもいいような気がする

### ② Gem Update 用のブランチを作成する

ブランチを名前を付けて切り替えるコマンドです  
**gem_update_20201001** みたいな形がブランチ名になります

```bash
git checkout -b "gem_update_`date +%Y%m%d`"
```

### ③ bundle update を行う

前の段階で bundle install を行っているので update を行うだけです

```bash
bundle update
```

### ④ commit して push する

ファイルをコミットして先程作成したブランチに push します

```bash
git add .
git commit -m ':wrench: Gem Update'
git push origin "gem_update_`date +%Y%m%d`"
```

### ⑤ プルリクエストを自動で作成する

[hub](https://github.com/github/hub)コマンドを使用してプルリクエストを作成する
最近、[GitHub CLI](https://github.com/cli/cli) がリリースされたので [hub](https://github.com/github/hub)コマンドの代わりに [GitHub CLI](https://github.com/cli/cli) を使用するでもよいかもしれません

```bash
hub pull-request -b master -m "🔧 Gem Update `date +%Y-%m-%d`"
```

### ②〜⑤を組み立てる

基本的には②〜⑤をそのまま繋げるだけで大丈夫なのですが下記の記述が追加で必要です

```yml
env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

上記記述に関しては [hub](https://github.com/github/hub)コマンドのリポジトリに Readme に GitHub Actions で使用する時のサンプル例に書かれている例になります

- [hub is ready to be used in your GitHub Actions workflows](https://github.com/github/hub#github-actions)

```yml
- name: Create gem update pull request
  run: |
    git checkout -b "gem_update_`date +%Y%m%d`"
    bundle update
    git add .
    git commit -m ':wrench: Gem Update'
    git push origin "gem_update_`date +%Y%m%d`"
    hub pull-request -b master -m "🔧 Gem Update `date +%Y-%m-%d`"
  env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

コマンドを組み上げる時は基本的には実際に ssh でログインして実際の環境で使用できるか確認しながらすすめるとよいです  
コマンドのインストールが必要かどうかなども ssh でログインしながら確かめると良いです

### ⑥ Slackに完了通知を行う

こちらは先程、作成したもので既に行っているので特に説明はしません

```yml
- name: Github Actions notify to Slack
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    fields: repo,message,commit,author,action,eventName,ref,workflow,job,took
    mention: 'here'
    if_mention: failure
  env:
    GITHUB_TOKEN: ${{ github.token }}
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
  if: always()
```

### 完成品

実際に作成したもので最終形は以下になります

<details>
<summary>完成品</summary>
<div>


```yml
name: GemUpdate

on:
  schedule:
    - cron: '0 0 1 * *'

jobs:
  create-gem-update:
    strategy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 2.7
      - name: Install dependencies
        run: bundle install
      - name: Settings Git
        run: |
          git config --global user.email ${{ secrets.MAIL_ADDRESS }}
          git config --global user.name "dodonki1223"
      - name: Create gem update pull request
        run: |
          git checkout -b "gem_update_`date +%Y%m%d`"
          bundle update
          git add .
          git commit -m ':wrench: Gem Update'
          git push origin "gem_update_`date +%Y%m%d`"
          hub pull-request -b master -m "🔧 Gem Update `date +%Y-%m-%d`"
        env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - name: Github Actions notify to Slack
        uses: 8398a7/action-slack@v3
        with:
          status: ${{ job.status }}
          fields: repo,message,commit,author,action,eventName,ref,workflow,job,took
          mention: 'here'
          if_mention: failure
        env:
          GITHUB_TOKEN: ${{ github.token }}
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
        if: always()
```

</div>
</details>

## 注意事項

**on.schedule** は下記の時だけ実行されます

> デフォルトまたはベースブランチの直近のコミットで実行されます

これに気づかないといくらやってもスケジュールが実行されずハマることになります……自分は実行されずにすごく困ったらこれが原因でした

# CircleCIとGitHub Actions の違うところ

自分が感じた CircleCI と GitHub Actions の違いをまとめようと思います

## GitHub Actionsは遅い時がある

GitHub Actionsを使っていて気になったのだがなぜかすごく遅い時がある
CircleCIだとだいたい終了時間が同じ感覚を受けるがGitHub Actionsはやたらと遅い時がある

使用されるサーバーのスペックガチャにより早かったり、遅かったりするのかも知れない……

## いろいろなトリガーが用意されている

CircleCI と違っていろいろな webhookイベントをトリガーに実行できるようだ
詳しくはドキュメントを参考にすると良い

- [ワークフローをトリガーするイベント - GitHub Docs](https://docs.github.com/ja/enterprise-server@2.22/actions/reference/events-that-trigger-workflows)

## ファイルの構成がスッキリする

CircleCIと違ってファイルの分割ができるため、行数を少なくすることができる

# 最後に

ずっとGitHub Actionsが難しそうで逃げていましたが実際にやってみるとすぐに実装することができました  
先人の知恵をお借りしたことにより自分の中で思ったよりも早く理解することができたのだと思います

これからもGitHub Actions を使っていきましょう！！
