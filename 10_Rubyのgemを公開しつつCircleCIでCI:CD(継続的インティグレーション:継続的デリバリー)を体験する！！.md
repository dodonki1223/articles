# Rubyのgemを公開しつつCircleCIでCI/CD(継続的インティグレーション/継続的デリバリー)を体験しよう

Rubyを触り始めて半年以上経ちました……

半年以上経った記念として[qiita_trend（Qiitaのトレンドをたったの10秒で取得できるgem）](https://rubygems.org/gems/qiita_trend/)を作成し公開しました。この記事は[qiita_trend](https://rubygems.org/gems/qiita_trend/)を作る過程で手に入れた知見をまとめたものになります

[qiita_trend](https://rubygems.org/gems/qiita_trend/)のgemを使用してSlackにQiitaのトレンドを通知するスクリプトを作成したのでもし良かったら使ってください

[dodonki1223/qiita_trend_slack_notifier: qiita_trendを使用したslack通知スクリプト](https://github.com/dodonki1223/qiita_trend_slack_notifier)

![qiita_trend_slack_notifier](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/cff78b05-dfcd-849d-595d-2e18b5a1abb7.png)

gemを一から作成する方法、CI/CD(継続的インティグレーション/継続的デリバリー)を体験することを目的とした記事になります

具体的にはgemを作成しそのgemのブランチのコードに変更があるたび、CircleCIが実行され、[RuboCop（静的解析ツール）](https://github.com/rubocop-hq/rubocop)、[RSpec（テスト）](https://github.com/rspec/rspec)が実行されるようになります

masterブランチに限っては[RuboCop（静的解析ツール）](https://github.com/rubocop-hq/rubocop)、[RSpec（テスト）](https://github.com/rspec/rspec)が成功したら[RubyGems](https://rubygems.org)に自動デプロイする

また失敗した時はデプロイされない状態を作っていきます

# gemを公開する

CircleCIの凄さを確認するためにまずはgemを公開します
gemの公開方法についてはいろいろな方が記事にしています

- [【Ruby】gemの作り方から公開まで - Qiita](https://qiita.com/9sako6/items/72994b8b1c00af4e61fe)
- [RubyGemはめっちゃ簡単に作れる！　 | 酒と涙とRubyとRailsと](https://morizyun.github.io/blog/ruby-gem-easy-publish-library-rails/index.html)

上記記事も参考にしつつgemを作成していきます

## gemの雛形を作成する

### 前準備

前準備として`gem`のアップデート、`Bundler`のアップデートを行う

gem自体をアップデートする

```shell
$ gem update --system
Latest version already installed. Done.
```

Bundler自体をインストールする（インストールしていない場合）

```shell
$ gem install bundler
Successfully installed bundler-2.0.2
Parsing documentation for bundler-2.0.2
Done installing documentation for bundler after 2 seconds
1 gem installed
```

Bundlerのアップデート

```shell
$ gem update bundler
Updating installed gems
Nothing to update
```

### gemの雛形を作成する

今回は`dodonki_sample`というgemを作成していきます。`-t`オプションはテスト関連のファイルを作成してくれます。

```shell
$ bundle gem dodonki_sample -t
Creating gem 'dodonki_sample'...
      create  dodonki_sample/Gemfile
      create  dodonki_sample/lib/dodonki_sample.rb
      create  dodonki_sample/lib/dodonki_sample/version.rb
      create  dodonki_sample/dodonki_sample.gemspec
      create  dodonki_sample/Rakefile
      create  dodonki_sample/README.md
      create  dodonki_sample/bin/console
      create  dodonki_sample/bin/setup
      create  dodonki_sample/.gitignore
      create  dodonki_sample/.travis.yml
      create  dodonki_sample/.rspec
      create  dodonki_sample/spec/spec_helper.rb
      create  dodonki_sample/spec/dodonki_sample_spec.rb
Initializing git repo in /Users/dodonki/Project/dodonki_sample
Gem 'dodonki_sample' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
```

`-t`オプションを指定しないで実行した場合は下記の結果になります

```shell
 bundle gem dodonki_sample
Creating gem 'dodonki_sample'...
      create  dodonki_sample/Gemfile
      create  dodonki_sample/lib/dodonki_sample.rb
      create  dodonki_sample/lib/dodonki_sample/version.rb
      create  dodonki_sample/dodonki_sample.gemspec
      create  dodonki_sample/Rakefile
      create  dodonki_sample/README.md
      create  dodonki_sample/bin/console
      create  dodonki_sample/bin/setup
      create  dodonki_sample/.gitignore
Initializing git repo in /Users/dodonki/Project/dodonki_sample
Gem 'dodonki_sample' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
```

今回はRSpecを実行してテストを自動化するので`-t`オプションを付けてください

#### 作成されるファイルについて

詳しくはBundlerの[ドキュメント](https://bundler.io/v2.0/guides/creating_gem.html#getting-started)を確認してください

#### gem名について

gem名は既存のgemとかぶらないように[RubyGems](https://rubygems.org)と`gem search`コマンドを使用して確認しましょう

**RubyGemsにて検索**

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/1520f58e-827a-5680-deb9-c2b96c3c02f2.png)

**gem searchコマンド検索**

```shell
$ gem search dodonki_sample

*** REMOTE GEMS ***
```

[【Ruby】gemの作り方から公開まで](https://qiita.com/9sako6/items/72994b8b1c00af4e61fe)の記事の[gemの名付けにおける注意1](https://qiita.com/9sako6/items/72994b8b1c00af4e61fe#gemの名付けにおける注意1)を参考にさせていただきました

#### gem名で使用する-(ハイフン)と_(アンダーバー)の違い

**-(ハイフン)で作成した場合**

```shell
$ bundle gem dodonki-sample -t
Creating gem 'dodonki-sample'...
      create  dodonki-sample/Gemfile
      create  dodonki-sample/lib/dodonki/sample.rb
      create  dodonki-sample/lib/dodonki/sample/version.rb
      create  dodonki-sample/dodonki-sample.gemspec
      create  dodonki-sample/Rakefile
      create  dodonki-sample/README.md
      create  dodonki-sample/bin/console
      create  dodonki-sample/bin/setup
      create  dodonki-sample/.gitignore
      create  dodonki-sample/.travis.yml
      create  dodonki-sample/.rspec
      create  dodonki-sample/spec/spec_helper.rb
      create  dodonki-sample/spec/dodonki/sample_spec.rb
Initializing git repo in /Users/dodonki/Project/dodonki-sample
Gem 'dodonki-sample' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
```

**_(アンダーバー)で作成した場合**

```shell
$ bundle gem dodonki_sample -t
Creating gem 'dodonki_sample'...
      create  dodonki_sample/Gemfile
      create  dodonki_sample/lib/dodonki_sample.rb
      create  dodonki_sample/lib/dodonki_sample/version.rb
      create  dodonki_sample/dodonki_sample.gemspec
      create  dodonki_sample/Rakefile
      create  dodonki_sample/README.md
      create  dodonki_sample/bin/console
      create  dodonki_sample/bin/setup
      create  dodonki_sample/.gitignore
      create  dodonki_sample/.travis.yml
      create  dodonki_sample/.rspec
      create  dodonki_sample/spec/spec_helper.rb
      create  dodonki_sample/spec/dodonki_sample_spec.rb
Initializing git repo in /Users/dodonki/Project/dodonki_sample
Gem 'dodonki_sample' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
```

作成されるディレクトリ構造が違うことに気づくでしょう
これはrequireでgemを呼び出す時の呼び出し方法が変わることを意味しています

RubyGemsの[ドキュメント](https://guides.rubygems.org/name-your-gem/)よりgem名とrequiereの関係とクラスとモジュールの関係については以下のようになるようです

|  GEM NAME            | REQUIRE STATEMENT               | MAIN CLASS OR MODULE  |
|:---------------------|:--------------------------------|:---------------------:|
|  ruby_parser         |  require 'ruby_parser'          | RubyParser            |
|  rdoc-data           |  require 'rdoc/data'            | RDoc::Data            |
|  net-http-persistent |  require 'net/http/persistent'  | Net::HTTP::Persistent |
|  net-http-persistent |  require 'net/http/persistent'  | Net::HTTP::Persistent |


[【Ruby】gemの作り方から公開まで](https://qiita.com/9sako6/items/72994b8b1c00af4e61fe)の記事の[gemの名付けにおける注意2](https://qiita.com/9sako6/items/72994b8b1c00af4e61fe#gemの名付けにおける注意2)を参考にさせていただきました

## .gemspecファイルの変更

ディレクトリ直下にある`gem名.gemspec`ファイルを編集します。基本的には`TODO`と書かれている箇所の修正を行います。今回は詳しいことはあまり説明しません。

修正前ソース

```ruby
lib = File.expand_path("lib", __dir__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require "dodonki_sample/version"

Gem::Specification.new do |spec|
  spec.name          = "dodonki_sample"
  spec.version       = DodonkiSample::VERSION
  spec.authors       = ["dodonki1223"]
  spec.email         = ["自分のemailが設定されています"]

  spec.summary       = %q{TODO: Write a short summary, because RubyGems requires one.}
  spec.description   = %q{TODO: Write a longer description or delete this line.}
  spec.homepage      = "TODO: Put your gem's website or public repo URL here."

  spec.metadata["allowed_push_host"] = "TODO: Set to 'http://mygemserver.com'"

  spec.metadata["homepage_uri"] = spec.homepage
  spec.metadata["source_code_uri"] = "TODO: Put your gem's public repo URL here."
  spec.metadata["changelog_uri"] = "TODO: Put your gem's CHANGELOG.md URL here."

  # Specify which files should be added to the gem when it is released.
  # The `git ls-files -z` loads the files in the RubyGem that have been added into git.
  spec.files         = Dir.chdir(File.expand_path('..', __FILE__)) do
    `git ls-files -z`.split("\x0").reject { |f| f.match(%r{^(test|spec|features)/}) }
  end
  spec.bindir        = "exe"
  spec.executables   = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
  spec.require_paths = ["lib"]

  spec.add_development_dependency "bundler", "~> 2.0"
  spec.add_development_dependency "rake", "~> 10.0"
  spec.add_development_dependency "rspec", "~> 3.0"
end

```

### gemの説明情報をセット

僕の作成した[qiita_trend](https://github.com/dodonki1223/qiita_trend)の場合は下記のようになっています

```ruby
  spec.summary       = 'Easy to get trend for Qiita in 10 seconds'
  spec.description   = 'Easy to get trend for Qiita in 10 seconds'
  spec.homepage      = 'https://github.com/dodonki1223/qiita_trend'
```

### 必要のない箇所をコメントアウト

下記のTODOの部分はコメントアウトします。gemの追加情報なのでコメントアウトしてしまっても問題ありません

詳しくは[公式ドキュメント](https://guides.rubygems.org/specification-reference/)を確認してください


```ruby
  # spec.metadata["allowed_push_host"] = "TODO: Set to 'http://mygemserver.com'"

  # spec.metadata["homepage_uri"] = spec.homepage
  # spec.metadata["source_code_uri"] = "TODO: Put your gem's public repo URL here."
  # spec.metadata["changelog_uri"] = "TODO: Put your gem's CHANGELOG.md URL here."
```

### ライセンス情報をセット

spec.homepageの下にライセンス情報を追加します。
MITライセンスについては[こちら](https://ja.wikipedia.org/wiki/MIT_License)を確認ください

```ruby
  spec.license       = 'MIT'
```

### gem内で使用するgemを追加する

追加形式は下記のようになっています

```ruby
# プログラム内で使用するgemの場合は
spec.add_dependency 'gem名'

# 開発時に使用するgemの場合
spec.add_development_dependency 'gem名'
```

今回はCircleCIでCIの凄さを試すために下記gemを追加します

- [RSpec JUnit Formatter（JUnit形式でテスト結果を出力してくれるツール）](https://github.com/sj26/rspec_junit_formatter)
    - CircleCIでテストを実行する時に必要なため（[CircleCIのドキュメント](https://circleci.com/docs/2.0/language-ruby/#config-walkthrough)にかかれています）
- [RuboCop（静的解析ツール）](https://github.com/rubocop-hq/rubocop)
- [SimpleCov（テストカバレッジ確認ツール）](https://github.com/colszowka/simplecov)
- [YARD（ドキュメント自動生成ツール）](https://github.com/lsegal/yard)

```ruby
  spec.add_development_dependency 'rspec_junit_formatter'
  spec.add_development_dependency 'rubocop', '~> 0.62'
  spec.add_development_dependency 'rubocop-rspec'
  spec.add_development_dependency 'simplecov'
  spec.add_development_dependency 'yard'
```

### .gemspecファイルの修正結果

```ruby
lib = File.expand_path("lib", __dir__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require "dodonki_sample/version"

Gem::Specification.new do |spec|
  spec.name          = "dodonki_sample"
  spec.version       = DodonkiSample::VERSION
  spec.authors       = ["dodonki1223"]
  spec.email         = ["自分のemailが設定されています"]

  spec.summary       = 'dodonki sample gem file'
  spec.description   = 'dodonki sample gem file'
  spec.homepage      = 'https://github.com/dodonki1223/dodonki_sample'
  spec.license       = 'MIT'

  # spec.metadata["allowed_push_host"] = "TODO: Set to 'http://mygemserver.com'"

  # spec.metadata["homepage_uri"] = spec.homepage
  # spec.metadata["source_code_uri"] = "TODO: Put your gem's public repo URL here."
  # spec.metadata["changelog_uri"] = "TODO: Put your gem's CHANGELOG.md URL here."

  # Specify which files should be added to the gem when it is released.
  # The `git ls-files -z` loads the files in the RubyGem that have been added into git.
  spec.files         = Dir.chdir(File.expand_path('..', __FILE__)) do
    `git ls-files -z`.split("\x0").reject { |f| f.match(%r{^(test|spec|features)/}) }
  end
  spec.bindir        = "exe"
  spec.executables   = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }
  spec.require_paths = ["lib"]

  spec.add_development_dependency "bundler", "~> 2.0"
  spec.add_development_dependency "rake", "~> 10.0"
  spec.add_development_dependency "rspec", "~> 3.0"
  spec.add_development_dependency 'rspec_junit_formatter'
  spec.add_development_dependency 'rubocop', '~> 0.62'
  spec.add_development_dependency 'rubocop-rspec'
  spec.add_development_dependency 'simplecov'
  spec.add_development_dependency 'yard'
end
```

### .gemspecファイルの設定項目について

[公式のドキュメント](https://guides.rubygems.org/specification-reference/)に詳しく書かれているので参照すること

## gemをインストールする

```ruby
$ bundle install --path vendor/bundle
```

.gitignoreにvendorディレクトリを除外する設定を追加（`/vendor/`の記述を追加）

```
/.bundle/
/.yardoc
/_yardoc/
/coverage/
/doc/
/pkg/
/spec/reports/
/tmp/
/vendor/

# rspec failure tracking
.rspec_status

/cache/
/spec/vcr/
```

## MITのライセンスファイルを追加する

下記コマンドを実行してLICENSE.txtファイルを作成してください

```shell
$ touch LICENSE.txt
```

[MIT License](https://opensource.org/licenses/mit-license.php)の原文をコピーし作成したLICENSE.txtに貼り付けて`<YEAR>`、`<COPYRIGHT HOLDER>`をいい感じに変更してください

## 「Hello World」を出力するプログラムを作成する

### lib/dodonki_sample.rbの修正

`lib/dodonki_sample.rb`のファイルを編集します

修正前ソース

```ruby
require "dodonki_sample/version"

module DodonkiSample
  class Error < StandardError; end
  # Your code goes here...
end
```

修正後ソース

```ruby
require "dodonki_sample/version"

module DodonkiSample
  def self.test 
    'Hello World'
  end
end
```

### 動作確認

`bin/console`コマンドを実行するとirbが立ち上がるので`DodonkiSample.test`を実行し`Hello World`が表示されればOKです

```shell
$ bin/console
irb(main):001:0> DodonkiSample.test
=> "Hello World"
```

## Githubにpushする

```shell
$ git add .

$ git commit -m 'first commit'

# リモートリポジトリの設定を追加する
# 「https://github.com/dodonki1223/dodonki_sample.git」ここは適宜変更してください
$ git remote add origin https://github.com/dodonki1223/dodonki_sample.git

$ git push -u origin master
```

## RubyGemsに登録しデプロイする

### RubyGemsにアカウントを作成する

- [新規登録ページ](https://rubygems.org/sign_up)にアクセスしアカウントを作成してください

### Gemコマンドを使うための設定

[プロフィール編集画面](https://rubygems.org/profile/edit)にアクセスし下記コマンドをローカルで実行してください

![](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/672423a9-5685-c507-8afc-f06ff05ed4ff.png)

```shell
$ curl -u user_name https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
```

- [Gemコマンドのリファレンス](https://guides.rubygems.org/command-reference/)

**この作業はなくても良いかも知れません……**

### RubyGemsにデプロイする

Bundlerを使ってビルドとデプロイを行います

#### gemをビルド

フォルダ直下のpkgフォルダの中に作成されます

```shell
$ bundle exec rake build
dodonki_sample 0.1.1 built to pkg/dodonki_sample-0.1.1.gem.
```

#### gemのデプロイ

```shell
$ bundle exec rake release
dodonki_sample 0.1.1 built to pkg/dodonki_sample-0.1.1.gem.
Tagged v0.1.1.
Pushed git commits and tags.
Pushing gem to https://rubygems.org...
Successfully registered gem: dodonki_sample (0.1.1)
Pushed dodonki_sample 0.1.1 to rubygems.org
```

#### エラーがでる場合

変更ファイルがある場合に下記のようなエラーが出ます

```
$ bundle exec rake release
dodonki_sample 0.1.1 built to pkg/dodonki_sample-0.1.1.gem.
rake aborted!
There are files that need to be committed first.
/Users/dodonki/.anyenv/envs/rbenv/versions/2.6.2/bin/bundle:23:in `load'
/Users/dodonki/.anyenv/envs/rbenv/versions/2.6.2/bin/bundle:23:in `<main>'
Tasks: TOP => release => release:guard_clean
(See full trace by running task with --trace)
```

`git status`で確認できる変更ファイルがあるのでcommitしてpushしてからもう一度デプロイ作業をしましょう
`version.rb`ファイルのバージョンを上げた時`bundle install`し忘れてよくこのエラーが出ました……


#### その他のビルド方法

Gemコマンドを使用してビルドする方法もあります

**gemのビルド**

`bundle exec rake build`との違いはフォルダ直下に出来上がることです

```shell
$ gem build gem_name.gemspec
```

#### ビルドについて

[gemコマンドのbuildにかかれている説明](https://guides.rubygems.org/command-reference/#description)では

> The best way to build a gem is to use a Rakefile and the Gem::PackageTask which ships with RubyGems.

と書かれています。
`bundle exec rake build`はRakefileで`Gem::PackageTask`を使用しているので`bundle exec rake build`を使うのが良さそうです

#### デプロイ完了

こんな感じで表示されていればgemの公開完了です！！

![sample_deploy](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/0fcdea33-9c5e-5b60-52e8-d9cac01b82a8.png)

# CircleCIを導入しテストを自動化(CI/継続的インティグレーション)する

基本的にはCircleCIのドキュメント通りに導入していきます

- [Getting Started Introduction - CircleCI](https://circleci.com/docs/2.0/getting-started/)
- [Language Guide: Ruby - CircleCI](https://circleci.com/docs/2.0/language-ruby/)

## CircleCIのconfig.ymlファイルを作成する

### ディレクトリ直下に`.circleci/config.yml`を作成します

```shell
$ mkdir .circleci
$ touch .circleci/config.yml
```

### Rubyプロジェクト用のCircleCIを設定する

公式ドキュメントの[Rubyプロジェクトのサンプル設定](https://circleci.com/docs/2.0/language-ruby/#sample-configuration)を作成した`config.yml`に貼り付けます

```yml
version: 2 # use CircleCI 2.0
jobs: # a collection of steps
  build: # runs not using Workflows must have a `build` job as entry point
    parallelism: 3 # run three instances of this job in parallel
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
        environment: # environment variables for primary container
          BUNDLE_JOBS: 3
          BUNDLE_RETRY: 3
          BUNDLE_PATH: vendor/bundle
          PGHOST: 127.0.0.1
          PGUSER: circleci-demo-ruby
          RAILS_ENV: test
      - image: circleci/postgres:9.5-alpine # database image
        environment: # environment variables for database
          POSTGRES_USER: circleci-demo-ruby
          POSTGRES_DB: rails_blog
          POSTGRES_PASSWORD: ""
    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - rails-demo-bundle-v2-{{ checksum "Gemfile.lock" }}
            - rails-demo-bundle-v2-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install --deployment

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: rails-demo-bundle-v2-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      # Only necessary if app uses webpacker or yarn in some other way
      - restore_cache:
          keys:
            - rails-demo-yarn-{{ checksum "yarn.lock" }}
            - rails-demo-yarn-

      - run:
          name: Yarn Install
          command: yarn install --cache-folder ~/.cache/yarn

      # Store yarn / webpacker cache
      - save_cache:
          key: rails-demo-yarn-{{ checksum "yarn.lock" }}
          paths:
            - ~/.cache/yarn

      - run:
          name: Wait for DB
          command: dockerize -wait tcp://localhost:5432 -timeout 1m

      - run:
          name: Database setup
          command: bin/rails db:schema:load --trace

      - run:
          name: Run rspec in parallel
          command: |
            bundle exec rspec --profile 10 \
                              --format RspecJunitFormatter \
                              --out test_results/rspec.xml \
                              --format progress \
                              $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)

      # Save test results for timing analysis
      - store_test_results: # Upload test results for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: test_results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for example deploy configs
```

### ローカル環境でCircleCIを実行する

CircleCIはローカルでも実行することができます。いくつかのインストール方法がありますが今回は簡単にインストールできるHomebrewを使ってインストールします

他のインストール方法は[公式ドキュメント](https://circleci.com/docs/ja/2.0/local-cli/)に書かれているので確認してください。下記、記事でも別のインストール方法を行って実施しています

- [CircleCI 2.0はローカル環境で実行できるよ - Qiita](https://qiita.com/ieee0824/items/28c7fa9e69cfc6ee7075)
- [CircleCI 2.0 をlocalで動かす - Qiita](https://qiita.com/selmertsx/items/45bd672c2c8ddab1981b)

```shell
$ brew install circleci
```

インストールが終わったら早速、実行してみましょう

```
$ circleci build
Docker image digest: sha256:b26ca9419c2baff5af2c92ab9dc93d91c8d7343ae2a32375d9d6c2f105c92e5a
====>> Spin up Environment
Build-agent version 1.0.13832-2ab6edfd (2019-08-16T10:37:19+0000)
Docker Engine Version: 19.03.1
Kernel Version: Linux c52c55bd6b64 4.9.184-linuxkit #1 SMP Tue Jul 2 22:58:16 UTC 2019 x86_64 Linux
Starting container circleci/ruby:2.4.2-jessie-node
  using image circleci/ruby@sha256:61e67a20f4411e80456599db160a0df736f70d539e372d06f2e24ccc90366703
Starting container circleci/postgres:9.5-alpine
  using image circleci/postgres@sha256:762188fcc7d0e00179c18479db6b604c9939c81082d7d057dfaf37d18ee1908e
====>> Container circleci/postgres:9.5-alpine
Service containers logs streaming is disabled for local builds
You can manually monitor container 956455e71253772001459050ad93da34d190b76dfbcc620cc306062a7bbb6bbb
====>> Checkout code
  #!/bin/bash -eo pipefail
mkdir -p /home/circleci/project && cd /tmp/_circleci_local_build_repo && git ls-files | tar -T - -c | tar -x -C /home/circleci/project && cp -a /tmp/_circleci_local_build_repo/.git /home/circleci/project
====>> Which bundler?
  #!/bin/bash -eo pipefail
bundle -v
Bundler version 1.16.0
====>> Restoring Cache
Error:
Skipping cache - error checking storage: not supported

Step failed
====>> Bundle Install
  #!/bin/bash -eo pipefail
bundle check --path vendor/bundle || bundle install --deployment
You must use Bundler 2 or greater with this lockfile.
You must use Bundler 2 or greater with this lockfile.
Error: Exited with code 20
Step failed
====>> Uploading test results
Error: Unable to save test results from /home/circleci/project/test_results
Error stat /home/circleci/project/test_results: no such file or directory

Error: Found no path with test results, skipping
Error: runner failed (exited with 101)
Task failed
Error: task failed
```

おそらくエラーになったでしょう。
このサンプルが`Railsプロジェクト`のものなので今回のgemで実行した場合はエラーになってしまいます。それでは修正していきましょう

### Railsプロジェクトで使用しているものを削除する

#### 必要のない環境変数の設定を削除する

```yml
          PGHOST: 127.0.0.1
          PGUSER: circleci-demo-ruby
          RAILS_ENV: test
```

#### DBの設定を削除する

```yml
      - image: circleci/postgres:9.5-alpine # database image
        environment: # environment variables for database
          POSTGRES_USER: circleci-demo-ruby
          POSTGRES_DB: rails_blog
          POSTGRES_PASSWORD: ""
```

#### webpackerの設定を削除する

```yml
      # Only necessary if app uses webpacker or yarn in some other way
      - restore_cache:
          keys:
            - rails-demo-yarn-{{ checksum "yarn.lock" }}
            - rails-demo-yarn-

      - run:
          name: Yarn Install
          command: yarn install --cache-folder ~/.cache/yarn

      # Store yarn / webpacker cache
      - save_cache:
          key: rails-demo-yarn-{{ checksum "yarn.lock" }}
          paths:
            - ~/.cache/yarn
```

#### DBのセットアップ系の設定を削除する

```yml
      - run:
          name: Wait for DB
          command: dockerize -wait tcp://localhost:5432 -timeout 1m

      - run:
          name: Database setup
          command: bin/rails db:schema:load --trace
```

この状態で実行してもBundlerでエラーが出るため今度はBundlerのエラーを修正していきます

```shell
bundle check --path vendor/bundle || bundle install --deployment
You must use Bundler 2 or greater with this lockfile.
You must use Bundler 2 or greater with this lockfile.
Error: Exited with code 20
Step failed
```

### Bundlerのエラーを解消する

こちらの記事を参考にBundlerのインストール処理を追加します
- [Using Bundler 2.0 during CI fails - Bug Reports - CircleCI Discuss](https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411/2)

`- checkout`の後に`Bundler`のインストール処理を追加します
`Gemfile.lock`から使用している`Bundler`のバージョンからインストールするようにする

```yml
      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler
```

### RSpecで出ているエラーを解消する

デフォルトで追加されているRSpecのテストで落ちている部分をコメントアウトします

```shell
====>> Run rspec in parallel
  #!/bin/bash -eo pipefail
bundle exec rspec --profile 10 \
                  --format RspecJunitFormatter \
                  --out test_results/rspec.xml \
                  --format progress \
                  $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)

Requested historical based timing, but they are not present.  Falling back to name based sorting
.F

Failures:

  1) DodonkiSample does something useful
     Failure/Error: expect(false).to eq(true)

       expected: true
            got: false

       (compared using ==)

       Diff:
       @@ -1,2 +1,2 @@
       -true
       +false

     # ./spec/dodonki_sample_spec.rb:7:in `block (2 levels) in <top (required)>'

Top 2 slowest examples (0.00955 seconds, 84.0% of total time):
  DodonkiSample does something useful
    0.00907 seconds ./spec/dodonki_sample_spec.rb:6
  DodonkiSample has a version number
    0.00047 seconds ./spec/dodonki_sample_spec.rb:2

Finished in 0.01137 seconds (files took 0.08332 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/dodonki_sample_spec.rb:6 # DodonkiSample does something useful

Error: Exited with code 1
```

`spec/gem名_spec.rb`のファイル内の下記部分をコメントアウトしてください

```ruby
  it "does something useful" do
    expect(false).to eq(true)
  end
```

### その他のエラーについて

ローカル実行の時は使用できない機能なのでこのエラーは無視します

```shell
====>> Restoring Cache
Error:
Skipping cache - error checking storage: not supported

Step failed

...

====>> Saving Cache
Error:
Skipping cache - error checking storage: not supported

Step failed

...

====>> Uploading test results
Archiving the following test results
  * /home/circleci/project/test_results

Error: Failed uploading test results directory
Error &errors.errorString{s:"not supported"}
```

### 気持ち悪い部分を修正する

- キャッシュファイルのキー名(`rails-demo-bundle-v2-`)をいい感じに変更する

### 修正後のconfig.yml

```yml
version: 2 # use CircleCI 2.0
jobs: # a collection of steps
  build: # runs not using Workflows must have a `build` job as entry point
    parallelism: 3 # run three instances of this job in parallel
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
        environment: # environment variables for primary container
          BUNDLE_JOBS: 3
          BUNDLE_RETRY: 3
          BUNDLE_PATH: vendor/bundle
    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-sample-{{ checksum "Gemfile.lock" }}
            - gem-sample-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install --deployment

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-sample-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      - run:
          name: Run rspec in parallel
          command: |
            bundle exec rspec --profile 10 \
                              --format RspecJunitFormatter \
                              --out test_results/rspec.xml \
                              --format progress \
                              $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)

      # Save test results for timing analysis
      - store_test_results: # Upload test results for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: test_results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for example deploy configs
```

## CircleCIでプロジェクトをセットアップする

CircleCIのアカウントが必要なので作成していない人は[Signup](https://circleci.com/signup/)ページにアクセスしアカウントを作成してください

### プロジェクトの追加を行う

[Add Projects](https://circleci.com/add-projects)ページにアクセスし`Set Up Project`をクリックします

![sample_add_project](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/db189a6e-ee5a-b2dd-a641-683a1ab95e07.png)

`Start building`をクリックします

![sample_start_building](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/ab082491-3900-cd0e-4073-c9d0d4ad4f09.png)

### 結果を確認する

`Start building`をクリックすることで初めてCircleCIが実行されます

`master / workflow`をクリックします

![first_build](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/25534909-545c-e81d-a875-6abd6e366fab.png)

`build`をクリックすることでCircleCIの実行結果を確認することができます

![first_build_buildbutton](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/343b6836-1d11-f51f-4c56-d98671948ef0.png)

テストが実行されていることを確認できます

![test_result_1](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/d98d2ca6-9ba6-90ba-635e-06a02a9f1632.png)
![test_result_2](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/b0435247-6b1f-318e-653a-790536ef4e9f.png)

これでGithubにpush、mergeするたびにCircleCIが実行され自動テストされるようになりました

### 実際に試してみる

何でもいいのでファイルを修正してGithubにpushしてみましょう。push後、commit履歴から実行されたCircleCIを確認できます

![確認方法](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/67948e10-c938-9b7d-3322-fb78b3070b2c.png)

# CircleCIのテストを充実させテスト結果を確認しやすくしよう

## RuboCopの導入

CircleCIに[RuboCop(静的解析ツール)](https://github.com/rubocop-hq/rubocop)を追加し一定のコード品質を保てるようにします

### CircleCIの実行時にRuboCopが実行されるようにする

下記の設定をconfig.ymlにRSpecの前に追加してください

```yml
      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
```

追加後の`config.yml`

```yml
version: 2 # use CircleCI 2.0
jobs: # a collection of steps
  build: # runs not using Workflows must have a `build` job as entry point
    parallelism: 3 # run three instances of this job in parallel
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
        environment: # environment variables for primary container
          BUNDLE_JOBS: 3
          BUNDLE_RETRY: 3
          BUNDLE_PATH: vendor/bundle

    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-sample-{{ checksum "Gemfile.lock" }}
            - gem-sample-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install --deployment

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-sample-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
      - run:
          name: Run rspec in parallel
          command: |
            bundle exec rspec --profile 10 \
                              --format RspecJunitFormatter \
                              --out test_results/rspec.xml \
                              --format progress \
                              $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)

      # Save test results for timing analysis
      - store_test_results: # Upload test results for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: test_results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for example deploy configs
```

### RuboCopが成功するようにする

`circleci build`を実行すると大量にエラーが出ていると思います
今回の記事ではRuboCopについては詳しくは説明しないので私の[RuboCop導入時のCommit履歴](https://github.com/dodonki1223/dodonki_sample/commit/c02b57d14362af1c60c10010a97873b17b23b9c6)と同じように修正すればRuboCopでエラーがでなくなります

### RuboCopの動作確認

`circleci build`を実行し下記のような表示があれば導入OKです

```shell
$ circleci build

......

====>> run rubocop
  #!/bin/bash -eo pipefail
bundle exec rubocop

Inspecting 8 files
........
```

修正した内容をpushしてCircleCIの動作を確認します

![rubocop_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a2797b9f-dc4d-f719-0c35-63b1139eff14.png)

RuboCopの導入完了です

## SimpleCovの導入

CircleCIに[SimpleCov（テストカバレッジ確認ツール）](https://github.com/colszowka/simplecov)を追加しテストカバレッジを確認できるようにします

### CircleCIの実行時にSimpleCovが実行されるようにする

`spec/spec_helper.rb`に下記コードを追加します

```ruby
require 'simplecov'

# SimpleCovのロード処理（RSpecのファイルは除外する）
SimpleCov.start do
  add_filter '/spec/'
end
```

### CircleCI上でカバレッジを確認できるようにする

CircleCIの`Artifacts`にSimpleCovのファイルが出力されるようにします
ついでにRSpecのテスト結果も出力されるようにします

CircleCIの[公式のドキュメント](https://circleci.com/docs/2.0/code-coverage/#ruby)にも導入方法が記載されています

下記の設定を`store_test_results`の後に記述します

```yml
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results
```

追加後の`config.yml`

```yml
version: 2 # use CircleCI 2.0
jobs: # a collection of steps
  build: # runs not using Workflows must have a `build` job as entry point
    parallelism: 3 # run three instances of this job in parallel
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
        environment: # environment variables for primary container
          BUNDLE_JOBS: 3
          BUNDLE_RETRY: 3
          BUNDLE_PATH: vendor/bundle

    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-sample-{{ checksum "Gemfile.lock" }}
            - gem-sample-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install --deployment

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-sample-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
      - run:
          name: Run rspec in parallel
          command: |
            bundle exec rspec --profile 10 \
                              --format RspecJunitFormatter \
                              --out test_results/rspec.xml \
                              --format progress \
                              $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)

      # Save test results for timing analysis
      - store_test_results: # Upload test results for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for example deploy configs
```

修正した内容はこちらの[コミット履歴](https://github.com/dodonki1223/dodonki_sample/commit/42b6fee079b11ab50a2ec5a4d5c695003e2a4350)を参照してください

### SimpleCov・RSpecのテスト結果が出力されているかの動作確認

`circleci build`を実行し下記のような表示があれば導入OKです

```shell
$ circleci build

......

====>> Uploading artifacts
Uploading /home/circleci/project/test_results to test-results
Uploading /home/circleci/project/test_results/rspec.xml (420 B): Error: FAILED with error not supported

====>> Uploading artifacts
Uploading /home/circleci/project/coverage to coverage-results
Uploading /home/circleci/project/coverage/.last_run.json (50 B): Error: FAILED with error not supported

......

```

修正した内容をpushしCircleCIの動作を確認します

![simplecov_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/ca9a6697-cba9-508d-92c2-626d9a94ef6c.png)

`Artifacts`の画面で画像のように出力されていれば導入完了です

`index.html`をクリックすることでテストのカバレッジを確認できるようになります

![test_coverrage_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/9212bb92-075b-83f9-7dd8-42463c7186d9.png)

# CircleCIでドキュメントが自動生成されるようにする

## YARDの導入

[YARD（ドキュメント自動生成ツール）](https://github.com/lsegal/yard)を追加し自動でドキュメントが作成されるようにします
必要のない人はYARDの導入は飛ばしてもらって構いません

### CircleCIの実行時にドキュメントが自動生成されるようにする

下記の設定をRSpecの処理の後に追加してください

```yml
      # create document
      - run:
          name: create document
          command: |
            bundle exec yard
```

下記の設定をカバレッジの結果の吐き出す処理の後に追加してください

```yml
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results
```

追加後の`config.yml`

```yml
version: 2 # use CircleCI 2.0
jobs: # a collection of steps
  build: # runs not using Workflows must have a `build` job as entry point
    parallelism: 3 # run three instances of this job in parallel
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
        environment: # environment variables for primary container
          BUNDLE_JOBS: 3
          BUNDLE_RETRY: 3
          BUNDLE_PATH: vendor/bundle

    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-sample-{{ checksum "Gemfile.lock" }}
            - gem-sample-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install --deployment

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-sample-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
      - run:
          name: Run rspec in parallel
          command: |
            bundle exec rspec --profile 10 \
                              --format RspecJunitFormatter \
                              --out test_results/rspec.xml \
                              --format progress \
                              $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)
      # create document
      - run:
          name: create document
          command: |
            bundle exec yard

      # Save test results for timing analysis
      - store_test_results: # Upload test results for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for example deploy configs
```

修正した内容はこちらの[コミット履歴](https://github.com/dodonki1223/dodonki_sample/commit/0df80c2e6aa646a47fce3a2e48c8e03d72ed25ba)を参照してください

### YARDの動作確認

`circleci build`を実行し下記のような表示があれば導入OKです

```shell

......

====>> create document
  #!/bin/bash -eo pipefail
bundle exec yard

Files:           2
Modules:         1 (    1 undocumented)
Classes:         0 (    0 undocumented)
Constants:       1 (    1 undocumented)
Attributes:      0 (    0 undocumented)
Methods:         1 (    1 undocumented)
 0.00% documented

 ......

 ====>> Uploading artifacts
Uploading /home/circleci/project/doc to yard-results
Uploading /home/circleci/project/doc/DodonkiSample.html (3.7 kB): Error: FAILED with error not supported

......
```

修正した内容をpushしCircleCIの動作を確認します

![yard_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/b6517a28-322f-4bb6-919c-54c2c93b96c4.png)

`yard-results`ディレクトリができていれば導入完了です

`yard-results/index.html`をクリックすることでドキュメントを見ることができます

![yard_document_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/4601e9d0-e6af-03de-f8dd-f22265eda1e1.png)

# RubyGemsへ自動デプロイ機能(CD/継続的デリバリー)を追加する

## RubyGemsのデプロイで使用する環境変数をCircleCIに登録する

プロジェクトページの`Environment Variables`をクリックします

![circleci_environment_setting](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/fffad959-dd1c-4259-8a0e-4e35e00ee8e6.png)

`Add Variable`をクリックし`Name`と`Value`に値をセットし`Add Variable`をクリックします

![circleci_environment_add_variable](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/4fa06ce8-288a-0937-d38e-d9633f29a76f.png)

`RUBYGEMS_PASSWORD`、`RUBYGEMS_EMAIL`という環境変数を追加してください

|  Name              | Value                               |
|:-------------------|:------------------------------------|
|  RUBYGEMS_PASSWORD |  RubyGemsにログインするパスワード   |
|  RUBYGEMS_EMAIL    |  GitHubに登録しているメールアドレス |


## デプロイ機能を追加する

様々な環境へのデプロイ方法が公式のドキュメントに書かれているので参考にしましょう

[Configuring Deploys - CircleCI](https://circleci.com/docs/2.0/deployment-integrations/)

### デプロイ用のjobを追加する

`config.yml`の一番下に下記コードを追加してください

```yml
  deploy:
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run

    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-deploy-{{ checksum "Gemfile.lock" }}
            - gem-deploy-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-deploy-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      - run:
          name: deploy
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release
```

nameが`deploy`のところが実際のデプロイ処理になります
それより前がデプロイコマンドを実行するための準備です

gemコマンドが使えるようにするため、下記コマンドを実行します(ここで`RUBYGEMS_PASSWORD`の環境変数を使用しています)

```shell
curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
```

連携先のGitの情報をセットします(ここでRUBYGEMS_EMAILを使用しています)

```shell
git config user.name dodonki1223
git config user.email $RUBYGEMS_EMAIL
```

RubyGemsのデプロイコマンドを実行します
`git tag`の情報がpushされるためデプロイコマンドの前でGit情報をセットしています

```shell
bundle exec rake build
bundle exec rake release
```

### デプロイのjobがmasterブランチでのみ実行されるようWorkflowで制御する

Workflowについては下記の記事を参考にしてください
- [ジョブの実行を Workflow で制御する - CircleCI](https://circleci.com/docs/ja/2.0/workflows/) 

`config.yml`の一番下に下記コードを追加してください

```yml
workflows:
  version: 2
  build-and-deploy:
    jobs:
      - build
      - deploy:
          requires:
            - build
          filters:
            branches:
              only: master
```

### 最終的なconfig.yml

```yml
version: 2 # use CircleCI 2.0
jobs: # a collection of steps
  build: # runs not using Workflows must have a `build` job as entry point
    parallelism: 3 # run three instances of this job in parallel
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
        environment: # environment variables for primary container
          BUNDLE_JOBS: 3
          BUNDLE_RETRY: 3
          BUNDLE_PATH: vendor/bundle

    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-sample-{{ checksum "Gemfile.lock" }}
            - gem-sample-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install --deployment

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-sample-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
      - run:
          name: Run rspec in parallel
          command: |
            bundle exec rspec --profile 10 \
                              --format RspecJunitFormatter \
                              --out test_results/rspec.xml \
                              --format progress \
                              $(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)
      # create document
      - run:
          name: create document
          command: |
            bundle exec yard

      # Save test results for timing analysis
      - store_test_results: # Upload test results for display in Test Summary: https://circleci.com/docs/2.0/collect-test-data/
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results
      # See https://circleci.com/docs/2.0/deployment-integrations/ for example deploy configs

  deploy:
    docker: # run the steps with Docker
      - image: circleci/ruby:2.4.2-jessie-node # ...with this image as the primary container; this is where all `steps` will run
      # - image: circleci/ruby:2.6.0-node-browsers

    steps: # a collection of executable commands
      - checkout # special step to check out source code to working directory

      - run:
          name: install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler

      # Which version of bundler?
      - run:
          name: Which bundler?
          command: bundle -v

      # Restore bundle cache
      # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
      - restore_cache:
          keys:
            - gem-deploy-{{ checksum "Gemfile.lock" }}
            - gem-deploy-

      - run: # Install Ruby dependencies
          name: Bundle Install
          command: bundle check --path vendor/bundle || bundle install

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: gem-deploy-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      - run:
          name: deploy
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

workflows:
  version: 2
  build-and-deploy:
    jobs:
      - build
      - deploy:
          requires:
            - build
          filters:
            branches:
              only: master
```

**`circleci build`コマンドがWorkflowに対応していないので今回はローカルでの実行はしません**

## CircleCIからGitHubに連携できるようにする

RubyGemsへのデプロイコマンドで`git tag`の情報がpushされるためCircleCIからGitHubに連携できるようにする必要があります

設定していないと下記のようなエラーが出てデプロイができません

![circleci_deploy_error](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/c164998f-9b62-b764-2e88-56ae086584fc.png)

```
dodonki_sample 0.1.5 built to pkg/dodonki_sample-0.1.5.gem.
dodonki_sample 0.1.5 built to pkg/dodonki_sample-0.1.5.gem.
Tagged v0.1.5.
Untagging v0.1.5 due to error.
rake aborted!
Couldn't git push. `git push ' failed with the following output:

warning: push.default is unset; its implicit value has changed in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the traditional behavior, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

When push.default is set to 'matching', git will push local branches
to the remote branches that already exist with the same name.

Since Git 2.0, Git defaults to the more conservative 'simple'
behavior, which only pushes the current branch to the corresponding
remote branch that 'git pull' uses to update the current branch.

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

ERROR: The key you are authenticating with has been marked as read only.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

`SETTINGS`の画面から`Projects`をクリックします

![circleci_setting](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/1dd18f85-67cf-edbd-df15-47962c36b5a8.png)

対象のプロジェクトの設定ボタンをクリックします

![circleci_setting_target](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/42a34fd1-3054-7342-2bf1-f69018404b75.png)

`Checkout SSH keys`の画面から`Authorize With GitHub`をクリックします

![circleci_setting_authorize1](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/c799ed50-7ec8-bb88-5643-2443827df97c.png)

`Create and add ユーザー名 user_key`をクリックします

![circleci_setting_authorize2](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/a9e8094c-110d-4876-d449-4c7608169800.png)

下記のようにKeyが追加されていれば準備OKです

![circleci_setting_authorize3](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/78fcfad3-e180-2487-6983-5f8abe9891b3.png)

## 自動デプロイ動作確認

### バージョンをアップする

`lib/dodonki_sample/version.rb`のファイルを修正します

バージョンを`0.1.0`から`0.1.1`に上げます

```ruby
  VERSION = '0.1.1'
```

### bundle installする

バージョンを上げることにより`bundle install`するとGemfile.loackに変更がかかります

### 自動デプロイの動作確認

masterブランチで今までの変更をcommitしpushしてみましょう

GitHubのCommit履歴を確認するとCircleCIのjobが２つ表示されていることが確認できます

![github_sample_deploy](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/aa4c2e63-2f4f-3ef5-c898-505f8a61df7d.png)

CircleCI上でログを確認しましょう

![circleci_sample_deploy](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/f3b3b85d-aaca-8295-c691-747630e6c218.png)

無事、デプロイされたことを確認できました

### master以外のブランチの時デプロイされないことの動作確認

別のブランチを作成しpushしてみましょう
GitHubのCommit履歴を確認するとなぜかjobが２つ表示されています。謎です……

![github_not_master_deploy_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/7e3b10d6-9d8f-4ef9-e57b-3c07a978eb00.png)

CircleCIのjob一覧を確認すると`build`のjobのみ実行されていて`deploy`のjobは実行されていないことが確認できます

![circleci_not_master_deploy_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/75df3950-7792-245b-391a-7bb4dc1e7bef.png)

# CircleCIでなんだかエラーになるぞエラーを特定しよう

CircleCIでエラーになった時はsshでCircleCIのコンテナに接続し直接コマンドを叩いて確認した方が効率が良いです
いちいちpushして確認するのはアホらしいので……

## CircleCIのコンテナに接続するための準備

こちらの記事を参考にSSHキーをGitHubアカウントに追加しておいてください

- [Adding a new SSH key to your GitHub account - GitHub Help](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account)
- [GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~ - Qiita](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)

## CircleCIのコンテナに接続する

[SSH を使用したデバッグ - CircleCI](https://circleci.com/docs/ja/2.0/ssh-access-jobs/)に詳しく書かれています

基本的に失敗したjobの画面の右上の`Return job with SSh`をクリックするだけです
一番下にsshで接続するためのコマンドが出てくるのでこれを実行すればログインできます

```shell
$ ssh -p 64535 18.208.137.163
```

![failure_job_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/3b4e9091-8604-ef8d-ca04-193fb8d21dfe.png)

私の場合は秘密鍵ファイル名を変更しているので上記コマンドではコンテナに入ることができません
秘密鍵のファイルを直接指定して入るようにしています

```shell
$ ssh -i ~/.ssh/github_rsa -p 64535 18.208.137.163
```

# SlackとCircleCI、GitHubを連携させ開発しやすくしよう

CircleCIを導入したことによりCircleCIのページやGitHubのページを頻繁に見に行く必要が出てきました……なんかいろいろとめんどくさいですよね
私はGitHubへのcommit履歴やCircleCIの結果をSlackに通知するように設定しています
テストが成功したかどうかはSlackに通知されるのでいちいちCircleCIにアクセスしたりしないで済む様になりました

## GitHubとSlackを連携させる

![github-action](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/b8986468-2a0a-5e3d-fd65-0e1f540b8ebb.png)

slackに`github-action`のチャンネルを作り、そこに履歴が残るように設定しています
Slackの公式ページに[連携方法](https://get.slack.help/hc/ja/articles/232289568-GitHub-と-Slack-を連携させる)が書かれているので参考にすると良いでしょう

## CirleCIとSlackを連携させる

![circle-ci](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/f42cfc32-441e-f8cf-0520-d9d2c0553319.png)

slackに`circle-ci`のチャンネルを作りそこにCirlceCIの実行結果が通知されるようにしています
下記の記事にわかりやすく書かれているので参考にすると良いでしょう

- [CircleCIの結果をSlackに通知する - Qiita](https://qiita.com/su-kun1899/items/640f6fa8b48749396c16)

# RubyDoc.infoに作成したgemのリファレンスを公開する

今回のタイトルとはあまり関係ないですがgemのリファレンスを簡単に公開できるので紹介します

## RubyDoc.infoに公開

[RubyDoc.info](https://rubydoc.info/)にアクセスし`Add Project`ボタンをクリックしgemのリポジトリを指定して`Go`のボタンをクリックするだけで簡単に公開できます

![rubydocinfo_sample](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/9069ffaa-2d8f-e569-1a73-56a1cc815c46.png)

公開すると下記の画像のようになります

![qiita_trend_document](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/299904/31036a04-9584-0843-4fe4-97b8c233e3bd.png)

公開している[ドキュメント](https://rubydoc.info/github/dodonki1223/qiita_trend/master)はこちら

# 最後に

自動テスト、自動デプロイ最高ですね！！

今回の作成したプログラムは[dodonki1223/dodonki_sample: RubyのgemをCirlcleCiでデプロイするサンプル用プログラム](https://github.com/dodonki1223/dodonki_sample)こちらで公開しています

AWSの[CodeDploy](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/welcome.html)、[CodeBuild](https://docs.aws.amazon.com/ja_jp/codebuild/latest/userguide/welcome.html)、[CodePipeline](https://docs.aws.amazon.com/ja_jp/codepipeline/latest/userguide/welcome.html)やGitHubの[GitHub Actions](https://github.com/features/actions)などCircleCIの代替もあるので今後はどうなっていくのでしょうか……

