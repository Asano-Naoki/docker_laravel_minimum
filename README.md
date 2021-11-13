[日本語版 README はこちら](/README_ja.md)

# docker_laravel_minimum
docker-compose for minimum Laravel(LEMP) environment

## Summary
You can create minimum Laravel(LEMP) environment using docker-compose.

## Prerequisites
Before using this repository, prepare your Laravel project directory.

If you'd like to create a new one, it can be done through docker Composer like this:

```
docker run --rm --volume $PWD:/app composer create-project laravel/laravel example-app
```
NOTE:
The files and directories created with this command are owned by root user/group. In order to change the owner, use the -u option of the docker run command or run the chown command seperately. 

DB_HOST is referred by the name "mysql" instead of 127.0.0.1 in this docker-compose environment, so change the section in the .env file in you Laravel project directory. Besides, set DB_USERNAME and DB_PASSWORD properly(not root or blank).


```
-DB_HOST=127.0.0.1
+DB_HOST=mysql
-DB_USERNAME=root
-DB_PASSWORD=
+DB_USERNAME=test_user
+DB_PASSWORD=test_pass
```


## Usage
First, copy all the files and directory in this repository to your Laravel project directory with git command(fetch and merge) or manually.

An example of the directory structure is like this:
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
<b>Bold files and directory</b> are included in this repository.

Next, start docker-compose.

```
docker-compose up -d
```
NOTE:
If mysql or nginx has already been started in your local machine and "address already in use" error occurs, please stop them(your local nginx and mysql) or change ports in the docker-composer.yaml appropriately.

Now, you can view your page at http://localhost/.

NOTE:
In case of "The stream or file "/var/www/html/storage/logs/laravel.log" could not be opened in append mode: Failed to open stream: Permission denied" error, give php-fpm(www-data) the right permission to write log files. This is achieved by this command.
```
docker-compose exec nginx sh
...
(after entering sh of nginx)
...
chown :www-data -R /var/www/html/storage/
chmod 775 -R /var/www/html/storage/
```

Finally, make sure that you can execute php artisan migrate.
```
docker-compose exec php sh
...
(after entering sh of php)
...
php artisan migrate
```
Congratulations! The database files shoud be stored in the volume directory.

## Author
[Asano Naoki](https://asanonaoki.com/blog/)


## License
Under the MIT License. See [LICENSE](/LICENSE) for details.




