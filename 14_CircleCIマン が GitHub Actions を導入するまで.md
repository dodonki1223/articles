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

![01_get_started_with_github_actions](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/01_get_started_with_github_actions.png)

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

![02_edit_new_file](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/02_edit_new_file.png)

最後に **Start commit** をクリックして **Commit new file** をクリックしてリポジトリにコミットします

![03_first_commit](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/03_first_commit.png)

## 実行結果を確認する

再度、 **Actions** をクリックし Workflows の一覧から先程作成したWorkflowsのコミットの **Create ruby.yml** をクリックします

![04_created_workflows](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/04_created_workflows.png)

左側の **test** をクリックし詳細を確認します

![05_before_click_test](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/05_before_click_test.png)

CIが実行中の場合です

![06_click_test](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/06_click_test.png)

CIが完了すると以下のような画面になります

![07_test_complted](https://raw.githubusercontent.com/dodonki1223/image_garage/master/qiita_articles/14/07_test_complted.png)

以上で簡単に RSpec を実行するだけの CI を実装することができました



















