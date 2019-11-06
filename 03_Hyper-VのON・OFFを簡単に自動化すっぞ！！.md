# Hyper-VのON・OFFを簡単に自動化すっぞ！！

Androidのエミュレータ（[NoxPlayer](https://jp.bignox.com/)）を使用したい時、Hyper-VがONだとブルースクリーンになって使用できない。
「Docker for Windows」使用したいし、Hyper-VのON・OFFを手動でするのがものすごくめんどくさいぞ！！
よし！！ 自動化だ！！
ということでこの記事はHyper-VのON・OFFを簡単に自動化するための記事になります。
Hyper-Vがインストールされていることが前提になりますのでインストールされていない人はインストールしてからこの記事に戻ってきて下さい。

## Hyper-VがONなのかOFFなのか現在の状態を調べる

「BCDEdit」を使用してHyper-VがON・OFFなのか現在の状態を調べます
※[BCDEditの公式ドキュメントはこちら](https://msdn.microsoft.com/ja-jp/library/windows/hardware/mt450468(v=vs.85).aspx)

コマンドプロンプトを管理者で実行して下記コマンドを実行して下さい

**Hyper-Vの現在の状態を確認する**

```bat
bcdedit /enum | find "hypervisorlaunchtype"
```

Hyper-VがONの時は下記のように出力されます

```bat
hypervisorlaunchtype    Auto
```

Hyper-VがOFFの時は下記のように出力されます

```bat
hypervisorlaunchtype    Off
```

## Hyper-VをON・OFFする

「BCDEdit」を使用してHyper-VをON・OFFにします

コマンドプロンプトを管理者で実行して下記コマンドを実行して下さい

**Hyper-VをONにする**

```bat
bcdedit /set hypervisorlaunchtype auto
```

**Hyper-VをOFFにする**

```bat
bcdedit /set hypervisorlaunchtype off
```

上記コマンドを実行後、**再起動**をすることでHyper-VがON・OFFになります

## 再起動をコマンドで実行する

コマンドプロンプトで下記コマンドを実行してください
※下記コマンドを実行すると再起動してしまうので注意

```bat
shutdown.exe /r /t 0
```

## Hyper-Vを自動化するためのバッチを作成します

Hyper-VをON・OFFにするためには管理者でコマンドプロンプトを実行しないといけません。
batファイルを管理者で実行するにはどうしたらいいの？
すでに調べてくれている人がいました。下記記事を参考にします。

[管理者権限でbatを実行したい時にやッた事](https://qiita.com/resolver/items/7187bd6ee8016ee5c741)

Hyper-VをONにするbatファイル、Hyper-VをOFFにするbatファイル、Hyper-Vの現在の状態を確認するbatファイルを作成し、作成したbatファイルを下記のようにして呼び出すbatファイルを作成します。

```bat
powershell start-process 作成したbatファイル -verb runas
```

これだけ情報があれば後は作るだけです……作るのめんどくさいですよね。
もう既に私が作成しているのでそれを使って下さい。

## Hyper-VをON・OFFにするバッチ

私のGithubのリポジトリにありますので取得してきて下さい。
[Hyper-VをON・OFFにするバッチ](https://github.com/dodonki1223/CreateScript/tree/master/ToEnableAndDisableHyper-V)

#### 注意

- 必要なのは**「ToEnableAndDisableHyper-V」**フォルダだけです
- ダウンロードすると文字コードが`UTF-8`になっていて正しく動作しないので文字コードを`Shift-JIS`に変換して使用してください

**フォルダ構成**

```
ToEnableAndDisableHyper-V
 ├ 01_ToEnableHyper-V.bat
 ├ 02_ToEnableHyper-V-Command.bat
 ├ 03_ToDisableHyper-V-Command.bat
 ├ 04_CheckHyper-V-Status.bat
 └ README.md
```

**それぞれのバッチファイルについて**

- 01_ToEnableHyper-V.bat
    - 実行するバッチファイル
- 02_ToEnableHyper-V-Command.bat
    - Hyper-VをONにするバッチになります
- 03_ToDisableHyper-V-Command.bat
    - Hyper-VをOFFにするバッチになります
- 04_CheckHyper-V-Status.bat
    - Hyper-Vの現在の状態を確認するバッチになります（ONなのかOFFなのか）

**使い方**

実行する時は「01_ToEnableHyper-V.bat」を実行してくれるだけでいいです。

実行すると２つのコマンドプロンプトが立ち上がります。

１番最初に立ち上がる画面はHyper-VをON・OFFにする画面

![１番始め.png](https://qiita-image-store.s3.amazonaws.com/0/299904/628cac42-3245-6ff9-15be-ba53839db627.png)

２番目に立ち上がる画面はHyper-Vの現在の状態を教えてくれるものになります。現在の状態を確認できたらこの画面は閉じてしまって構いません。

![現在の状態を確認する画面.png](https://qiita-image-store.s3.amazonaws.com/0/299904/9a4eaed7-1a4c-b629-8416-93205446581e.png)

１番最初に立ち上がった画面に戻り、
Hyper-VをONにしたい時は「1」を入力しEnter
Hyper-VをOFFにしたい時は「2」を入力しEnter

「1」か「2」を入力しEnterを押すと新しく画面が立ち上がります
※「1」を入力した場合

![ONにする.png](https://qiita-image-store.s3.amazonaws.com/0/299904/b56ec920-1b9b-4a3d-a067-b98756da6100.png)

Enterを押したらこの画面は閉じてしまっていいです。

![ONにするEnter.png](https://qiita-image-store.s3.amazonaws.com/0/299904/3de4c582-a1b5-e8e8-affb-d2f9c6625992.png)

１番最初に立ち上がった画面に戻り、直ちに再起動する場合は「y」または「Y」を入力しEnterを押せば再起動が行われます。
「y」または「Y」以外を入力した場合はそのままコマンドプロンプトが終了します。

![batファイルの実行後.png](https://qiita-image-store.s3.amazonaws.com/0/299904/61187e15-eae9-1cc7-06af-f08df8c8558e.png)


