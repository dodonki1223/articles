# 自作ライブラリの運用で代わり果ててしまった CircleCI の設定ファイル

[QiitaTrend](https://github.com/dodonki1223/qiita_trend) という Ruby の gem を開発していく中で導入した CircleCI の設定がどういう理由でどう変わっていったのか当時の状況を思い出しつつまとめてみたいと思います

![qiita_trend](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_trend/01_qiita_trend.gif)

[QiitaTrend](https://github.com/dodonki1223/qiita_trend) は Qiitaのトレンド情報（Daily、Weekly、Monthly）を10秒で取得することができる gem になります。

# config.yml の大まかな年表

CircleCI の config.yml を今まででたくさん更新してきました。
運用してきた config.yml は厳密にはもっと修正が細かいですが大まかに分けると以下のような修正履歴になります。

| 年月     | 変更内容                                      |
|:--------:|:----------------------------------------------|
| 2019/08  | CircleCIの導入                                |
| 2019/08  | YARD でドキュメントが自動生成されるように変更 |
| 2019/08  | ワークフローを導入                            |
| 2019/12  | CircleCI2.1へバージョンアップ                 |
| 2020/02  | workspace 機能の導入                          |
| 2020/04  | slack 通知・デプロイ承認機能追加              |
| 2020/08  | 毎日テストを実行させるワークフローを追加      |
| 2020/10  | CircleCI のイメージを次世代のものに変更       |
| 2020/11  | Ruby の orb の導入                            |


# 2019年8月：初じめてのCircleCI

自分の勉強のために Ruby の gem を作成しようというところから始まります。  

**テストとデプロイを自動化したい** という思いから業務でも使用していた **CircleCI を導入すること** にしました。
そもそもこの時点では **CI/CDの構築を一回もしたことがなく** ほぼ知識がない状態からの導入です。

## 導入方法

そもそも知識がない状態でどうやって導入したんだっけ？
思い出してみると初導入に関しては **Qiita に記事** として残してあります。

- [Rubyのgemを公開しつつCircleCIでCI/CD(継続的インティグレーション/継続的デリバリー)を体験しよう](https://qiita.com/dodonki1223/items/c94f5b185fd5fa815bb1#circleci%E3%82%92%E5%B0%8E%E5%85%A5%E3%81%97%E3%83%86%E3%82%B9%E3%83%88%E3%82%92%E8%87%AA%E5%8B%95%E5%8C%96ci%E7%B6%99%E7%B6%9A%E7%9A%84%E3%82%A4%E3%83%B3%E3%83%86%E3%82%A3%E3%82%B0%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%99%E3%82%8B)

基本的には **公式のドキュメントを読んでそのままコピペしたものに不必要なものを削除、必要な機能を追加していき作成** しました。詳しくは Qiita の記事を読んでください。

## config.yml

初めて導入した config.yml は以下です。今、見るととても簡素なものですね（笑）
version2.1 ではないし、workflow も組んでいませんね。

![00_add_first_circleci](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/00_add_first_circleci.png)

<details>
<summary>config.yml</summary>
<div>

```yml
# Ruby CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-ruby/ for more details
#
version: 2
jobs:
  build:
    docker:
      # specify the version you desire here
      - image: circleci/ruby:2.6.0-node-browsers
        environment:
          BUNDLER_VERSION: 2.0.1

      # Specify service dependencies here if necessary
      # CircleCI maintains a library of pre-built images
      # documented at https://circleci.com/docs/2.0/circleci-images/
      # - image: circleci/postgres:9.4

    working_directory: ~/repo

    steps:
      - checkout
      # Download and cache dependencies
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-
      # install Bundler!
      # ref:https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411
      - run:
          name: install bundler
          command: |
            sudo gem update --system
            sudo gem uninstall bundler
            sudo rm /usr/local/bin/bundle
            sudo rm /usr/local/bin/bundler
            sudo gem install bundler
      # install gem!
      - run:
          name: install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}
      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
      # run tests!
      - run:
          name: run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES
      # collect reports
      - store_test_results:
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results
      # deploy RubyGems
      - deploy:
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              mkdir ~/.gem
              curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
              git config user.name dodonki1223
              git config user.email $RUBYGEMS_EMAIL
              bundle exec rake build
              bundle exec rake release
            fi
```

</div>
</details>

## 追加した内容

キャッシュ以外の処理について説明します。基本的には CircleCI のドキュメントに載っていたものをそのまま持ってきただけのものが多いです。

### Bundler のインストール

- Bundler のインストールを行っている処理
- こちらは CircleCI のドキュメントに書かれていたものをコピペして実装
- ちなみに Bundler とは gem のバージョンや gem の依存関係を管理してくれる gem のこと

```yml
- run:
    name: install bundler
    command: |
      sudo gem update --system
      sudo gem uninstall bundler
      sudo rm /usr/local/bin/bundle
      sudo rm /usr/local/bin/bundler
      sudo gem install bundler
```

### 依存関係のインストール

- gem で使用する依存関係をインストールする処理
- こちらも CircleCI のドキュメントに書かれていたものをコピペして実装

```yml
- run:
    name: install gem
    command: |
      # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
      bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3
```

### RuboCop の実行

- 静的解析ツールの [RuboCop](https://github.com/rubocop-hq/rubocop) を実行している処理

```yml
- run:
    name: run rubocop
    command: |
      bundle exec rubocop
```

###  RSpec の実行

- RSpec を実行している処理（テストの実行）
- こちらも CircleCI のドキュメントに書かれていたものをコピペして実装

```yml
- run:
    name: run tests
    command: |
      TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
      bundle exec rspec \
        --format progress \
        --format RspecJunitFormatter \
        --out test_results/rspec.xml \
        --format progress \
        $TEST_FILES
```

### 成果物のアップロード

- テスト結果、カバレッジの結果を CircleCI にアップロードして結果を見れるようにしている処理
- こちらも CircleCI のドキュメントを参考に実装したものです

```yml
# collect reports
- store_test_results:
    path: test_results
- store_artifacts:
    # テスト結果をtest-resultsディレクトリに吐き出す
    path: test_results
    destination: test-results
- store_artifacts:
    # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
    path: coverage
    destination: coverage-results
```

### RubyGems へデプロイ

- master ブランチの時に RubyGems へのデプロイをする処理
- [RubygemsへのデプロイをCircleCIで自動化してみた | WEB EGG](https://blog.leko.jp/post/automate-deploy-to-rubygems-with-circleci/) を参考に実装

```yml
- deploy:
    command: |
      if [ "${CIRCLE_BRANCH}" == "master" ]; then
        mkdir ~/.gem
        curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
        git config user.name dodonki1223
        git config user.email $RUBYGEMS_EMAIL
        bundle exec rake build
        bundle exec rake release
      fi
```

## 初めて導入してみてどうだったか？

- CI/CD がよくわからない難しいものだと思っていたが実際に順を追って導入すると **そんなに難しくないこと** がわかった
- CircleCI の **キャッシュについてある程度、理解することができた**

# 2019年8月：ドキュメントの自動作成

**gem のドキュメントを RubyDoc.info に公開するために [YARD](https://github.com/lsegal/yard) を導入しドキュメントを自動生成する処理を追加。** 実際に公開しているドキュメントは[こちら](https://rubydoc.info/github/dodonki1223/qiita_trend)になります。

## config.yml

ドキュメントを自動生成する処理を追加した config.yml です。

![01_add_create_document](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/01_add_create_document.png)

<details>
<summary>config.yml</summary>
<div>

```yml
# Ruby CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-ruby/ for more details
#
version: 2
jobs:
  build:
    docker:
      # specify the version you desire here
      - image: circleci/ruby:2.6.0-node-browsers
        environment:
          BUNDLER_VERSION: 2.0.1

      # Specify service dependencies here if necessary
      # CircleCI maintains a library of pre-built images
      # documented at https://circleci.com/docs/2.0/circleci-images/
      # - image: circleci/postgres:9.4

    working_directory: ~/repo

    steps:
      - checkout

      # Download and cache dependencies
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

      # install Bundler!
      # ref:https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411
      - run:
          name: install bundler
          command: |
            sudo gem update --system
            sudo gem uninstall bundler
            sudo rm /usr/local/bin/bundle
            sudo rm /usr/local/bin/bundler
            sudo gem install bundler
      # install gem!
      - run:
          name: install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}
      # run rubocop!
      - run:
          name: run rubocop
          command: |
            bundle exec rubocop
      # run tests!
      - run:
          name: run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES
      # create document
      - run:
          name: create document
          command: |
            bundle exec yard
      # collect reports
      - store_test_results:
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
      # deploy RubyGems
      - deploy:
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              mkdir ~/.gem
              curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
              git config user.name dodonki1223
              git config user.email $RUBYGEMS_EMAIL
              bundle exec rake build
              bundle exec rake release
            fi
```

</div>
</details>

## 更新内容

YARD の作成処理と作成した YARD のアップロード機能を追加。

### YARD の作成

- YARD のドキュメントを参考に実装

```yml
# create document
- run:
    name: create document
    command: |
      bundle exec yard
```

### 作成した YARD のアップロード

- 以前同じようなことをしているので特に何も考えなくても実装できた

```yml
- store_artifacts:
    # ドキュメントの結果をyard-resultsディレクトリに吐き出す
    path: ./doc
    destination: yard-results
```

# 2019年8月：ワークフローの導入

ワークフローという機能があることに気づき、 **ワークフローの導入を行う**。

## config.yml

ワークフローの設定を追加した config.yml です。

![02_add_workflows](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/02_add_workflows.png)

<details>
<summary>config.yml</summary>
<div>

```yml
# Ruby CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-ruby/ for more details
#
version: 2
jobs:
  build:
    docker:
      - image: circleci/ruby:2.6.0
        environment:
          BUNDLE_PATH: vendor/bundle

    working_directory: ~/repo

    steps:
      - checkout

      # Install Bundler
      # ref:https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411
      - run:
          name: Install Bundler
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
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

      # Install gem
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

      # Store bundle cache for Ruby dependencies
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

      # Run RuboCop
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

      # Run tests
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES

      # Create document
      - run:
          name: Create document
          command: |
            bundle exec yard

      # Collect Reports
      - store_test_results:
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

  deploy:
    docker:
      - image: circleci/ruby:2.6.0

    steps:
      - checkout

      # Install Bundler
      # ref:https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411
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
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            - v1-dependencies-

      # Install gem
      - run:
          name: Install gem
          command: bundle check --path vendor/bundle || bundle install

      # Store bundle cache for Ruby dependencies
      - save_cache:
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}
          paths:
            - vendor/bundle

      # Deploy RubyGems
      - run:
          name: Deploy RubyGems
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
              only: master # masterブランチの時だけDeployJobを実行するようにする
```

</div>
</details>

## 更新内容

build の一部を切り離し deploy という job を作成しそれぞれを workflows で順番を担保させるように改修。

### 追加した workflow 設定

- build → deploy の順番で処理されるように設定
- deploy は master ブランチのみ実行されるように設定。

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
              only: master # masterブランチの時だけDeployJobを実行するようにする
```

# 2019年12月：CircleCI 2.1 の導入

**CircleCI のバージョンを 2.0 から 2.1 に変更し冗長な書きぶりを排除**。
この改修をしてた時は **AWS 認定 ソリューションアーキテクト – アソシエイト** を受けて落ちてやけくそになっていた時でした（笑）
これが完成した時は少しだけいい気分になりました……。CircleCI のおかげです。

**AWS 認定 ソリューションアーキテクト – アソシエイト** は1ヶ月後に無事受かりました。

## config.yml

バージョンを 2.0 から 2.1 に変更した config.yml です。

![03_update_version2_1](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/03_update_version2_1.png)

<details>
<summary>config.yml</summary>
<div>

```yml
version: 2.1
executors:
  base:
    docker:
      - image: circleci/ruby:2.6.0
        environment:
          BUNDLE_PATH: vendor/bundle
    working_directory: ~/repo

commands:
  # ref:https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411
  install-bundler:
    steps:

      - run:
          name: Install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler
            # Which version of bundler?
            bundle -v

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:

      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

  run-rubocop:
    steps:
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

  run-tests:
    steps:
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard

  collect-reports:
    steps:
      # ref:https://circleci.com/docs/ja/2.0/configuration-reference/#store_test_results
      - store_test_results:
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

  # Deploy RubyGems
  deploy-rubygems:

    steps:
      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache

  lint:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - run-rubocop

  test:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - run-tests
      - create-document
      - collect-reports

  deploy:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - deploy-rubygems

workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - test:
          requires:
            - setup
      - deploy:
          requires:
            - lint
            - test
          filters:
            branches:
              only: master # masterブランチの時だけDeployJobを実行するようにする
```

</div>
</details>

## 更新内容

この時の改修が一番変更箇所が多くて中身がガラッと変わった時です。
[executor](https://circleci.com/docs/ja/2.0/configuration-reference/#executors-version-21-%E3%81%8C%E5%BF%85%E9%A0%88) と [command](https://circleci.com/docs/ja/2.0/configuration-reference/#commands-version-21-%E3%81%8C%E5%BF%85%E9%A0%88) 機能を使用することで冗長な部分がなくなりかなりスッキリとした改修になりました。

まだ 2.0 を使っているなら **はやく 2.1 にバージョンアップすることをオススメ** します。

### executors

- executors を使用しそれぞれの job で image を使い回すことができるようになった

```yml
executors:
  base:
    docker:
      - image: circleci/ruby:2.6.0
        environment:
          BUNDLE_PATH: vendor/bundle
    working_directory: ~/repo
```

### commands

- commands に必要な処理をそれぞれ実装し使い回せるようにする

```yml
commands:
  # ref:https://discuss.circleci.com/t/using-bundler-2-0-during-ci-fails/27411
  install-bundler:
    steps:

      - run:
          name: Install Bundler
          command: |
            echo 'export BUNDLER_VERSION=$(cat Gemfile.lock | tail -1 | tr -d " ")' >> $BASH_ENV
            source $BASH_ENV
            gem install bundler
            # Which version of bundler?
            bundle -v

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:

      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

  run-rubocop:
    steps:
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

  run-tests:
    steps:
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard

  collect-reports:
    steps:
      # ref:https://circleci.com/docs/ja/2.0/configuration-reference/#store_test_results
      - store_test_results:
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

  # Deploy RubyGems
  deploy-rubygems:
    steps:
      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release
```

### jobs

- executor と command を使用し jobs をスッキリと記述
- 細かい単位で job を定義する

```yml
jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache

  lint:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - run-rubocop

  test:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - run-tests
      - create-document
      - collect-reports

  deploy:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - deploy-rubygems
```

### workflows

- 細かくした job を反映させる

```yml
workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - test:
          requires:
            - setup
      - deploy:
          requires:
            - lint
            - test
          filters:
            branches:
              only: master # masterブランチの時だけDeployJobを実行するようにする
```

# 2020年2月：workspace 機能の導入

**workspace という job 間で使用できるキャッシュ機能** を知り、実装する

## config.yml

workspace を追加し job 間でキャッシュを使えるようにした config.yml です。

![04_add_workspace](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/04_add_workspace.png)

<details>
<summary>config.yml</summary>
<div>

```yml
version: 2.1
executors:
  base:
    docker:
      - image: circleci/ruby:2.6.0
        environment:
          # Bundlerのパス設定が書き換えられ`vendor/bundle`ではなくて`/usr/local/bundle`を参照してしまい`bundle exec`でエラーになる
          # Bundlerのconfigファイル(pathの設定がされたもの)をworkspaceで永続化し`vendor/bundle`を参照するようにするための設定
          BUNDLE_APP_CONFIG: .bundle
    working_directory: ~/dodonki1223/qiita_trend

commands:
  install-bundler:
    steps:
      - run:
          name: Install bundler(2.1.0)
          command: gem install bundler:2.1.0

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

  save-workspace:
    steps:
      - persist_to_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          root: .
          paths: .

  using-workspace:
    steps:
      - attach_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          at: .

  run-rubocop:
    steps:
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

  run-tests:
    steps:
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard

  collect-reports:
    steps:
      # ref:https://circleci.com/docs/ja/2.0/configuration-reference/#store_test_results
      - store_test_results:
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

  deploy-rubygems:
    steps:
      # ref:https://support.circleci.com/hc/ja/articles/115015628247-%E6%8E%A5%E7%B6%9A%E3%82%92%E7%B6%9A%E8%A1%8C%E3%81%97%E3%81%BE%E3%81%99%E3%81%8B-%E3%81%AF%E3%81%84-%E3%81%84%E3%81%84%E3%81%88-
      # read/write両方の権限が必要
      - add_ssh_keys:
          fingerprints:
            - "38:d2:72:5e:9f:67:93:9a:ec:95:94:a2:0e:bf:41:9e"

      # ref:https://circleci.com/docs/2.0/gh-bb-integration/#establishing-the-authenticity-of-an-ssh-host
      - run:
          name: Avoid hosts unknown for github
          command: |
            mkdir -p ~/.ssh
            echo 'github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
                  bitbucket.org ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAubiN81eDcafrgMeLzaFPsw2kNvEcqTKl/VqLat/MaB33pZy0y3rJZtnqwR2qOOvbwKZYKiEO1O6VqNEBxKvJJelCq0dTXWT5pbO2gDXC6h6QDXCaHo6pOHGPUy+YBaGQRGuSusMEASYiWunYN0vCAI8QaXnWMXNMdFP3jHAJH0eDsoiGnLPBlBp4TNm6rYI74nMzgz3B9IikW4WVK+dc8KZJZWYjAuORU3jc1c/NPskD2ASinf8v3xnfXeukU0sJ5N6m5E8VLjObPEO+mN2t/FZTMZLiFqPWc/ALSqnMnnhwrNi2rbfg/rd/IpL8Le3pSBne8+seeFVBoGqzHM9yXw==
                  ' >> ~/.ssh/known_hosts

      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials
            chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-tests
      - create-document
      - collect-reports

  deploy:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - deploy-rubygems

workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - test:
          requires:
            - setup
      - deploy:
          requires:
            - lint
            - test
          filters:
            branches:
              only: master # masterブランチの時だけDeployJobを実行するようにする
```

</div>
</details>

## 更新内容

workspace の保存と適用の command を追加しそれを job で使用するように更新。

### workspace の保存と適用の command を追加

command に workspace の保存と適用のコマンドをそれぞれ追加する。

```yml
save-workspace:
  steps:
    - persist_to_workspace:
        # working_directory からの相対パスか絶対パスを指定します
        root: .
        paths: .

using-workspace:
  steps:
    - attach_workspace:
        # working_directory からの相対パスか絶対パスを指定します
        at: .
```

### job で workspace を使用する

- setup の job で workspace に保存する
- setup で作成した資材をそれぞれの job で使用する

```yml
jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-tests
      - create-document
      - collect-reports

  deploy:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - deploy-rubygems
```

## workspace の導入でわかったこと

![caching-dependencies-overview](https://circleci.com/docs/assets/img/docs/caching-dependencies-overview.png)

画像は [CircleCIのドキュメント](https://circleci.com/docs/ja/2.0/caching/) より参照

- CircleCI で使用できるキャッシュについて理解できた
- **job のキャッシュ** と **workflow で使い回せるキャッシュ（workspace）** があることを知った

# 2020年4月：Slack通知・デプロイ承認機能の追加

初めて **orb** を導入し slack 通知とデプロイの承認機能を追加。

## config.yml

slack 通知とデプロイの承認機能を追加した config.yml です。

![05_slack_notify](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/05_slack_notify.png)

<details>
<summary>config.yml</summary>
<div>

```yml
version: 2.1
orbs:
  slack: circleci/slack@3.4.2
executors:
  base:
    docker:
      - image: circleci/ruby:2.6.0
        environment:
          # Bundlerのパス設定が書き換えられ`vendor/bundle`ではなくて`/usr/local/bundle`を参照してしまい`bundle exec`でエラーになる
          # Bundlerのconfigファイル(pathの設定がされたもの)をworkspaceで永続化し`vendor/bundle`を参照するようにするための設定
          BUNDLE_APP_CONFIG: .bundle
    working_directory: ~/dodonki1223/qiita_trend

commands:
  install-bundler:
    steps:
      - run:
          name: Install bundler(2.1.0)
          command: gem install bundler:2.1.0

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

  save-workspace:
    steps:
      - persist_to_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          root: .
          paths: .

  using-workspace:
    steps:
      - attach_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          at: .

  run-rubocop:
    steps:
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

  run-tests:
    steps:
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec.xml \
              --format progress \
              $TEST_FILES

  collect-reports:
    steps:
      # ref:https://circleci.com/docs/ja/2.0/configuration-reference/#store_test_results
      - store_test_results:
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results

  deploy-rubygems:
    steps:
      # ref:https://support.circleci.com/hc/ja/articles/115015628247-%E6%8E%A5%E7%B6%9A%E3%82%92%E7%B6%9A%E8%A1%8C%E3%81%97%E3%81%BE%E3%81%99%E3%81%8B-%E3%81%AF%E3%81%84-%E3%81%84%E3%81%84%E3%81%88-
      # read/write両方の権限が必要
      - add_ssh_keys:
          fingerprints:
            - "38:d2:72:5e:9f:67:93:9a:ec:95:94:a2:0e:bf:41:9e"

      # ref:https://circleci.com/docs/2.0/gh-bb-integration/#establishing-the-authenticity-of-an-ssh-host
      - run:
          name: Avoid hosts unknown for github
          command: |
            mkdir -p ~/.ssh
            echo 'github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
                  bitbucket.org ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAubiN81eDcafrgMeLzaFPsw2kNvEcqTKl/VqLat/MaB33pZy0y3rJZtnqwR2qOOvbwKZYKiEO1O6VqNEBxKvJJelCq0dTXWT5pbO2gDXC6h6QDXCaHo6pOHGPUy+YBaGQRGuSusMEASYiWunYN0vCAI8QaXnWMXNMdFP3jHAJH0eDsoiGnLPBlBp4TNm6rYI74nMzgz3B9IikW4WVK+dc8KZJZWYjAuORU3jc1c/NPskD2ASinf8v3xnfXeukU0sJ5N6m5E8VLjObPEO+mN2t/FZTMZLiFqPWc/ALSqnMnnhwrNi2rbfg/rd/IpL8Le3pSBne8+seeFVBoGqzHM9yXw==
                  ' >> ~/.ssh/known_hosts

      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials
            chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

  deploy-notification:
    steps:
      - slack/status:
          success_message: ':circleci-pass: RubyGemsにデプロイが完了しました\n:github_octocat: User: $CIRCLE_USERNAME'
          failure_message: ':circleci-fail: RubyGemsにデプロイが失敗しました\n:github_octocat: User: $CIRCLE_USERNAME'

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-tests
      - collect-reports

  document:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - create-document

  deploy:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - deploy-rubygems
      - deploy-notification

workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - test:
          requires:
            - setup
      - document:
          requires:
            - setup
      - slack/approval-notification:
          message: ':circleci-pass: RubyGemsへのデプロイ準備が整っています\n:github_octocat: User: $CIRCLE_USERNAME\nデプロイを実行する場合は *Approve* を押してください'
          requires:
            - lint
            - test
          filters:
            branches:
              only: master
      - approval-job:
          type: approval
          requires:
            - slack/approval-notification
      - deploy:
          requires:
            - approval-job
          filters:
            branches:
              only: master
```

</div>
</details>

## 更新内容

orbs に slack を追加し承認のプロセスを slack に通知したり、デプロイ完了後に slack に通知させる機能を追加した。

![05_01_slack_notify_sample](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/05_01_slack_notify_sample.png)

### orbs に slack を追加

```yml
version: 2.1
orbs:
  slack: circleci/slack@3.4.2
```

### デプロイ完了通知を追加

- command にデプロイの完了を slack に通知させる

```yml
deploy-notification:
  steps:
    - slack/status:
        success_message: ':circleci-pass: RubyGemsにデプロイが完了しました\n:github_octocat: User: $CIRCLE_USERNAME'
        failure_message: ':circleci-fail: RubyGemsにデプロイが失敗しました\n:github_octocat: User: $CIRCLE_USERNAME'
```

- deploy の job の最後に slack 通知を追加

```yml
deploy:
  executor: base
  steps:
    - using-workspace
    - install-bundler
    - deploy-rubygems
    - deploy-notification
```

### 承認プロセスを追加

- workflow に承認のプロセスを追加
- master ブランチの時のみ実行されるようにする

```yml
workflows:
  version: 2.1
  main:
    jobs:
      ・
      ・
      ・
      - slack/approval-notification:
          message: ':circleci-pass: RubyGemsへのデプロイ準備が整っています\n:github_octocat: User: $CIRCLE_USERNAME\nデプロイを実行する場合は *Approve* を押してください'
          requires:
            - lint
            - test
          filters:
            branches:
              only: master
      ・
      ・
      ・
```

# 2020年8月：毎日テストを実行させるワークフローを追加

[QiitaTrend](https://github.com/dodonki1223/qiita_trend) という gem は Qiita のサイトをスクレイピングしているため、DOM の構成が変わるとエラーで落ちてしまいます。
いつの間にかテストが落ちていることが何度かあり gem が使用できなくなることがあった。
**DOM の構成が変わったことをいち早く検知** する必要があったため **毎日テストを実行する** ように修正。

## config.yml

テストが定期実行するように設定を追加した config.yml です。

![06_triggered_execute](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/06_triggered_execute.png)

<details>
<summary>config.yml</summary>
<div>

```yml
version: 2.1
orbs:
  slack: circleci/slack@3.4.2
executors:
  base:
    docker:
      - image: circleci/ruby:2.6.0
        auth:
          username: dodonki1223
          password: $DOCKERHUB_PASSWORD
        environment:
          # Bundlerのパス設定が書き換えられ`vendor/bundle`ではなくて`/usr/local/bundle`を参照してしまい`bundle exec`でエラーになる
          # Bundlerのconfigファイル(pathの設定がされたもの)をworkspaceで永続化し`vendor/bundle`を参照するようにするための設定
          BUNDLE_APP_CONFIG: .bundle
          # ref: https://circleci.com/docs/2.0/faq/#how-can-i-set-the-timezone-in-docker-images
          TZ: "Asia/Tokyo"
    working_directory: ~/dodonki1223/qiita_trend

commands:
  install-bundler:
    steps:
      - run:
          name: Install bundler(2.1.0)
          command: gem install bundler:2.1.0

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

  save-workspace:
    steps:
      - persist_to_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          root: .
          paths: .

  using-workspace:
    steps:
      - attach_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          at: .

  run-rubocop:
    steps:
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

  run-tests:
    steps:
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec/rspec.xml \
              --format progress \
              $TEST_FILES

  collect-reports:
    steps:
      # ref:https://circleci.com/docs/ja/2.0/configuration-reference/#store_test_results
      - store_test_results:
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results

  deploy-rubygems:
    steps:
      # ref:https://support.circleci.com/hc/ja/articles/115015628247-%E6%8E%A5%E7%B6%9A%E3%82%92%E7%B6%9A%E8%A1%8C%E3%81%97%E3%81%BE%E3%81%99%E3%81%8B-%E3%81%AF%E3%81%84-%E3%81%84%E3%81%84%E3%81%88-
      # read/write両方の権限が必要
      - add_ssh_keys:
          fingerprints:
            - "38:d2:72:5e:9f:67:93:9a:ec:95:94:a2:0e:bf:41:9e"

      # ref:https://circleci.com/docs/2.0/gh-bb-integration/#establishing-the-authenticity-of-an-ssh-host
      - run:
          name: Avoid hosts unknown for github
          command: |
            mkdir -p ~/.ssh
            echo 'github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
                  bitbucket.org ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAubiN81eDcafrgMeLzaFPsw2kNvEcqTKl/VqLat/MaB33pZy0y3rJZtnqwR2qOOvbwKZYKiEO1O6VqNEBxKvJJelCq0dTXWT5pbO2gDXC6h6QDXCaHo6pOHGPUy+YBaGQRGuSusMEASYiWunYN0vCAI8QaXnWMXNMdFP3jHAJH0eDsoiGnLPBlBp4TNm6rYI74nMzgz3B9IikW4WVK+dc8KZJZWYjAuORU3jc1c/NPskD2ASinf8v3xnfXeukU0sJ5N6m5E8VLjObPEO+mN2t/FZTMZLiFqPWc/ALSqnMnnhwrNi2rbfg/rd/IpL8Le3pSBne8+seeFVBoGqzHM9yXw==
                  ' >> ~/.ssh/known_hosts

      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials
            chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

  deploy-notification:
    steps:
      - slack/status:
          success_message: ':circleci-pass: RubyGemsにデプロイが完了しました\n:github_octocat: User: $CIRCLE_USERNAME'
          failure_message: ':circleci-fail: RubyGemsにデプロイが失敗しました\n:github_octocat: User: $CIRCLE_USERNAME'

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-tests
      - collect-reports

  document:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - create-document

  deploy:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - deploy-rubygems
      - deploy-notification

workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - test:
          requires:
            - setup
      - document:
          requires:
            - setup
      - slack/approval-notification:
          message: ':circleci-pass: RubyGemsへのデプロイ準備が整っています\n:github_octocat: User: $CIRCLE_USERNAME\nデプロイを実行する場合は *Approve* を押してください'
          requires:
            - lint
            - test
          filters:
            branches:
              only: master
      - approval-job:
          type: approval
          requires:
            - slack/approval-notification
      - deploy:
          requires:
            - approval-job
          filters:
            branches:
              only: master
  # 定期でテストを実行する
  # ref:https://circleci.com/docs/ja/2.0/triggers/
  nightly:
    triggers:
      - schedule:
          cron: "0 22 * * *" # UTCで記述
          filters:
            branches:
              only:
                - master
    jobs:
      - setup
      - test:
          requires:
            - setup
```

</div>
</details>

## 更新内容

RSpec を毎日実行しテストが落ちない限りは Qiita のトレンド情報を取得できることを担保する。

### テストを毎日実行させる

- [triggers](https://circleci.com/docs/ja/2.0/triggers/) を使用し毎日、朝の7時にテストが実行されるようにする

```yml
workflows:
  version: 2.1
  # 定期でテストを実行する
  # ref:https://circleci.com/docs/ja/2.0/triggers/
  nightly:
    triggers:
      - schedule:
          cron: "0 22 * * *" # UTCで記述
          filters:
            branches:
              only:
                - master
    jobs:
      - setup
      - test:
          requires:
            - setup
```

## 毎日実行するようになってどうなった？

- テストが何度か落ちて、DOMの構成が変わったことをいち早く検知することができるようになった
- テストが落ちた時は CircleCI と slack を連携させているため通知が来てすぐに検知が可能

![06_01_error_sample](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/06_01_error_sample.png)

こんな感じでエラーが slack に通知される


# 2020年10月：CircleCI のイメージを次世代ものに変更

[CircleCI実践入門──CI/CDがもたらす開発速度と品質の両立](https://www.amazon.co.jp/CircleCI%E5%AE%9F%E8%B7%B5%E5%85%A5%E9%96%80%E2%94%80%E2%94%80CI-CD%E3%81%8C%E3%82%82%E3%81%9F%E3%82%89%E3%81%99%E9%96%8B%E7%99%BA%E9%80%9F%E5%BA%A6%E3%81%A8%E5%93%81%E8%B3%AA%E3%81%AE%E4%B8%A1%E7%AB%8B-WEB-PRESS-plus/dp/4297114119) の本が発売されたので読んでみると cimg という次世代の image があることを知り、こちらを導入することにした。

## config.yml

cimg に変更した config.yml

![07_change_cimg](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/07_change_cimg.png)

<details>
<summary>config.yml</summary>
<div>

```yml
version: 2.1
orbs:
  slack: circleci/slack@3.4.2
executors:
  base:
    docker:
      - image: cimg/ruby:2.6.6
        auth:
          username: dodonki1223
          password: $DOCKERHUB_PASSWORD
        environment:
          # Bundlerのパス設定が書き換えられ`vendor/bundle`ではなくて`/usr/local/bundle`を参照してしまい`bundle exec`でエラーになる
          # Bundlerのconfigファイル(pathの設定がされたもの)をworkspaceで永続化し`vendor/bundle`を参照するようにするための設定
          BUNDLE_APP_CONFIG: .bundle
          # ref: https://circleci.com/docs/2.0/faq/#how-can-i-set-the-timezone-in-docker-images
          TZ: "Asia/Tokyo"
    working_directory: ~/dodonki1223/qiita_trend

commands:
  install-bundler:
    steps:
      - run:
          name: Install bundler(2.1.0)
          command: gem install bundler:2.1.0

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}

  save-workspace:
    steps:
      - persist_to_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          root: .
          paths: .

  using-workspace:
    steps:
      - attach_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          at: .

  run-rubocop:
    steps:
      - run:
          name: Run RuboCop
          command: |
            bundle exec rubocop

  run-tests:
    steps:
      - run:
          name: Run tests
          command: |
            TEST_FILES="$(circleci tests glob "spec/**/*_spec.rb" | circleci tests split --split-by=timings)"
            bundle exec rspec \
              --format progress \
              --format RspecJunitFormatter \
              --out test_results/rspec/rspec.xml \
              --format progress \
              $TEST_FILES

  collect-reports:
    steps:
      # ref:https://circleci.com/docs/ja/2.0/configuration-reference/#store_test_results
      - store_test_results:
          path: test_results
      - store_artifacts:
          # テスト結果をtest-resultsディレクトリに吐き出す
          path: test_results
          destination: test-results
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results

  deploy-rubygems:
    steps:
      # ref:https://support.circleci.com/hc/ja/articles/115015628247-%E6%8E%A5%E7%B6%9A%E3%82%92%E7%B6%9A%E8%A1%8C%E3%81%97%E3%81%BE%E3%81%99%E3%81%8B-%E3%81%AF%E3%81%84-%E3%81%84%E3%81%84%E3%81%88-
      # read/write両方の権限が必要
      - add_ssh_keys:
          fingerprints:
            - "38:d2:72:5e:9f:67:93:9a:ec:95:94:a2:0e:bf:41:9e"

      # ref:https://circleci.com/docs/2.0/gh-bb-integration/#establishing-the-authenticity-of-an-ssh-host
      - run:
          name: Avoid hosts unknown for github
          command: |
            mkdir -p ~/.ssh
            echo 'github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
                  bitbucket.org ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAubiN81eDcafrgMeLzaFPsw2kNvEcqTKl/VqLat/MaB33pZy0y3rJZtnqwR2qOOvbwKZYKiEO1O6VqNEBxKvJJelCq0dTXWT5pbO2gDXC6h6QDXCaHo6pOHGPUy+YBaGQRGuSusMEASYiWunYN0vCAI8QaXnWMXNMdFP3jHAJH0eDsoiGnLPBlBp4TNm6rYI74nMzgz3B9IikW4WVK+dc8KZJZWYjAuORU3jc1c/NPskD2ASinf8v3xnfXeukU0sJ5N6m5E8VLjObPEO+mN2t/FZTMZLiFqPWc/ALSqnMnnhwrNi2rbfg/rd/IpL8Le3pSBne8+seeFVBoGqzHM9yXw==
                  ' >> ~/.ssh/known_hosts

      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials
            chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

  deploy-notification:
    steps:
      - slack/status:
          success_message: ':circleci-pass: RubyGemsにデプロイが完了しました\n:github_octocat: User: $CIRCLE_USERNAME'
          failure_message: ':circleci-fail: RubyGemsにデプロイが失敗しました\n:github_octocat: User: $CIRCLE_USERNAME'

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-tests
      - collect-reports

  document:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - create-document

  deploy:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - deploy-rubygems
      - deploy-notification

workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - test:
          requires:
            - setup
      - document:
          requires:
            - setup
      - slack/approval-notification:
          message: ':circleci-pass: RubyGemsへのデプロイ準備が整っています\n:github_octocat: User: $CIRCLE_USERNAME\nデプロイを実行する場合は *Approve* を押してください'
          requires:
            - lint
            - test
          filters:
            branches:
              only: master
      - approval-job:
          type: approval
          requires:
            - slack/approval-notification
      - deploy:
          requires:
            - approval-job
          filters:
            branches:
              only: master
  # 定期でテストを実行する
  # ref:https://circleci.com/docs/ja/2.0/triggers/
  nightly:
    triggers:
      - schedule:
          cron: "0 22 * * *" # UTCで記述
          filters:
            branches:
              only:
                - master
    jobs:
      - setup
      - test:
          requires:
            - setup
```

</div>
</details>

## 更新内容

次世代の image を使用して executors を作成するように修正。

### cimg に変更

- cimg/ruby:2.6.6 を使用するように修正

```yml
version: 2.1
orbs:
  slack: circleci/slack@3.4.2
executors:
  base:
    docker:
      - image: cimg/ruby:2.6.6
        auth:
          username: dodonki1223
          password: $DOCKERHUB_PASSWORD
        environment:
          # Bundlerのパス設定が書き換えられ`vendor/bundle`ではなくて`/usr/local/bundle`を参照してしまい`bundle exec`でエラーになる
          # Bundlerのconfigファイル(pathの設定がされたもの)をworkspaceで永続化し`vendor/bundle`を参照するようにするための設定
          BUNDLE_APP_CONFIG: .bundle
          # ref: https://circleci.com/docs/2.0/faq/#how-can-i-set-the-timezone-in-docker-images
          TZ: "Asia/Tokyo"
    working_directory: ~/dodonki1223/qiita_trend
```

# 2020年11月：Ruby の orb を導入

Ruby の orb を使用することで自前で実装していた処理を orb で提供された command 使用することができるようになり記述の簡略化ができるようになった。

## config.yml

Ruby の orb に寄せた config.yml です。

![08_change_ruby_orb](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/16/08_change_ruby_orb.png)

<details>
<summary>config.yml</summary>
<div>

```yml
version: 2.1
orbs:
  ruby: circleci/ruby@1.1.2
  slack: circleci/slack@3.4.2
executors:
  base:
    docker:
      - image: cimg/ruby:2.6.6
        auth:
          username: dodonki1223
          password: $DOCKERHUB_PASSWORD
        environment:
          # Bundlerのパス設定が書き換えられ`vendor/bundle`ではなくて`/usr/local/bundle`を参照してしまい`bundle exec`でエラーになる
          # Bundlerのconfigファイル(pathの設定がされたもの)をworkspaceで永続化し`vendor/bundle`を参照するようにするための設定
          BUNDLE_APP_CONFIG: .bundle
          # ref: https://circleci.com/docs/2.0/faq/#how-can-i-set-the-timezone-in-docker-images
          TZ: "Asia/Tokyo"
    working_directory: ~/dodonki1223/qiita_trend

commands:
  save-workspace:
    steps:
      - persist_to_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          root: .
          paths: .

  using-workspace:
    steps:
      - attach_workspace:
          # working_directory からの相対パスか絶対パスを指定します
          at: .

  collect-reports:
    steps:
      - store_artifacts:
          # カバレッジの結果をcoverage-resultsディレクトリに吐き出す
          path: coverage
          destination: coverage-results

  create-document:
    steps:
      - run:
          name: Create document
          command: |
            bundle exec yard
      - store_artifacts:
          # ドキュメントの結果をyard-resultsディレクトリに吐き出す
          path: ./doc
          destination: yard-results

  deploy-rubygems:
    steps:
      # https://discuss.circleci.com/t/the-authenticity-of-github-host-cant-be-stablished/33133 と同じ現象で job が進まなくなるので以下の記事を参考に実装
      # ref:https://circleci.com/docs/ja/2.0/gh-bb-integration/#ssh-%E3%83%9B%E3%82%B9%E3%83%88%E3%81%AE%E4%BF%A1%E9%A0%BC%E6%80%A7%E3%81%AE%E7%A2%BA%E7%AB%8B

      - run:
          name: Avoid hosts unknown for github
          command: |
            mkdir -p ~/.ssh
            echo 'github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
                  ' >> ~/.ssh/known_hosts

      - run:
          name: Deploy RubyGems
          command: |
            curl -u dodonki1223:$RUBYGEMS_PASSWORD https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials
            chmod 0600 ~/.gem/credentials
            git config user.name dodonki1223
            git config user.email $RUBYGEMS_EMAIL
            bundle exec rake build
            bundle exec rake release

  deploy-notification:
    steps:
      - slack/status:
          success_message: ':circleci-pass: RubyGemsにデプロイが完了しました\n:github_octocat: User: $CIRCLE_USERNAME'
          failure_message: ':circleci-fail: RubyGemsにデプロイが失敗しました\n:github_octocat: User: $CIRCLE_USERNAME'

jobs:
  setup:
    executor: base
    steps:
      - checkout
      - ruby/install-deps
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - ruby/rubocop-check

  test:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - ruby/rspec-test:
          out-path: 'test_results/rspec/'
      - collect-reports

  document:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - create-document

  deploy:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - deploy-rubygems
      - deploy-notification

workflows:
  version: 2.1
  main:
    jobs:
      - setup
      - lint:
          requires:
            - setup
      - test:
          requires:
            - setup
      - document:
          requires:
            - setup
      - slack/approval-notification:
          message: ':circleci-pass: RubyGemsへのデプロイ準備が整っています\n:github_octocat: User: $CIRCLE_USERNAME\nデプロイを実行する場合は *Approve* を押してください'
          requires:
            - lint
            - test
          filters:
            branches:
              only: master
      - approval-job:
          type: approval
          requires:
            - slack/approval-notification
      - deploy:
          requires:
            - approval-job
          filters:
            branches:
              only: master
  # 定期でテストを実行する
  # ref:https://circleci.com/docs/ja/2.0/triggers/
  nightly:
    triggers:
      - schedule:
          cron: "0 22 * * *" # UTCで記述
          filters:
            branches:
              only:
                - master
    jobs:
      - setup
      - test:
          requires:
            - setup
```

</div>
</details>

## 更新内容

### Ruby の orb の導入

- circleci/ruby@1.1.2 を使用するように変更

```yml
version: 2.1
orbs:
  ruby: circleci/ruby@1.1.2
```

### orbs で定義されている command と同等の処理を削除

- Bundler のインストール、 gem のインストールの処理を削除
- 以下の処理は Ruby の orb で定義されているのですべて削除し代替の処理に変更する

```yml
  install-bundler:
    steps:
      - run:
          name: Install bundler(2.1.0)
          command: gem install bundler:2.1.0

  # Read about caching dependencies: https://circleci.com/docs/2.0/caching/
  restore-gem-cache:
    steps:
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "Gemfile.lock" }}
            # fallback to using the latest cache if no exact match is found
            - v1-dependencies-

  install-gem:
    steps:
      - run:
          name: Install gem
          command: |
            # jobs=4は並列処理をして高速化するための設定（４つのjobで実行するって意味）
            bundle check --path=vendor/bundle || bundle install --path=vendor/bundle --jobs=4 --retry=3

  save-gem-cache:
    steps:
      - save_cache:
          paths:
            - ./vendor/bundle
          key: v1-dependencies-{{ checksum "Gemfile.lock" }}
```

### 削除した処理を Ruby の orb の command に変更する

- orb に変更する前と変更後の記述

#### 修正前

```yml
jobs:
  setup:
    executor: base
    steps:
      - checkout
      - install-bundler
      - restore-gem-cache
      - install-gem
      - save-gem-cache
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - run-tests
      - collect-reports

  document:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - create-document

  deploy:
    executor: base
    steps:
      - using-workspace
      - install-bundler
      - deploy-rubygems
      - deploy-notification
```

#### 修正後

```yml
jobs:
  setup:
    executor: base
    steps:
      - checkout
      - ruby/install-deps
      - save-workspace

  lint:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - run-rubocop

  test:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - run-tests
      - collect-reports

  document:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - create-document

  deploy:
    executor: base
    steps:
      - using-workspace
      - ruby/install-deps
      - deploy-rubygems
      - deploy-notification
```

# 最後に

CircleCI で CI/CD を設定して運用してきたが自分の学びが多く、実装してすごく良かったと思う。
個人で CircleCI を運用することはすごく勉強になるので個人的にはすごくオススメします。
これからもこの設定ファイルを運用していき、気になった機能が増えたらすぐに実装していきたいと思います！
