# CircleCI ✖︎ AWSでCI/CDを体験しよう！！

![image](https://user-images.githubusercontent.com/66664167/92107539-ecf40280-ee20-11ea-8651-7261697f7995.png)

## 事前準備
- AWS アカウント作成
- Dockerインストール
- docker-composeインストール
- Gitインストール
- GitHub アカウント作成
- Windowsの場合、SSH接続ができる環境の用意(Tera Termなど）
([Tera Termのインストールはこちら](https://ja.osdn.net/projects/ttssh2/))


## ★　今日やること　★
### 1.目的
#### CircleCI ✖︎ AWSでCI/CDを体験する


### 2.今日のゴール
- ローカルからGithubにPushすると、本番環境（EC2）に変更が反映されていることを確認する
- 間違いテストの場合、変更が反映されないことを確認する
- CircleCIの仕組みを体系的に理解する
![image](https://user-images.githubusercontent.com/66664167/93009834-7ee9d100-f5c0-11ea-9712-7d43f4084278.png)


### 3.環境・言語
- Docker
- AWS(EC2インスタンス)
- CircleCI
- Git
- Github
- Laravel(PHP)


## はじめに
### CI/CDとは

CI/CDとは高度な継続的自動化と継続的監視をアプリケーション開発に取り組むこと、また、そのプロセスのこと

![image](https://user-images.githubusercontent.com/66664167/92073666-39264f00-edef-11ea-8e05-c9db7127b3e9.png)

<br>
<br>

### 継続的インテグレーション

>継続的インテグレーションは、コードを共有リポジトリの master ブランチに常時頻繁に統合することを奨励する開発手法です。それぞれの機能を個別にビルドして、開発サイクルの最後に統合するのではなく、各開発者のコードが 1 日に何度も共有リポジトリに統合されます。
>すべての開発者が共有メインラインに毎日コミットします。コミットされるたびに、自動化されたビルドとテストがトリガーされます。ビルドやテストが失敗しても、数分以内にすばやく修復できます。
>チームの生産性、効率、満足度を向上させます。 問題をすばやく検出して解決できます。 より高品質で安定したプロダクトをリリースできます。
<br>
※　CircleCi公式ドキュメントより：https://circleci.com/docs/ja/2.0/about-circleci/#section=welcome
<br>

![image](https://user-images.githubusercontent.com/66664167/92101045-83232b00-ee17-11ea-89d9-38ee6caee2df.png)

<br>
※　技術評論社より：https://gihyo.jp/book/pickup/2015/0045
<br>

### 継続的デリバリー
>検証されたコードのリポジトリへのリリースを自動化します。継続的デリバリーの目標は、本番環境にデプロイできるコードベースを常に保持しておくことです。
継続的デリバリーでは、コード変更のマージから本番環境対応のビルドのデリバリーまで、すべてのステージでテストの自動化とコードリリースの自動化が実施されます。このプロセスの終了時には、運用チームはアプリケーションを本番環境にすばやく簡単にデプロイできます。
<br>

### 続的デプロイメント
>成熟した CI/CD パイプラインの最終ステージは、継続的デプロイメントです。本番環境対応のビルドをコードリポジトリへ自動的にリリースする継続的デリバリーの延長として、継続的デプロイメントはアプリケーションを本番環境に自動的にリリースします。本番環境に移す前のパイプラインのステージでは手動でのチェックはないので、継続的デプロイメントは主に、入念に設計されたテスト自動化を活用します。
<br>
<br>

## なぜCI/CDなのか？
#### ・自動化テストの重要性の高まり<br>
これだけソフトウェアが普及していくと、品質に対する要求も高くなり、今まで手動でテストしていた分野でも積極的に自動化テストを取り入れるようになってきます。なぜならCI/CDを使うと、コードの変更が発生するたびに自動でテストが行われるので、テストの実行忘れや、環境依存のテストを失くすことでの品質を向上させることができるからです。

#### ・アジャイル開発のプラクティスの浸透と進化<br>
アジャイル開発で重要なことはスピードです。より小さな粒度の変更をいかに早くテスト／リリースしてフィードバックを得るかがアジャイル開発の成功の鍵となりますが、CI/CDを使うことでこれらを実現することができます。
<br>
<br>

## CI/CDツール選定
### Jenkins
![image](https://user-images.githubusercontent.com/66664167/92107197-5a536380-ee20-11ea-95b5-6488c0ecb79f.png)
#### メリット
- 拡張性が高い。プロジェクトに合わせてカスタマイズしやすい
- オンプレ上に構築するため、IPが固定される
- ドキュメントが豊富で調査しやすい

#### デメリット
サーバを用意する必要があるため、費用がかかる、準備に時間がかかる
本番を含めワークフローに組み込むため、導入は計画的に行う必要がある
保守・運用が必要なため管理を怠ると属人化しやすい


### TravisCI
![image](https://user-images.githubusercontent.com/66664167/92107394-a9999400-ee20-11ea-80af-30f183fb4802.png)
#### メリット
- Cloudであるため、サーバの管理・運用は不要になる
- Githubとの親和性高い
- ドキュメントが豊富で調査しやすい

#### デメリット
- コンテナにsshで入れないのでデバッグしにくい
- $129がかかるため、CIをはじめて導入する際には提案しにくい
- Cloudなのでビルドの度にIPが変わる


### CircleCI
![image](https://user-images.githubusercontent.com/66664167/92107539-ecf40280-ee20-11ea-8651-7261697f7995.png)

#### メリット
- Cloudであるため、サーバの管理・運用は不要
- 公式のサポートが手厚い
- ドキュメントが豊富で調査しやすい
- sshでコンテナに入れるので、デバッグしやすい
- 無料で使い始められる

#### デメリット
- Clouldなのでビルドの度に毎回IPが変わる
- GitHubまたはBitbucketとの連携が前提



## やってみよう！

### 1. リポジトリを作成
#### 1-1 GitHubのリポジトリにアクセスする
[リポジトリにアクセス](https://github.com/miracleave-ltd/meet-up_CI-CD)

#### 1-2 FORKする
![image](https://user-images.githubusercontent.com/66664167/92993991-68e0ff80-f531-11ea-8906-c8597b8acb01.png)

##### [自分のユーザー名]/meet-up_CI-CDになっていることを確認する
![image](https://user-images.githubusercontent.com/66664167/92994008-9332bd00-f531-11ea-8354-3ba81dba2440.png)

#### 1-3 ローカルの任意の場所にリポジトリをクローンする
※ターミナル（コマンドプロンプト)を使用します。

##### 1-3-1 リポジトリのURLをコピーする
![image](https://user-images.githubusercontent.com/54019059/92994174-f3762e80-f532-11ea-9a87-ab4fad897171.png)

##### 1-3-2 クローンする

```
例：Desktopにクローンする場合
~ $ cd ~/Desktop
~/Desktop $ git clone [コピーしたURL] 
```

#### ※以降、リポジトリがデスクトップに存在している前提でコマンドを表記します。
適宜、自分のリポジトリの場所に置き換えてコマンド実行してください。
```
例：
~/Desktop/meet-up_CI-CD $　[コマンド]
```


### 2. 初期設定 (今回は割愛しますがローカルで動かす場合はこちらの設定が必要です）
ローカル内のDockerコンテナにLaravelのライブラリパッケージ等をインストールする

#### 2-0 docker-compose.yml のポート番号を編集する

##### 編集前
![image](https://user-images.githubusercontent.com/66664167/93546902-d7451800-f99e-11ea-8eb4-83ff5c6a7384.png)


##### 編集後
![image](https://user-images.githubusercontent.com/66664167/93543159-c7750600-f995-11ea-911b-c488dae3c699.png)




#### ★本番環境に反映する際は[80:80]でプッシュすること★



#### 2-1 コンテナを立ち上げる
```
// リポジトリに移動する
~/Desktop $ cd ~/Desktop/meet-up_CI-CD
// コンテナを立ち上げる
~/Desktop/meet-up_CI-CD $ docker-compose up -d
```
#### 2-2 dockerコンテナに入る
```
~/Desktop/meet-up_CI-CD $ docker-compose exec app bash
```

コンテナ内に入ると、[root@06e2xxxxxx:/var/www/html# ]のような表示に変わります。ローカルの環境によって差分が出ますが全く一緒じゃなくても問題ありません。
（コンテナを実行している箇所を表示しています）

#### 2-3 インストールする
```
cd my-laravel-app && composer install && cp ../docker/laravel/.env .env && chmod 777 -R storage/ && php artisan key:generate && php artisan config:cache && php artisan migrate 
```

#### 2-4 コンテナを抜ける
```
exit
```

#### 2-5 コンテナを再起動する 
```
~/Desktop/meet-up_CI-CD $ docker-compose down && docker-compose up
```

#### 2-6 起動を確認する
http://localhost:8989/


### 3.EC2の設定
#### 3-1 EC2の設定をする
[手順に沿ってEC2インスタンスを起動する](https://github.com/miracleave-ltd/aws_ec2)

#### ★注意★　上記手順の「Elastic IP」の設定は不要

#### ★注意★　上記手順の「インスタンスの詳細を設定する」にて変更あり
##### ※「インスタンスの詳細を設定する」という画面で【ユーザーデータ】という項目があるので、そこに下記のファイルの内容を設定する

```
#!/bin/bash
# Dockerをインストール
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo chkconfig docker on
sudo usermod -a -G docker ec2-user
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo yum install -y git
```
![image](https://user-images.githubusercontent.com/66664167/92995757-3808c680-f541-11ea-9201-99b009bf15ef.png)


#### 3-2 EC2に接続する
##### Macの方
```
// EC2にログイン
~/Desktop/meet-up_CI-CD $ ssh -i ~/.ssh/{pem key名} {ユーザー名}@{IP}
```

##### Windowsの方(tera term)
###### 1.パブリックIPアドレスコピーする（プライベートIPではないので注意）
![image](https://user-images.githubusercontent.com/66664167/93546598-17f06180-f99e-11ea-9483-aea979af72fb.png)

###### 2. tera termを開き、「新しい接続」画面の「ホスト(T)」、に、EC2のパブリックIPアドレスを入力する
![image](https://user-images.githubusercontent.com/66664167/93546679-4706d300-f99e-11ea-9ff2-1049b5f67559.png)

###### 3.続行を押下する
![image](https://user-images.githubusercontent.com/66664167/93545458-651f0400-f99b-11ea-844d-612305c00eaa.png)

###### 4.「SSH認証画面」の「ユーザー名」に「ec2-user」を入力、「認証方式」で「秘密鍵」をチェックして、ダウンロードしたpemファイルを指定する
![image](https://user-images.githubusercontent.com/66664167/93546769-82090680-f99e-11ea-9ce7-719cf0b31e60.png)

###### 5. 上記設定完了したら「OK」ボタンを押下して接続確認する。
![image](https://user-images.githubusercontent.com/66664167/93546226-5c2f3200-f99d-11ea-872a-1c447ed8d1cc.png)


#### 3-3 リポジトリをcloneする
```
[ec2-user@ip-{プライベートIP} ~]$ git clone https://github.com/{ユーザー名}/{リポジトリ}.git
```

#### 3-4 コンテナを立ち上げる
```
[ec2-user@ip-{プライベートIP} ~]$ cd {リポジトリ名}
[ec2-user@ip-{プライベートIP} ~ {リポジトリ名}]$ docker-compose up -d
```

#### 3-5 コンテナ中に入り、Laravelに関する設定する
```
[ec2-user@ip-{プライベートIP} ~ {リポジトリ名}]$ docker-compose exec app bash
cd my-laravel-app && composer install && cp ../docker/laravel/.env .env && chmod 777 -R storage/ && php artisan key:generate && php artisan config:cache && php artisan migrate
```

#### 3-6 コンテナを抜け、再起動する
```
exit
[ec2-user@ip-{プライベートIP} ~ {リポジトリ名}]$ docker-compose restart
```

#### 3-7 アプリが見えるか確認(ブラウザで確認)
```
http://{EC2パブリックIPアドレス}
```

#### 3-8　秘密鍵・公開鍵を設定
##### ※色々と英語で聞かれますが、何も入力せず Enter 連打で OK
```
// 秘密鍵・公開鍵の作成を行う
[ec2-user@ip-{プライベートIP} ~]$ssh-keygen -m pem
```

```
// 公開鍵をauthorized_keysに追記する
[ec2-user@ip-{プライベートIP} ~]$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

### 4 CircleCIの設定
#### 4-1 ログインする
https://circleci.com/ja/vcs-authorize/

![image](https://user-images.githubusercontent.com/66664167/92234453-095d7100-eeed-11ea-8730-beb61574538a.png)


#### 4-2 該当プロジェクトの設定をする
##### 4-2-1 該当プロジェクトの「Set Up Project」を押下する
![image](https://user-images.githubusercontent.com/66664167/92300453-c6a3a380-ef95-11ea-8aee-ab2a4078691c.png)

##### 4-2-2 「Use Existing Config」ボタンを押下する
![image](https://user-images.githubusercontent.com/66664167/92300489-2ac66780-ef96-11ea-922c-d56902bea456.png)

##### 4-2-3 「Start Building」ボタンを押下する
![image](https://user-images.githubusercontent.com/66664167/92300523-7aa52e80-ef96-11ea-8856-8511d7febf14.png)


#### 4-3 EC2インスタンスと関連付ける
テストが開始される（各種設定前なのでFAILDになるが問題ありません）
##### 4-3-1 「Project Setting」ボタンを押下する
![image](https://user-images.githubusercontent.com/66664167/92300761-7417b680-ef98-11ea-9a75-a0ae99371953.png)

##### 4-3-2 メニューの「SSH Keys」ボタンを押下する
![image](https://user-images.githubusercontent.com/66664167/92300833-16379e80-ef99-11ea-87de-99bc615f8c53.png)

##### 4-3-3 ターミナルを開き、EC2インスタンスにログインして、SSH Keyをコピーする

```
// EC2にログイン
$ ssh -i ~/.ssh/[pem key名] [ユーザー名]@[IP]

// SSH Keyが表示される
$ cat ~/.ssh/id_rsa
```

コピーする
![image](https://user-images.githubusercontent.com/66664167/92301975-2d7b8980-efa3-11ea-9ab9-461ff58fa09b.png)


##### 4-3-4 ページ下部の「Add SSH Keys」ボタンを押下すし、IPアドレスとコピーしたKeyを入力する
![image](https://user-images.githubusercontent.com/66664167/92301125-d58d5480-ef9b-11ea-9f27-ee3ce9b92fdc.png)
![image](https://user-images.githubusercontent.com/66664167/92302429-23f42080-efa7-11ea-8730-1d28548e33ef.png)

##### 入力した内容が表示されていることを確認する（Keyは変換されて表示されているため、実際の入力した内容と表示は異なります）
![image](https://user-images.githubusercontent.com/66664167/92482470-9de40e00-f222-11ea-8bcb-78bb6130ccd5.png)



#### 4-4 環境変数を設定する
##### 4-4-1 メニューの「Organization Settings」ボタンを押下する
![image](https://user-images.githubusercontent.com/66664167/92302693-0d4ec900-efa9-11ea-9f7f-e80315837035.png)

##### 4-4-2 メニューの「Contexts」を選択（初期画面）し、「Create Context」を押下する
![image](https://user-images.githubusercontent.com/66664167/92478693-7f2f4880-f21d-11ea-8415-d0338347fc9f.png)

##### 4-4-3 「Context Name」に任意の名前を入力し、「Create Context」を押下する（例：meet-up）
![image](https://user-images.githubusercontent.com/66664167/92479021-f95fcd00-f21d-11ea-9058-81509b3171a5.png)

##### 4-4-4 前画面に戻るので、入力した名前があるか確認し、該当のNameを押下する
![image](https://user-images.githubusercontent.com/66664167/92479460-87d44e80-f21e-11ea-947a-5a8ed7b8e1f9.png)

##### 4-4-5 「Add Environment Variables」を押下する
![image](https://user-images.githubusercontent.com/66664167/92479794-f6191100-f21e-11ea-8d4a-ade01ec2fd12.png)

##### 4-4-6 「Environment Variable Name」と「Value」にそれぞれ下記の通り入力し、 「Add Environment Variables」を押下する
```
① Environment Variable Name : USER_NAME
　 Value                     : EC2インスタンスのユーザー名（例：ec2-user）
  
② Environment Variable Name : HOST_NAME
　 Value                     : EC2インスタンスのIPアドレス
```

![image](https://user-images.githubusercontent.com/66664167/92480545-01b90780-f220-11ea-9379-feb90dc1c7c3.png)

###### 入力後の画面にてそれぞれName,Valueが入っていることを確認する
![image](https://user-images.githubusercontent.com/66664167/92481230-0205d280-f221-11ea-8325-9c25d2ba924f.png)



#### 設定完了！お疲れ様でした！
#### ここからいよいよCI/CD体験をしていきましょう！



### 5 ローカルで変更を加えてCI/CDの動作を確認する

#### 5-1 ローカルのクライアントソースで任意の場所を変更し、リポジトリにpushする。

##### 例：　ヘッダーのボタンをそれぞれ「登録」「一覧」から「登録ボタン」「一覧ボタン」に変更する。
##### Before
![image](https://user-images.githubusercontent.com/66664167/92484565-ccfb7f00-f224-11ea-9d07-dc6482b7fc82.png)

##### After
![image](https://user-images.githubusercontent.com/66664167/92484786-0df39380-f225-11ea-9a7f-6a22c5627b00.png)


![image](https://user-images.githubusercontent.com/55343055/93594405-bb6a6200-f9f0-11ea-930c-ea51097d7377.png)


##### 5-1-1 ソース変更後、変更をリポジトリにpushする。
```
~/Desktop/meet-up_CI-CD$ git add .
~/Desktop/meet-up_CI-CD$ git commit -m "fix"
~/Desktop/meet-up_CI-CD$ git push
```

##### 5-1-2 CircleCIを確認する

「Running」状態になっている
![image](https://user-images.githubusercontent.com/66664167/92486011-94f53b80-f226-11ea-901c-332e524ab23a.png)

時間が経つと「Success」になる
![image](https://user-images.githubusercontent.com/66664167/92486333-ed2c3d80-f226-11ea-88da-f35f6aabad27.png)


##### 5-1-3 CircleCIで「Success」後、ブラウザで確認する。
変更が反映されている！
![image](https://user-images.githubusercontent.com/66664167/92484786-0df39380-f225-11ea-9a7f-6a22c5627b00.png)


#### 5-2 テストが失敗した状態を体験する
##### 5-2-1 テストコードをテストが失敗するように編集する(例：あるURLにgetリクエストした際のレスポンススタッツで200が返ってくる想定の箇所で、500を設定する）

![image](https://user-images.githubusercontent.com/66664167/92488026-ff0ee000-f228-11ea-82b1-219101bdc322.png)


##### 5-2-2 ソース変更後、変更をリポジトリにpushする。
```
~/Desktop/meet-up_CI-CD$ git add .<br>
~/Desktop/meet-up_CI-CD$ git commit -m "fix"<br>
~/Desktop/meet-up_CI-CD$ git push
```

##### 5-2-3 CircleCIを確認する

「Faild」になる
![image](https://user-images.githubusercontent.com/66664167/92488378-6f1d6600-f229-11ea-94be-b955b8ced24e.png)

##### 5-2-4 「Faild」→「失敗箇所（今回は「build」）」を押下して、ログを確認する
![image](https://user-images.githubusercontent.com/66664167/92490179-a9880280-f22b-11ea-9328-d6d81766cfa6.png)
