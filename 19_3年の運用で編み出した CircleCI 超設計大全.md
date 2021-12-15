個人でも業務でもすごくお世話になっている CircleCI について説明したいと思います。
設定する際の Tips など個人的な観点を元に紹介していきます！

# CircleCI の構造をざっくりと理解する

CircleCI で設定する `.circleci/config.yml` ファイルの中身の構造について理解していきます。
`config.yml` は大きく分けて、 `version`, `orbs`, `executors`, `commands`, `jobs`, `workflows` の6つのキーワードに分かれています。
この6つのキーワードを理解することで CircleCI がよくわからない方もざっくりと理解することができます。
`version` キーワードを除くその他のキーワードを見てもらうとわかりますが、基本的に `****s` になっているので複数指定することができます。

![00_code_block](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/00_code_block.png)

では順番にそれぞれのキーワードについてざっくりと説明していきます。

## version

**[version](https://circleci.com/docs/ja/2.0/configuration-reference/#version)** キーワードは CircleCI を動かすバージョンになります。
最新は **2.1** になっており、現在の CircleCI のドキュメントではほぼ **2.1** で書かれていることが多いです。
新規に CircleCI を構築する場合は、あえて古いバージョンを使用する必要はないのでここは何も考えずにとりあえず **2.1** を設定することがオススメします。

## executors

**[executors](https://circleci.com/docs/ja/2.0/configuration-reference/#executors-requires-version-21)** キーワードは CircleCI で実行するテストやデプロイを動かすための環境（job で使用する）になります。
docker を使ったり、ubuntu や macOS, Windows などを指定することができます。

**コード部分**

![04_executor_code](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/04_executor_code.png)

**CircleCI画面**

![04_executor](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/04_executor.png)

## commands

**[commands](https://circleci.com/docs/ja/2.0/configuration-reference/#commands-requires-version-21)** キーワードはいわゆるターミナルで実行するコマンドをひとまとめにしたものになります。
１つの実行コマンドだけでなく、複数の実行コマンドをまとめ、それに名前を付けることで何をする処理なのかわかりやすくなります。

**コード部分**

![03_command_code](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/03_command_code.png)

**CircleCI画面**

![03_command](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/03_command.png)

## jobs

**[jobs](https://circleci.com/docs/ja/2.0/configuration-reference/#jobs)** キーワードは executors で設定した環境を指定し複数の command を実行させるまとまりになります。

**コード部分**

![02_job_code](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/02_job_code.png)

**CircleCI画面**

![02_job](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/02_job.png)

## workflows

**[workflows](https://circleci.com/docs/ja/2.0/configuration-reference/#workflows)** キーワードは作成した job をどういう順番でどういう条件で実行するのか制御させるものになります。

**コード部分**

![01_workflow_code](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/01_workflow_code.png)

**CircleCI画面**

![01_workflow](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/01_workflow.png)

## orbs

**[orbs](https://circleci.com/docs/ja/2.0/configuration-reference/#orbs-requires-version-21)** キーワードは job, command, executor をまとめたものです。
CircleCI から特定の環境用の作られたものが大量に公開されており、そちらを使うことで簡単に CI/CD の構築ができるようになっています。

![05_orbs_code](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/05_orbs_code.png)

## それぞれのキーワードをわかりやすく図で表す

それぞれのキーワードをざっくりと図で表し、関係を理解しましょう！

![06_circleci_keywords](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/06_circleci_keywords.png)

# CircleCIの情報を探す

CircleCI は公式ドキュメントがすごくしっかりしているので、公式ドキュメントに沿って構築していくことでほとんどのことを実装することができます。
そこでちょっとした公式ドキュメントを読む時の Tips を紹介します。

## 日本語のドキュメントを読む

CircleCIのドキュメントが英語しか出てこないことがあります。
自分が見たいドキュメントの日本語版を探すのは大変です。そこで簡単に日本語のドキュメントを参照する方法があります。

英語のドキュメントのURLに `ja` を追加するだけで簡単に日本語のドキュメントに変更することができます。
以下のドキュメントに [https://circleci.com/docs/2.0/parallelism-faster-jobs/](https://circleci.com/docs/2.0/parallelism-faster-jobs/) に `ja` を追加してみます。

![07_search_document_en](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/07_search_document_en.png)

URL のドメイン名の後に `ja` を追加し [https://circleci.com/ja/docs/2.0/parallelism-faster-jobs/](https://circleci.com/ja/docs/2.0/parallelism-faster-jobs/) とすることで簡単に日本語のドキュメントを参照することができます。
日本語のドキュメントが表示されない時は諦めて英語のドキュメントを読みましょう！

![07_search_document_ja](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/07_search_document_ja.png)

# config.yml の修正した際は circleci config validate コマンドを使う

config.yml を修正した際は push して動作を確認するのではなく、まずは `circleci config validate` コマンドを使用して config.yml が構文的におかしいところがないかチェックしてもらいます。

昔は一々 push して確認していましたが、これは時間がかかるしとても非効率なので必ず `circleci config validate` コマンドを使用することをオススメします！

![13_circleci_config_validate](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/13_circleci_config_validate.png)

circleci コマンドは brew でインストールできます。

```shell
$ brew install circleci
```

# デバッグは ssh ログインを駆使しよう

CircleCI には落ちた job にログインする機能がデフォルトであります。
何かしらうまく job が機能しない場合は一々 push して確かめるのではなく、**ssh でログインして動く方法を確立してからそれを config.yml に落とし込む方が早く改修することができる** ようになります。

## ssh でのログイン方法

`Rerun` をクリックし、`Rerun Job with SSH` をクリックします。

![16_ssh_login_start](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/16_ssh_login_start.png)

再度、job が実行されるので終わるまで待ちます。
job の実行が終わると、最後に ssh でログインするコマンドが出力されるのでそれを使ってターミナルからアクセスします。

![16_how_to_ssh_login](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/16_how_to_ssh_login.png)

# ドキュメントの改修時は CircleCI をスキップさせる

公式ドキュメントにも書かれていますが、ドキュメントの改修時は CircleCI を実行させる必要は無いのでスキップさせることができます。
コミットのタイトルか説明に `[ci skip], [skip ci]` と入力すると CircleCI がスキップされます。

詳しくは下記のドキュメントを確認してください。

- [ビルドのスキップとキャンセル](https://circleci.com/docs/ja/2.0/skip-build/)

# ステータスバッジを使用して、Readmeをカッチョ良くする

![20_status_badge](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/20_status_badge.png)

Readme に CircleCI の実行結果のステータスバッジを置くことでカッチョ良くなります。
プロジェクトの以下のページにアクセスすることでコピーするだけでステータスバッジが設定できるようになります。

- https://app.circleci.com/settings/project/github/ユーザー名/リポジトリ名/status-badges

Markdownに貼り付けるだけで使えるようになっているので Copy ボタンを押して Readme に貼り付けてください。

![20_status_badge_screen](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/20_status_badge_screen.png)

## バッジが気に食わない場合はカスタマイズする

[Shields.io](https://shields.io/) を使うと更に自分好みのステータスバッジに変更することができます。
以下のページから CircleCI で絞り込み、必要な情報を入力するといい感じにステータスバッジを作成してくれます。

- [Shields.io - build](https://shields.io/category/build)

## ステータスバッジのサンプル

![CircleCI](https://circleci.com/gh/dodonki1223/template_sample_rails_6/tree/master.svg?style=svg)
![CircleCI](https://img.shields.io/circleci/build/github/dodonki1223/template_sample_rails_6/master?style=plastic)
![CircleCI](https://img.shields.io/circleci/build/github/dodonki1223/template_sample_rails_6/master)
![CircleCI](https://img.shields.io/circleci/build/github/dodonki1223/template_sample_rails_6/master?color=purple&style=for-the-badge)

# 構築時に大切なこと

今まで、いくつかの CircleCI を構築してきた経験でどういうふうに構築していくべきか自分が学んできたことを書いていきます。

## CircleCI を実行する環境

CircleCI を実行する環境つまり executor の設定で注意することになります。

### 再現性を担保させる

CircleCI が実行される環境がどこかのタイミングで変更されることがないようにすることです。
つまり **いつも同じ環境でビルドとデプロイ（再現性のあるビルド）が行われるようにすること** が大切です！

デプロイする成果物は **ソースコードが同じであれば必ず同じものでなければいけません**。再現性が担保されていない場合、つまり環境が変わった影響で成果物が変更されてしまう可能性があります。

またデプロイするには成果物と設定ファイル（環境変数など）の２つを合わせて行うことになると思いますがこの設定ファイルだけ変更し、なにかしらの影響でデプロイが失敗した時、デプロイ環境が変わってしまう状況で **設定ファイルを変更したことが原因** なのかそれとも **デプロイ環境が変わってしまったことが原因** なのか判断することは簡単にできるでしょうか？
こういう状況では簡単に判断することは難しいのではないかと思います。そういった理由で **CircleCIが実行する環境は必ず常に同じもの** でなければいけません。

詳しくは以下のスライド資料でも書かれていますので読むことをオススメします。

- [インフラ自動化の落とし穴と宣言的アーキテクチャ](https://speakerdeck.com/nojima/inhurazi-dong-hua-falseluo-tosixue-toxuan-yan-de-akitekutiya?slide=30)

### 再現性を担保させるには？

**使用する docker のイメージは latest を使わず必ずバージョンを指定して設定** しましょう！
latest を指定するとどこかのタイミングでバージョンが違うものがダウンロードされてしまうことがあるため、latest の使用は避けることです。

latest を指定していたことで私も何回もこれで苦労しました……。

```yml
executors:
  base:
    docker:
      - image: cimg/ruby:2.6.6-node
```

CircleCIの設定での話ではないですが、 **プロダクトでは必ずライブラリなどはバージョンをちゃんと指定し一定の頻度でライブラリのアップデートをする機会を設けることをオススメ** します。

### 次世代のイメージを使うこと

CircleCI がメンテナンスしている Docker イメージは次世代のイメージを使うようにしましょう。
公式のドキュメントにも書かれていますが、**プレフィックスが circleci のレガシーイメージは 2021年12月31日に廃止** されるようです。

まだ古いレガシーイメージを使っている方は **プレフィックスが [cimg]((https://hub.docker.com/u/cimg)) の次世代イメージを使う** ようにしましょう！
移行ガイドについては公式のドキュメントにも書かれているのでそちらを参考にするとよいでしょう。

詳しくは公式のドキュメントを参考にするとよいでしょう。

- [Legacy Convenience Image Deprecation - Announcements - CircleCI Discuss](https://discuss.circleci.com/t/legacy-convenience-image-deprecation/41034/1)
- [Announcing our next-generation convenience images: smaller, faster, more deterministic - CircleCI](https://circleci.com/blog/announcing-our-next-generation-convenience-images-smaller-faster-more-deterministic/)

次世代のイメージの Dockerfile については github の Dockerfile を確認すると良いでしょう。

- [CircleCI Public cimg repositories](https://github.com/orgs/CircleCI-Public/repositories?language=&q=cimg&sort=&type=all)

#### 単純な移行方法

```
circleci/ruby:2.3.0 → cimg/ruby:2.3.0
circleci/python:3.8.4 → cimg/python:3.8.4
```

#### ブラウザテストのイメージの移行方法

[Browser Tool Orb](https://circleci.com/developer/orbs/orb/circleci/browser-tools) を使う必要があります。

```
-browsers → browsers + Browser Tool Orb
-browsers-legacy → browsers + Browser Tool Orb
-node-browsers- → browsers + Browser Tool Orb
-node-browsers-legacy → browsers + Browser Tool Orb
```

### Docker Hub のレート制限に対応する

現在は Docker Hub社と CircleCI は提携されていて基本的にはレート制限に引っかからないようになっているそうです。ただし、一部の使用方法によるとレート制限に引っかかってしまう場合があるので調べておくとよいでしょう。

また仕様が代わり、レート制限に対応する必要が出てくる可能性があるのでその時、どう対応したらいいのか雰囲気理解しておくことをオススメします。

詳しくは以下のFAQを自分たちのユースケースにおいては大丈夫なのかさらっと見ておくと良いでしょう！

- [Docker Hubのレート制限に関するFAQ](https://support.circleci.com/hc/ja/articles/360050623311-Docker-Hub%E3%81%AE%E3%83%AC%E3%83%BC%E3%83%88%E5%88%B6%E9%99%90%E3%81%AB%E9%96%A2%E3%81%99%E3%82%8BFAQ)

## キャッシュを使う

CircleCI ではキャッシュ的な機能が大きく2つあるのでそれぞれ有効に使っていきましょう。

### JOB単位のキャッシュ

JOB 単位でキャッシュを行うことができます。次回以降のワークフローの実行でキャッシュされたものが使われるようになるので CircleCI の高速化ができます。

![caching-dependencies-overview](https://circleci.com/docs/assets/img/docs/caching-dependencies-overview.png)
画像は [公式ドキュメント - 依存関係のキャッシュ](https://circleci.com/docs/ja/2.0/caching/) より

command で **`restore_cache`, `save_cache` キーワードを使うことでキャッシュを使うことができる** ようになります。ただ、自分で実装するのはめんどくさいですよね。なんと最近の CircleCI では何も考えずに使用することができます。
Orb に実装されていることが多いので **Orb に実装されている command を使用しましょう**。

[circleci/ruby@1.2.0 の install-deps コマンド](https://circleci.com/developer/ja/orbs/orb/circleci/ruby#commands-install-deps)でキャッシュ機能が使われている箇所を見てみましょう！

```yml
steps:
  - when:
      condition: <<parameters.with-cache>>
      steps:
        - restore_cache:
            keys:
              - >-
                << parameters.key >>-{{ checksum
                "<<parameters.app-dir>>/Gemfile.lock"  }}-{{ .Branch }}
              - >-
                << parameters.key >>-{{ checksum
                "<<parameters.app-dir>>/Gemfile.lock"  }}
              - << parameters.key >>
  - run:
      command: |
        if test -f "Gemfile.lock"; then
          APP_BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")
          if [ -z "$APP_BUNDLER_VERSION" ]; then
            echo "Could not find bundler version from Gemfile.lock. Please use bundler-version parameter"
          else
            echo "Gemfile.lock is bundled with bundler version $APP_BUNDLER_VERSION"
          fi
        fi

        if ! [ -z <<parameters.bundler-version>> ]; then
          echo "Found bundler-version parameter to override"
          APP_BUNDLER_VERSION=<<parameters.bundler-version>>
        fi

        if ! echo $(bundle version)  | grep -q $APP_BUNDLER_VERSION; then
          echo "Installing bundler $APP_BUNDLER_VERSION"
          gem install bundler:$APP_BUNDLER_VERSION
        else
          echo "bundler $APP_BUNDLER_VERSION is already installed."
        fi

        if [ "<< parameters.path >>" == "./vendor/bundle" ]; then
          bundle config set deployment 'true'
        fi
        bundle config set path << parameters.path >>
        bundle check || bundle install
      name: >-
        Bundle Install <<^parameters.with-cache>>(No
        Cache)<</parameters.with-cache>>
      working_directory: <<parameters.app-dir>>
  - when:
      condition: <<parameters.with-cache>>
      steps:
        - save_cache:
            key: >-
              << parameters.key >>-{{ checksum
              "<<parameters.app-dir>>/Gemfile.lock"  }}-{{ .Branch }}
            paths:
              - <<parameters.app-dir>>/<< parameters.path >>
```

ちゃんと `restore_cache`, `save_cache` キーワードが使われていますね。
下記の部分がキャッシュがどの状態の時に保存、復元するのかの戦略方法になります。`{{}}` で囲まれている部分が CircleCI で用意されているテンプレート（どの状態でキャッシュをするのか）になっています。
この例だと「Gemfile.lockの内容」 と 「ブランチ」の組み合わせが変わるごとにキャッシュを保存する設定になります。

```shell
<< parameters.key >>-{{ checksum "<<parameters.app-dir>>/Gemfile.lock"  }}-{{ .Branch }}
```

詳しくはCircleCIのドキュメントの [キーとテンプレートの使用](https://circleci.com/docs/ja/2.0/caching/#using-keys-and-templates)を見てみてください。
もし自前で実装する必要がある場合はドキュメントの [依存関係のキャッシュ](https://circleci.com/docs/ja/2.0/caching/) を確認してみてください。

#### キャッシュをクリアしたい場合

キャッシュは最長で15日間保持されるため、明示的にクリアしたい時はプレフィックスで使用されている key を変更することで可能になります。
下記の部分だと `<< parameters.key >>` の部分にあたります。`v1` などとつけることが多いため、ここをカウントアップさせて `v2`, `v3`…… としていくことでキャッシュをクリアできます。

```shell
<< parameters.key >>-{{ checksum "<<parameters.app-dir>>/Gemfile.lock"  }}-{{ .Branch }}
```

[circleci/ruby@1.2.0 の install-deps コマンド](https://circleci.com/developer/ja/orbs/orb/circleci/ruby#commands-install-deps)でもパラメータで key を指定することができるようになっています。

![08_cache_key](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/08_cache_key.png)

### workflow 単位でデータを共有する

job 単位のキャッシュでは横へのキャッシュでしたが、今回は縦単位のキャッシュでworkflow 間でデータの共有（キャッシュとは違うかもしれませんが……）を行うことができます。
WORKSPACE というところにデータを保存しそれを job の実行時に読み込むことで job 間でデータの共有を行うことができるようになっています。
job 間で同じ資材を使う場合はそれを使い回すことができるようになり、CircleCIの高速化に繋がります。

![workspaces](https://circleci.com/docs/assets/img/docs/workspaces.png)
画像は [公式ドキュメント - ワークスペースによるジョブ間のデータ共有](https://circleci.com/docs/ja/2.0/workflows/#using-workspaces-to-share-data-among-jobs) より

この機能を使うことで仮に linter と test で job が分かれている場合、予め依存ライブラリのインストールが必要になると思いますが、最初の job でインストール後、次の job にインストールした依存ライブラリを共有することで再インストールが必要なくなります。

#### 実際に使用してみる

これは WORKSPACE 用のコマンドを用意しそれをそれぞれ、job ごと使用するようにしています。
setup という job で WORKSPACE を保存し、lint, test の job で保存した WORKSPACE を使うようにしています。イメージを掴むためのコードでいろいろと省略しています。

```yml
commands:
  save-workspace:
    steps:
      - persist_to_workspace:
          # 絶対パスまたは working_directory からの相対パスを指定する必要があります。
          # WORKSPACE のルートディレクトリの設定です。
          root: .
          # ルート（上で設定した）からの相対パスを指定する必要があります。
          # どこのディレクトリを WORKSPACE に保存するかの設定です。
          paths: .
  using-workspace:
    steps:
      - attach_workspace:
          # 絶対パスまたは working_directory からの相対パスを指定する必要があります。
          # 保存した WORKSPACE のファイルをどこに展開するかの設定です。
          at: .

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - ruby/install-deps
      # この場合はソースコードを clone してきて、依存関係をインストールしたものを WORKSPACE に保存しています。
      - save-workspace
  lint:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - ruby/rubocop-check:
          label: Rubocop
  test:
    executor: base
    parallelism: 2
    steps:
      - using-workspace
      - ruby/install-deps
      - ruby/rspec-test:
          label: Run all RSpec
      - store-coverage
```

### Docker レイヤーキャッシュの有効化

Docker レイヤーキャッシュは Docker のイメージのビルドが CI/CD のプロセスの一環として定期的に行われる場合に効果を発揮する機能になります。
詳しくはドキュメントを確認してみてください。

- [ocker レイヤー キャッシュの有効化](https://circleci.com/docs/ja/2.0/docker-layer-caching/)

## 並列実行を使う

並列に実行できるものに関しては並列に実行し CI/CD をより早くすることで開発速度をあげていくことができます。
ここでは並列に実行するための Tips を考えていきましょう。

### テストの並列実行（executorを複数にして実行させる）

CircleCI にはテストを並列で実行するための機能が備わっています。
こちらの並列実行の機能を使うことでテストを早く終わらせることができるようになります。

job を実行させるために executor を指定すると思いますが、こちらの executor を複数立ち上げて分割したテストを実行させることで並列実行をできるようにします。

![test_splitting](https://circleci.com/docs/assets/img/docs/test_splitting.png)
画像は [公式ドキュメント - テストの並列実行](https://circleci.com/docs/ja/2.0/parallelism-faster-jobs/) より

ただし、予め並列実行させるために準備が必要です。それを見ていきましょう。

#### タイミングデータを作成する

CircleCI ではタイミングデータを使用してテストを分割することが最良の方法だと記載されています。
なのでテストの分割を最良の方法で行うためにタイミングデータを作成するようにします。

[store_test_results](https://circleci.com/docs/ja/2.0/configuration-reference/#storetestresults) を指定することでテストのメタデータを作成するようにします。こうすることで、test の job で CircleCI 上でテスト結果を詳細に確認することができるようになります。このメタデータを作成することでタイミングデータも作成されます。

ただし、JUnit フォーマッタを有効化しないと CircleCI では自動的に収集してくれないのでフォーマッタの有効化も必要になります。
詳しくは以下のドキュメントを参照してください。

- [テスト メタデータの収集](https://circleci.com/docs/ja/2.0/collect-test-data/)

**job の詳細画面**

![09_test_section](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/09_test_section.png)

**Test insights画面**

![09_test_insights](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/09_test_insights.png)

#### テストの分割と executor の数を指定する

テストのメタデータを保存することができたら後は test のコマンドを変更していきます。
ruby で RSpec で行う場合は以下のようになるかと思います。サンプルなのでいろいろと省略してあります。

- `circleci tests glob` コマンドを使用してファイルの一覧を取得しそれを `circleci tests split --split-by=timings` コマンドでタイミングデータで分割する
- 分割したテストを実行させる
- job で parallelism のキーワードを使用して executor の並列実行させる数を指定する

```yml
commands:
  rspec-test:
    steps:
    - run:
        # テストの格納フォルダを作成し指定する
        # タイミングデータを元にテストの分割を行う
        # タイミングデータで分割したテストを実行する
        command: >
            mkdir -p /tmp/test-results/rspec

            TESTFILES=$(circleci tests glob "spec/**/*_spec.rb" | circleci tests
            split --split-by=timings)

            bundle exec rspec $TESTFILES --profile 10 --format RspecJunitFormatter
            --out /tmp/test-results/rspec/results.xml --format progress
    - store_test_results:
        path: /tmp/test-results/rspec

jobs:
  test:
    executor: base
    # ここで executor の数を指定する
    parallelism: 2
    steps:
      - using-workspace
      - ruby/install-deps
      - rspec-test:
          label: Run all RSpec
      - store-coverage
```

これで並列実行されるようになります。

![10_parallelism_test](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/10_parallelism_test.png)

#### orb のテストコマンドを使用する

実はこれらの機能は orb にすでに実装されているケースが多いです。なので特に考えずに orb を使うとよいでしょう。

![11_ruby_orb_parallelism_test](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/11_ruby_orb_parallelism_test.png)

### テストの並列実行（テストをjob単位で複数実行させる）

もう一つの並列実行させる仕組みはテストを job 単位で分割して実行させる方法です。

例えば Rails なら spec フォルダ内のフォルダごと分割されているテストをいくつかのグループに分けて実行させる方法です。イメージとしては テストの job が複数ある感じです。

![12_rspec_split_for_job](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/12_rspec_split_for_job.png)

## テスト結果をアップロードする

テスト結果（カバレッジなど）はアップロードし CircleCI で確認できるようにしましょう。
テスト結果を見れる状態にしておくことでカバレッジを確認できるようになるのでテストを書くモチベーションも上がります！

**ARTIFACTSタブ**

![21_artifacts_coverage_results](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/21_artifacts_coverage_results.png)

**ARTIFACTSにアップロードされたindex.html**

![21_artifacts_coverage_results_index](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/21_artifacts_coverage_results_index.png)

### 実装する

CircleCI のドキュメントに言語ごとのコードカバレッジの実装方法が記載されていますのでこちらを参考にすることで簡単に実装ができます。
ドキュメントを確認すると `store_artifacts` のキーワードを使用していることがわかります。

- [コードカバレッジ メトリクスの生成](https://circleci.com/docs/ja/2.0/code-coverage/)

### store_artifacts と store_test_results の使い分け

今回はどちらもテストの結果を確認できる用途で使いましょうと説明しましたが [store_artifacts](https://circleci.com/docs/ja/2.0/configuration-reference/#storeartifacts) に関しては別にテスト結果に限った話ではなく、何でも格納可能になっています。
しかし [store_test_results](https://circleci.com/docs/ja/2.0/configuration-reference/#storetestresults) に関してはテスト結果専用で使用するものとなっているので [store_artifacts](https://circleci.com/docs/ja/2.0/configuration-reference/#storeartifacts) とは少し違います。

なぜテスト結果を格納するのに両方使うのかと言うと、[store_test_results](https://circleci.com/docs/ja/2.0/configuration-reference/#storetestresults) は CircleCI に最適化されたテストの結果が表示されるようになりますが [store_artifacts](https://circleci.com/docs/ja/2.0/configuration-reference/#storeartifacts) の場合はその言語のライブラリの出力結果を見ることができるようになるので確認できる内容は全然、違うものになっています。
Ruby の場合だと [SimpleCov](https://github.com/simplecov-ruby/simplecov) を使うと思いますが、こちらはコードのカバレッジ結果を確認できますが、[store_test_results](https://circleci.com/docs/ja/2.0/configuration-reference/#storetestresults) はカバレッジ結果は見れませんが、それぞれのテストの実行時間を簡単に確認できるようになり、さらにテストの並列実行に必要なタイミングデータも作成してくれるので全く違う用途で使うことになります。

## 基本的には orb に寄せる

言語ごとそれに適した orb があるのでそれを活用していくことをオススメします。

### orb を使用する利点とは？

自前で実装するよりも **config.yml の行数が圧倒的に減り**、見やすくなります。
キャッシュや並列実行でも触れましたが、orb の実装ではほぼ **キャッシュや並列実行が簡単にできるように実装されているケースが多い** のでわざわざ自分で実装せずに circleci が公開している orb に寄せて管理は circleci に任せると良いでしょう。

### orb を使用する時の注意点

orb が登場してから使用してきましたが、私の間隔だとバージョンアップがよくされているように感じます。なので半年前に実装した orb と中身が結構違うケースがあります。
orb のバージョンがアップすると、 **command や job の名前が変わったりする** のでバージョンアップ時には注意が必要です。

またよくわからず使用することはオススメしません。
基本的に orb に寄せるで良いと思っていますが、**ちゃんと使用する command, job の処理の中身を確認** して自分のプロダクトにあった処理になっているか確かめてから使用するとよいと思います。

### orb の使用方法

orb のドキュメントページには必ず example のコードが記載されています。なのでその example のコードをよしなに変更にして使うようにしましょう。

![14_orb_usage_example](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/14_orb_usage_example.png)

## 環境変数の設定の Tips

CircleCI で使用する環境変数の Tips を紹介していきます。

### Organization で共通で使用する場合は Contexts を使う

CircleCI の設定で Organization で環境変数を扱う場合は、Contexts を使用して共通で管理できるようにしましょう。
slack 通知で必要な アクセストークンやチャンネルIDなどを設定するとよいと思います。

#### 使用方法

workflow で設定する job に対して contexts を使用するように指定します。`context: slack` の感じで指定することで環境変数を読み込むことができるようになります。

```yml
workflows:
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - slack/on-hold:
          name: slack_on_hold
          channel: $SLACK_CHANNEL
          # ここで contexts を設定しています
          context: slack
          custom: |
            {
              "blocks": [
                {
                  "type": "header",
                  "text": {
                    "type": "plain_text",
                    "text": "ON HOLD - Awaiting Approval :raised_hand:",
                    "emoji": true
                  }
                },
                {
                  "type": "section",
                  "fields": [
                    {
                      "type": "mrkdwn",
                      "text": "*Project*:\n$CIRCLE_PROJECT_REPONAME"
                    },
                    {
                      "type": "mrkdwn",
                      "text": "*Branch*:\n$CIRCLE_BRANCH"
                    }
                  ]
                },
                {
                  "type": "actions",
                  "elements": [
                    {
                      "type": "button",
                      "text": {
                        "type": "plain_text",
                        "text": "View Workflow"
                      },
                      "url": "https://circleci.com/workflow-run/${CIRCLE_WORKFLOW_ID}"
                    }
                  ]
                }
              ]
            }
          requires:
            - lint
```

#### 設定画面について

設定画面は以下のような感じになっています。

**Contexts 画面**

![15_environment_contexts](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/15_environment_contexts.png)

**Contexts 詳細画面**

![15_environment_contexts_details](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/15_environment_contexts_details.png)

### 複数行の環境変数を扱う場合

`.env` ファイルなどの複数行の環境変数を一括で１つの環境変数として使用し、ビルド時にその環境変数を `.env` ファイルなどに展開して使用する方法です。

#### 複数行の環境変数のファイルを base64 で暗号化する

bash64 コマンドを使用して .env ファイルの値を encode します。

```
$ cat .env | base64
qwertyuiopasdfghjklzxcvbnm
```

出力された値を環境変数に設定します。

#### 環境変数から .env ファイルを生成する

今度は逆で環境変数の値を decode しその値から .env ファイルを生成します。

```yml
commands:
  setup_env_file:
    steps:
      - run:
          name: Create .env file
          command: |
            echo $MULTIPLE_ENV_VALUE | base64 --decode > ./.env
```

こちらの方法は [CircleCI実践入門──CI/CDがもたらす開発速度と品質の両立](https://www.amazon.co.jp/CircleCI%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80%E2%94%80%E2%94%80CI-CD%E3%81%8C%E3%82%82%E3%81%9F%E3%82%89%E3%81%99%E9%96%8B%E7%99%BA%E9%80%9F%E5%BA%A6%E3%81%A8%E5%93%81%E8%B3%AA%E3%81%AE%E4%B8%A1%E7%AB%8B-WEB-PRESS-plus/dp/4297114119) の書籍に紹介されていた方法になります。
こちらの書籍は CircleCI について網羅的に書かれているのですごくオススメです！

## slack 通知を実装する

![19_slack_notification_sample](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/19_slack_notification_sample.png)

デプロイが完了したり、デプロイの準備が整ったら slack に通知されて欲しいかと思います。
ここでは slack に CircleCI の通知を行う方法を記載していきます。

公式が [slack の orb ](https://circleci.com/developer/ja/orbs/orb/circleci/slack) を提供してくれているのでこちらを使用して実装します。

### slack から通知できるように設定を行う

slack の orb の github のページに Setup 方法が記載されているのでこれの通りに slack に設定を行い、CircleCI にも環境変数の設定を行います。

- [Setup · CircleCI-Public/slack-orb Wiki](https://github.com/CircleCI-Public/slack-orb/wiki/Setup)

Wiki に書かれている通り、以下の手順をそれぞれなぞって設定します。

1. Create a Slack App
2. Permissions
3. Install and Receive Token
4. Create a Context on CircleCI

#### チャンネルIDはどうやって知ればいいの？

チャンネルを右クリックしリンクコピーすると、`https://xxxxxxxxx.slack.com/archives/XXXXXXXXXXXX` のような形でコピーされるのでその末尾の `XXXXXXXXXXXX` がチャンネルIDになります。

![17_slack_channel_id_copy](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/17_slack_channel_id_copy.png)

### 通知処理を実装

設定さえ、ちゃんできていれば slack の orb に載っている `only_notify_on_branch` をそのまま使用することで通知が可能になります。

- [circleci/slack - only_notify_on_branch](https://circleci.com/developer/ja/orbs/orb/circleci/slack#usage-only_notify_on_branch)

```yml
version: '2.1'
orbs:
  slack: circleci/slack@4.1
jobs:
  build:
    machine: true
    steps:
      - run: some build command
      - slack/notify:
          branch_pattern: main
          event: fail
          template: basic_fail_1
      - slack/notify:
          branch_pattern: production
          event: fail
          mentions: '<@U8XXXXXXX>, @UserName'
          template: basic_fail_1
workflows:
  deploy_and_notify:
    jobs:
      - build:
          context: slack-secrets
      - deploy:
          filters:
            branches:
              only:
                - production
          requires:
            - build
```

### デプロイ前に承認処理を実装する

デプロイ前に承認処理を追加し、`Approve` ボタンをクリックしないと後続の job が実行されない仕組みを実装しましょう。
この処理を挟むことでデプロイ内容を予めチェックし問題なければ、そのままデプロイすることが可能になります。いわゆる安全装置です。

**承認処理 job 押下前**

![18_slack_before_on_hold](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/18_slack_before_on_hold.png)

**承認処理 job 押下後**

![18_slack_after_on_hold](https://raw.githubusercontent.com/dodonki1223/image_garage/master/articles/19/18_slack_after_on_hold.png)

こちらの処理も slack の orb に載っている `on_hold_notification` を使用することで実装することが可能です。

- [circleci/slack - on_hold_notification](https://circleci.com/developer/ja/orbs/orb/circleci/slack#usage-on_hold_notification)

```yml
version: '2.1'
orbs:
  slack: circleci/slack@4.1
workflows:
  on-hold-example:
    jobs:
      - my_test_job
      - slack/on-hold:
          context: slack-secrets
          requires:
            - my_test_job
      - pause_workflow:
          requires:
            - my_test_job
            - slack/on-hold
          type: approval
      - my_deploy_job:
          requires:
            - pause_workflow
```

### slack の通知内容をカスタマイズする

通知処理と承認処理の両方を実装することができたら、後はお好みでカスタマイズしていきます。
slack 通知する内容は [テンプレート](https://github.com/CircleCI-Public/slack-orb#templates) が用意してあってそれを使用して通知してします。

自分でカスタマイズする場合は `custom` パラメータを適宜変更してそれを実装するようにします。

```yml
- slack/notify:
    event: always
    custom: |
      {
        "blocks": [
          {
            "type": "section",
            "fields": [
              {
                "type": "plain_text",
                "text": "*This is a text notification*",
                "emoji": true
              }
            ]
          }
        ]
      }
```

細かい表示の変更をする時は slack の [Block Kit Builder](https://app.slack.com/block-kit-builder/T7E3ZNL8P#%7B%22blocks%22:%5B%7B%22type%22:%22section%22,%22text%22:%7B%22type%22:%22mrkdwn%22,%22text%22:%22Hello,%20Assistant%20to%20the%20Regional%20Manager%20Dwight!%20*Michael%20Scott*%20wants%20to%20know%20where%20you'd%20like%20to%20take%20the%20Paper%20Company%20investors%20to%20dinner%20tonight.%5Cn%5Cn%20*Please%20select%20a%20restaurant:*%22%7D%7D,%7B%22type%22:%22divider%22%7D,%7B%22type%22:%22section%22,%22text%22:%7B%22type%22:%22mrkdwn%22,%22text%22:%22*Farmhouse%20Thai%20Cuisine*%5Cn:star::star::star::star:%201528%20reviews%5Cn%20They%20do%20have%20some%20vegan%20options,%20like%20the%20roti%20and%20curry,%20plus%20they%20have%20a%20ton%20of%20salad%20stuff%20and%20noodles%20can%20be%20ordered%20without%20meat!!%20They%20have%20something%20for%20everyone%20here%22%7D,%22accessory%22:%7B%22type%22:%22image%22,%22image_url%22:%22https://s3-media3.fl.yelpcdn.com/bphoto/c7ed05m9lC2EmA3Aruue7A/o.jpg%22,%22alt_text%22:%22alt%20text%20for%20image%22%7D%7D,%7B%22type%22:%22section%22,%22text%22:%7B%22type%22:%22mrkdwn%22,%22text%22:%22*Kin%20Khao*%5Cn:star::star::star::star:%201638%20reviews%5Cn%20The%20sticky%20rice%20also%20goes%20wonderfully%20with%20the%20caramelized%20pork%20belly,%20which%20is%20absolutely%20melt-in-your-mouth%20and%20so%20soft.%22%7D,%22accessory%22:%7B%22type%22:%22image%22,%22image_url%22:%22https://s3-media2.fl.yelpcdn.com/bphoto/korel-1YjNtFtJlMTaC26A/o.jpg%22,%22alt_text%22:%22alt%20text%20for%20image%22%7D%7D,%7B%22type%22:%22section%22,%22text%22:%7B%22type%22:%22mrkdwn%22,%22text%22:%22*Ler%20Ros*%5Cn:star::star::star::star:%202082%20reviews%5Cn%20I%20would%20really%20recommend%20the%20%20Yum%20Koh%20Moo%20Yang%20-%20Spicy%20lime%20dressing%20and%20roasted%20quick%20marinated%20pork%20shoulder,%20basil%20leaves,%20chili%20&%20rice%20powder.%22%7D,%22accessory%22:%7B%22type%22:%22image%22,%22image_url%22:%22https://s3-media2.fl.yelpcdn.com/bphoto/DawwNigKJ2ckPeDeDM7jAg/o.jpg%22,%22alt_text%22:%22alt%20text%20for%20image%22%7D%7D,%7B%22type%22:%22divider%22%7D,%7B%22type%22:%22actions%22,%22elements%22:%5B%7B%22type%22:%22button%22,%22text%22:%7B%22type%22:%22plain_text%22,%22text%22:%22Farmhouse%22,%22emoji%22:true%7D,%22value%22:%22click_me_123%22%7D,%7B%22type%22:%22button%22,%22text%22:%7B%22type%22:%22plain_text%22,%22text%22:%22Kin%20Khao%22,%22emoji%22:true%7D,%22value%22:%22click_me_123%22,%22url%22:%22https://google.com%22%7D,%7B%22type%22:%22button%22,%22text%22:%7B%22type%22:%22plain_text%22,%22text%22:%22Ler%20Ros%22,%22emoji%22:true%7D,%22value%22:%22click_me_123%22,%22url%22:%22https://google.com%22%7D%5D%7D%5D%7D) を使用して自分が表示して欲しい形式を作成してから config.yml に落とし込むのが良いでしょう。

#### circleci が定義している環境変数を使う

CircleCI が持っている[定義済みの環境変数](https://circleci.com/docs/ja/2.0/env-vars/#built-in-environment-variables)を使用することでより slack の通知を使いやすくすることができます。

例えば slack 通知に job の URL へ遷移するボタンは `CIRCLE_BUILD_URL` という環境変数を使用し以下のように設定すると使えるようになります。

```json
{
  "type": "actions",
  "elements": [
    {
      "type": "button",
      "text": {
        "type": "plain_text",
        "text": "View Job"
      },
      "url": "${CIRCLE_BUILD_URL}"
    }
  ]
}
```

プルリクエストを作成したユーザー名は `CIRCLE_PR_USERNAME` という環境変数で取得できたりします。
お好みの環境変数を使用して自分好みの slack 通知を作成すると良いでしょう。

# 最後に

私が3年ぐらい、業務や個人で CircleCI を使用し溜めてきた Tips やいつも必ず設計することや効率的な開発方法について述べてみました。
皆さんのオススメの開発方法などありましたら教えてください！
