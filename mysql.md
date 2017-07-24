install して character とか変える

```
mysql -u ユーザ名 -p
```

```
status; // ステータス表示
show databases; // データベース表示
use データベース名; // データベースを使う
show tables; // テーブルを表示
```

```
CREATE TABLE hoge (
fuga INT(3),
piyo VARCHAR(5)
);
```
とかで、テーブル作成

```
desc テーブル名; // テーブルの構造表示
select フィールド名 from テーブル名; // フィールド名の表示
drop database データベース名; // データベースの削除
```

## password change
1. mysqldをstop
```sh
$ /etc/init.d/mysqld restart
```
2. --skip-grant-tables --user=rootというオプションつきで起動させる

3. mysqlにて
update user set authentication_string=PASSWORD("パスワード") where User='root';
flush privileges;
