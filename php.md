## 環境設定
phpの関数の補完
neocomplete-php
http://yuheikagaya.hatenablog.jp/entry/2014/01/19/235957

## php-memo

html内に直接書けたりする
開始タグ`<?php`
終了タグ`?>`


```php
<?php

echo "hogehoge"

?>
<!DOCTYPE html>
<html lang="ja">
<body>

<p>Hello world <?php echo "from php"; ?></p>
</body>
</html>
```

みたいに書く

`php -S 172.25.50.109:8000`　で built-in web server 建てることができる

phpの終了タグ、その後に出力するものがなければ、省略することが推奨されている

## 型

型は自動的に判断してくれる

```php
$msg = "hogehoge";
var_dump($msg); // データの型と値を教えてくれる
```

## 定数

定数はdefineで定義

```php
define("MY_EMAIL", "hoge@gmail.com");
```

`echo MY_EMAIL;`とか (変数みたいに$をつけなくてもよい）

代表的に定義されている定数もある（定義済みの定数）

```php
var_dump(__LINE__); // 現在の行数を出力
var_dump(__FILE__); // ファイル名
var_dump(__DIR__); // 現在いるディレクトリ
```

## 演算子

```
+ - / * % **(PHP5.6-)
```
インクリメントやデクリメントもできる

## 文字列

"特殊文字（\n, \t）使えたり、変数展開できたり
'上述のことができない

```php
$name = "hoge";
$s1 = "hello $name! \n"; //変数展開はそのまま$つけてかけばおｋ
```

わかりづらければ,
```php
$s1 = "hello {$name}! \n"; //変数展開はそのまま$つけてかけばおｋ
$s1 = "hello ${name}! \n"; //変数展開はそのまま$つけてかけばおｋ
```

とか、{}つけて書くこともできる

```php
$s = "hoge"."fuga"; //文字の連結は.を使う
```

## if文

if, elseif, else

比較演算子は >, <, >=, <=, ==, ===
== , != は値の比較
=== , !== は値とデータの型の比較も行う（より厳密に比較したい場合）

論理演算子は and, &&, or, ||, !(否定)

## 真偽値

### falseになる場合
文字列: 空、"0"

数値: 0, 0.0

論理値: false

配列: 要素の数が０

null

三項演算子も使える
```php
$max = ($a > $b) ? %a : $b;
```

## switch文
```php
$signal = "green";

switch ($signal) {
  case "red":
    echo "stop!";
    break;
  case "blue":
  case "green":
    echo "go!"
    break;
  case "yellow":
    echo "caution!";
    break;
  default:
    echo "wrong signal!";
    break;
}
```

## while文

while と do...while も使える

```php
while() {

}
```

```php
do {

}while();
```

## for文

for と break と continueも使える

## 配列

key と valueで管理するらしい

```php
$sales = array(
  "hoge" => 100,
  "fuga" => 200
);
```

ver 5.4以降だと

```php
$sales = [
  "hoge" => 100,
  "fuga" => 200
];
```

と書くことができる

```php
var_dump($sales["hoge"]);
```

のように値にアクセスする

```php
$sales["hoge"] = 400;
```

のように値を変更したりする

必ず、keyを設定する必要はなくて

```php
$colors = ["red", "blue", "pink"];
```

のようにすると、0からの数字が連番で値が与えられて

```php
$colors[1]; // blue
```

のように、よく見る配列として使える

## foreach

値を連続？で取り出せる

```php
$sales = [
  "hoge" => 100,
  "fuga" => 200
];

foreach ($sales as $key => $value) {
  echo "($key) $value";
}
```

keyが省略されている場合は、もう少し簡単に書ける

```php
$colors = ["red", "blue", "pink"];

foreach ($colors as $value) {
  echo "$value";
}
```


## foreach, if, while, for コロン構文について

```php
foreach ($colors as $value) :
  echo "$value";
endforeach;
```

とかみたいに書けたりもする

(while の場合は endforwhile らしい)
