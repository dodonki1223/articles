# 大吾「ワシ、ssh接続忘れるんだが……きさぁーまは？」、ノブ「貴様！？」

最近、某CMがすごくお気に入りです（笑）

複数のサーバーへssh接続する時、サーバー名を忘れてしまい設定ファイルや人に聞いたりすることがよくありました。
というか接続先が大量にあると忘れるじゃぁあああああああああああ。

## コマンドがめんどくさい

公開鍵認証の場合は下記のようになると思います。
※ポート番号がデフォルト（22の時）の場合は`-p ポート番号`は指定しなくても良いです。

ポート番号を指定する時

```shell
ssh -i ~/.ssh/鍵パス ユーザー名@ホスト名 -p ポート番号
```

ポート番号を指定しない時

```shell
ssh -i ~/.ssh/鍵パス ユーザー名@ホスト名 
```

記憶力皆無の私には上記のコマンドをいちいち打つなんてめんどくさいし覚えられない……。

## わかりやすくする

わかりやすくするために考えられる方法

1. エイリアスを設定する
1. .ssh/configファイルを作成する。

## エイリアスを設定する

#### .bashrcファイルにエイリアスの設定をする

.bashrcファイルにサーバー毎のエイリアスを下記のように設定していく

```shell
alias host1='ssh -i ~/.ssh/鍵パス ユーザー@host1 -p 22'
alias host2='ssh -i ~/.ssh/鍵パス ユーザー@host2 -p 22'
alias host3='ssh -i ~/.ssh/鍵パス ユーザー@host3 -p 22'
alias host4='ssh -i ~/.ssh/鍵パス ユーザー@host4 -p 22'
alias host5='ssh -i ~/.ssh/鍵パス ユーザー@host5 -p 22'
alias host6='ssh -i ~/.ssh/鍵パス ユーザー@host6 -p 22'
alias host7='ssh -i ~/.ssh/鍵パス ユーザー@host7 -p 22'
```

#### 実際に実行してみる

下記のようにすれば接続出来るようになるだろう

```shell
$ host1
```

この方式だとエイリアスを覚えないといけなくて記憶力皆無の私には……以下同文。

## .ssh/configを使用してssh接続の簡略化

[.ssh/configファイルでSSH接続を管理する - Qiita](https://qiita.com/0084ken/items/2e4e9ae44ec5e01328f1)に詳しく.ssh/configファイルの作成方法が書かれています。

#### .sshフォルダの作成

```shell
$ mkdir ~/.ssh
$ chmod 700 .ssh
$ touch ~/.ssh/config
```

#### .ssh/configファイルにサーバー設定を追加

```
# サーバー１
Host host1
    HostName 999.999.999.999
    User user1
    IdentityFile ~/.ssh/鍵ファイル
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes

# サーバー２
Host host2
    HostName 888.888.888.888
    User user2
    IdentityFile ~/.ssh/鍵ファイル
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes

# サーバー３
Host host3
    HostName 777.777.777.777
    User user3
    IdentityFile ~/.ssh/鍵ファイル
    Port 22
    TCPKeepAlive yes
    IdentitiesOnly yes
```

#### 実際に実行してみる

```shell
$ ssh host1
```

この方式だとHostの名称を覚えないといけなくて記憶力皆無の私には……以下同文。

## 第３の方法を提示（ssh接続シェルスクリプト化）

とりあえず見ていただこう

![Feb-04-2019 10-36-04.gif](https://qiita-image-store.s3.amazonaws.com/0/299904/52c8b94d-37a9-8a13-3803-5cc5fbe1fc3a.gif)


如何でしょうか？
このシェルスクリプトなら実行するだけで接続先のサーバーを表示してくれて、接続したいサーバーの番号を入力するだけで接続することが出来ます。
記憶力皆無の私にはうってつけのツールである。
このシェルスクリプトをエイリアスに登録すればたった１つのコマンドを覚えるだけですべてのサーバーに接続できるようになるわけです。

### シェルスクリプトの構成

シェルスクリプトの構成は下記のようになっています。

```
ssh_connection/
├── README.md               説明ファイル
├── server_connection.conf  設定ファイル
└── ssh_connection.sh       シェルスクリプト
```

### シェルスクリプトの設定ファイルについて

設定ファイルがあるのでそれを読み込み、その内容に応じて接続先サーバーの一覧を表示しています。
設定ファイルは下記のような形式になっています。

```ssh_connection/server_connection.conf
サーバー１,/Users/test/.ssh/test.cer,ec2-user@999.999.999.999,22
サーバー２,/Users/test/.ssh/test.cer,ec2-user@888.888.888.888,22
サーバー３,/Users/test/.ssh/test.cer,ec2-user@777.777.777.777,22
```

下記のような形式で設定ファイルに記述します。

```
サーバー名,鍵ファイルへのパス,ユーザー名@サーバーのホスト,ポート番号
```

### シェルスクリプトの中身

```shell
#!/bin/sh

#***********************************************************************************
#* バッチ名：ssh接続ツール
#* 処理内容：接続先設定ファイルを読み込み、その内容を画面に表示しどの環境に接続するか
#*           ユーザーに対話し接続先を指定するツール
#* 使用方法：このシェルスクリプトと同階層にあるserver_connection_info.confのファイル
#*           にサーバーの接続先情報を設定して下さい
#*           設定ファイルにはカンマ区切りで下記のように設定して下さい
#*             環境名,sshキーファイルの場所,サーバー情報(「ユーザー名@IPアドレス」形式),ポート番号
#***********************************************************************************

# このシェルスクリプトの実行パスを取得する
scriptPath=$(cd $(dirname ${BASH_SOURCE:-$0}); pwd)

# 接続先設定ファイル
connection_setting=$scriptPath/server_connection.conf

#*****************************************************************************
#* 処 理 名：接続先の環境選択処理
#* 引    数：なし
#* 処理内容：不正な値を入力した時は、エラーを表示
#*           targetNumber変数に対象の環境番号をセットする
#*           ※接続先設定ファイルで読み込んだ環境ごとに番号を１から割り振って
#*             あり、その内容を元に入力された番号が存在するかチェックする
#*           ※$targetNumber、$numbersはグローバル変数です
#*****************************************************************************
inputEnvironmentNumber()
{
    read -p "接続したい環境の番号を入力して下さい: " targetNumber
    isExistsNumber ${numbers[*]}
    result=$?
    echo
    return $result
}

#*****************************************************************************
#* 処 理 名：入力された番号が存在するか
#* 引    数：なし
#* 処理内容：画面で入力された番号が存在するかどうかチェックする
#*           存在する場合は1を返し、存在しない場合はメッセージを表示し0を返す
#*           ※$targetNumber、$numbersはグローバル変数です
#*****************************************************************************
isExistsNumber()
{

    # 入力された値が空文字の時は入力値が不正と判断する
    if [ -z "${targetNumber}" ]; then
        echo "入力値が不正です。正しい値を入力して下さい！！"
        return 0
    fi

    # 配列内に対象の番号があるかどうかチェック
    for number in ${numbers[*]}
    do
        if  [ $number = $targetNumber ]; then
            return 1
        fi
    done

    # 配列内に対象の番号が存在しなかった時
    echo "入力値が不正です。正しい値を入力して下さい！！"
    return 0
}

# タイトルの表示
echo -----------------------------------------------------------
echo \<ssh接続ツール\>
echo 接続先のサーバーの一覧が表示されるので、接続したいサーバー
echo の番号を入力して下さい。入力した番号のサーバーにssh接続しま
echo す。サーバーのリストを編集したい時はserver_connection.conf
echo を修正して下さい。
echo -----------------------------------------------------------

# 接続先設定ファイルの読み込み処理
# ※それぞれ配列にセットする
numbers=(`awk -F'[,]' '{print NR}' "${connection_setting}"`)
environments=(`awk -F'[,]' '{print $1}' "${connection_setting}"`)
sshkeys=(`awk -F'[,]' '{print $2}' "${connection_setting}"`)
servers=(`awk -F'[,]' '{print $3}' "${connection_setting}"`)
ports=(`awk -F'[,]' '{print $4}' "${connection_setting}"`)

# 接続先の環境の一覧を画面に表示
echo
echo \<接続先環境の一覧\>
environmentCount=0
for environment in ${environments[*]}
do
    echo ${numbers[$environmentCount]}：$environment
    environmentCount=$((environmentCount+1))
done

# 接続先環境の入力処理
echo
inputEnvironmentNumber
result=$?
while [ $result = 0 ]
do
    inputEnvironmentNumber
    result=$?
done

# 配列と一致させるために添え字の番号を調節
targetSubscript=$(($targetNumber-1))
echo ---------------------------------------------
echo \<接続先情報\>
echo 接続先名　　　：${environments[$targetSubscript]}
echo ＳＳＨキー情報：${sshkeys[$targetSubscript]}
echo サーバー情報　：${servers[$targetSubscript]}
echo ポート番号　　：${ports[$targetSubscript]}
echo ---------------------------------------------
echo

# 処理を続行するかユーザーに確認
echo 上記のサーバーへ下記のコマンドを実行して接続します。よろしいですか？
echo ssh -i ${sshkeys[$targetSubscript]} ${servers[$targetSubscript]} -p ${ports[$targetSubscript]}
echo
echo 「Enter」キーを押すとそのまま処理が続行されます
echo 処理をやめる時はCtrl+Cで終了して下さい……
echo
read -p "Hit enter: "
echo

# サーバー接続処理の実行
ssh -i ${sshkeys[$targetSubscript]} ${servers[$targetSubscript]} -p ${ports[$targetSubscript]}

```

### シェルスクリプトの中身の説明

#### 設定ファイルの読み込み処理

設定ファイルはawkコマンドを使用して読み込みます。

```shell
numbers=(`awk -F'[,]' '{print NR}' "${connection_setting}"`)      # 一意の番号
environments=(`awk -F'[,]' '{print $1}' "${connection_setting}"`) # サーバー名
sshkeys=(`awk -F'[,]' '{print $2}' "${connection_setting}"`)      # 鍵ファイルへのパス
servers=(`awk -F'[,]' '{print $3}' "${connection_setting}"`)      # 接続先サーバー情報（ユーザー名@ホスト名）
ports=(`awk -F'[,]' '{print $4}' "${connection_setting}"`)        # ポート番号
```

#### 接続先のサーバーの一覧の表示

一意の番号とサーバー名を`一意の番号：サーバー名`の形式で表示する

```shell
# 接続先のサーバーの一覧を画面に表示
echo
echo \<接続先環境の一覧\>
environmentCount=0
for environment in ${environments[*]}
do
    echo ${numbers[$environmentCount]}：$environment
    environmentCount=$((environmentCount+1))
done
```

下記のような感じで表示されます

```shell
<接続先環境の一覧>
1：サーバー１
2：サーバー２
3：サーバー３
```

#### 接続先サーバーの入力処理

接続先サーバーの番号を受け取り、その番号が存在する時は処理を続行し存在しない時はもう一度、接続先サーバーの入力処理を行なう

```shell
# 接続先環境の入力処理
echo
inputEnvironmentNumber
result=$?
while [ $result = 0 ]
do
    inputEnvironmentNumber
    result=$?
done
```

`inputEnvironmentNumber`関数が接続先サーバーの入力処理です。
`inputEnvironmentNumber`関数から結果を受け取り、存在する番号だった時は`$result`に`1`が返り、存在しない番号だった場合は`0`が返ってきます。
`$result`が`0`だったら`inputEnvironmentNumber`の処理を何回でも実行します。

`inputEnvironmentNumber`関数の中身です。
この関数はユーザーからの入力値を受け取り、その番号を`isExistsNumber`関数を使用して接続先サーバーに値が存在するかどうかを判断しています。

```shell
inputEnvironmentNumber()
{
    read -p "接続したい環境の番号を入力して下さい: " targetNumber
    isExistsNumber ${numbers[*]}
    result=$?
    echo
    return $result
}
```

`isExistsNumber`という関数は接続先サーバーの番号の存在チェックを行なう関数です。
ユーザーからの入力値の空文字の判定を行い、その後入力値が存在するものかどうかをfor文を使用して存在チェックしています。

```shell
isExistsNumber()
{

    # 入力された値が空文字の時は入力値が不正と判断する
    if [ -z "${targetNumber}" ]; then
        echo "入力値が不正です。正しい値を入力して下さい！！"
        return 0
    fi

    # 配列内に対象の番号があるかどうかチェック
    for number in ${numbers[*]}
    do
        if  [ $number = $targetNumber ]; then
            return 1
        fi
    done

    # 配列内に対象の番号が存在しなかった時
    echo "入力値が不正です。正しい値を入力して下さい！！"
    return 0
}
```

#### sshコマンドの実行処理

ユーザーからの入力値をawkコマンドで取得した設定値と合わせるために`-1`します。
その後はsshの接続先コマンドを作成し、実行コマンドを画面に表示しsshコマンドを実行します。

入力待ちの処理はこちらを参考にしました。
[BASHシェルスクリプトで「キー入力待ち」プロンプトを実装する ｜ DevelopersIO](https://dev.classmethod.jp/etc/waiting-for-your-input-with-read-command/)

```shell
# 配列と一致させるために添え字の番号を調節
targetSubscript=$(($targetNumber-1))
echo ---------------------------------------------
echo \<接続先情報\>
echo 接続先名　　　：${environments[$targetSubscript]}
echo ＳＳＨキー情報：${sshkeys[$targetSubscript]}
echo サーバー情報　：${servers[$targetSubscript]}
echo ポート番号　　：${ports[$targetSubscript]}
echo ---------------------------------------------
echo

# 処理を続行するかユーザーに確認
echo 上記のサーバーへ下記のコマンドを実行して接続します。よろしいですか？
echo ssh -i ${sshkeys[$targetSubscript]} ${servers[$targetSubscript]} -p ${ports[$targetSubscript]}
echo
echo 「Enter」キーを押すとそのまま処理が続行されます
echo 処理をやめる時はCtrl+Cで終了して下さい……
echo
read -p "Hit enter: "
echo

# サーバー接続処理の実行
ssh -i ${sshkeys[$targetSubscript]} ${servers[$targetSubscript]} -p ${ports[$targetSubscript]}
```

## 最後に

これで記憶力皆無の私でもこのシェルスクリプトにエイリアスを設定するだけでたった１つのコマンドを覚えるだけですべてのサーバーに簡単に接続することが出来るようになりました。

紹介したシェルスクリプトですが、下記にありますのでもしよかったら使用してみて下さい。

[https://github.com/dodonki1223/ShellScript/tree/master/ssh_connection](https://github.com/dodonki1223/ShellScript/tree/master/ssh_connection)
