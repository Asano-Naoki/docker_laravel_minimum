# docker_laravel_minimum
最小限のLaravel(LEMP)環境を作るためのdocker-compose

## 概要
docker-composeを使って最小限のLaravel(LEMP)環境を構築することができます。

## 前提条件
このリポジトリを使う前に、Laravelプロジェクトディレクトリを用意してください。

新しいLaravelプロジェクトディレクトリを作る際には、dockerのComposerを以下のように利用することができます。

```
docker run --rm --volume $PWD:/app composer create-project laravel/laravel example-app
```
注:
上記コマンドで作成されるファイルとディレクトリはrootの所有ユーザー／グループとなります。所有者を変更するために、docker runコマンドの-uオプションを使うか、ファイル及びディレクトリ作成後にchownコマンドを使ってください。

このdocker-compose環境では、DB_HOSTが127.0.0.1ではなくmysqlという名前で参照されます。よって、.envファイルの該当箇所を変更してください。そして、DB_USERNAMEとDB_PASSWORDを適切に設定してください。rootや空白は不可です。


```
-DB_HOST=127.0.0.1
+DB_HOST=mysql
-DB_USERNAME=root
-DB_PASSWORD=
+DB_USERNAME=test_user
+DB_PASSWORD=test_pass
```


## 使い方
まず、このリポジトリ内のファイルとディレクトリをすべて、gitコマンド（fetchとmerge）か手動で、Laravelプロジェクトディレクトリにコピーしてください。

ディレクトリ構成の一例：
<pre>
.
├── <b>LICENSE</b>
├── <b>README.md</b>
├── app
├── artisan
├── bootstrap
├── composer.json
├── composer.lock
├── config
├── database
├── <b>docker</b>
├── <b>docker-compose.yaml</b>
├── package.json
├── phpunit.xml
├── public
├── resources
├── routes
├── server.php
├── storage
├── tests
├── vendor
└── webpack.mix.js
</pre>
<b>太字</b>はこのリポジトリに含まれているものです。

次に、docker-composeを開始してください。
```
docker-compose up -d
```
注:
mysqlやnginxがローカルマシンですでに起動していて"address already in use"エラーが発生したら、それらを停止するかdocker-composer.yamlのポートを適当に変更するかしてください。

http://localhost/
でページを見ることができます。

注:
"The stream or file "/var/www/html/storage/logs/laravel.log" could not be opened in append mode: Failed to open stream: Permission denied"エラーが発生する場合は、php-fpm(www-data)にログを書き込むための適切なパーミションを与えてください。例えば次のコマンドになります。
```
docker-compose exec nginx sh
...
(after entering sh of nginx)
...
chown :www-data -R /var/www/html/storage/
chmod 775 -R /var/www/html/storage/
```

最後に、php artisan migrateを実行できることを確認してください。
```
docker-compose exec php sh
...
(after entering sh of php)
...
php artisan migrate
```
これで終わりです。データベースファイルがvolumeディレクトリに保存されているはずです。

## 著者
[浅野直樹](https://asanonaoki.com/blog/)


## License
MITライセンスの元にライセンスされています。詳細は[LICENSE](/LICENSE)をご覧ください。




