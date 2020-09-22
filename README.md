# Data Layer 

#### conexão

```php
define("DATA_LAYER_CONFIG", [
    "driver" => "mysql",
    "host" => "localhost",
    "port" => "3306",
    "dbname" => "db",
    "username" => "root",
    "passwd" => "",
    "options" => [
        PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8",
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_OBJ,
        PDO::ATTR_CASE => PDO::CASE_NATURAL
    ]
]);
```

#### model

```php
class User extends DataLayer
{
    public function __construct()
    {
        //string "TABLE_NAME", array ["REQUIRED_FIELD_1", "REQUIRED_FIELD_2"], string "PRIMARY_KEY", bool "TIMESTAMPS"
        parent::__construct("users", ["first_name", "last_name"]);
    }
}
```

#### busca

```php
<?php
use Example\Models\User;
$model = new User();

//find all users
$users = $model->find()->fetch(true);

//find all users limit 2
$users = $model->find()->limit(2)->fetch(true);

//find all users limit 2 offset 2
$users = $model->find()->limit(2)->offset(2)->fetch(true);

//find all users limit 2 offset 2 order by field ASC
$users = $model->find()->limit(2)->offset(2)->order("first_name ASC")->fetch(true);

//looping users
foreach ($users as $user) {
    echo $user->first_name;
}

//find one user by condition
$user = $model->find("first_name = :name", "name=Paul")->fetch();
echo $user->first_name;

//find one user by two conditions
$user = $model->find("first_name = :name AND last_name = :last", "name=Paul&last=Joseph")->fetch();
echo $user->first_name . " " . $user->first_last;
```

#### findById

```php
<?php
use Example\Models\User;

$model = new User();
$user = $model->findById(2);
echo $user->first_name;
```

#### secure params
######See example find_example.php and model classes
Consulte exemplo find_example.php e classes modelo

```php
$params = http_build_query(["name" => "UpInside & Associated"]);
$company = (new Company())->find("name = :name", $params);
var_dump($company, $company->fetch());
```

#### join method
######See example find_example.php and model classes
Consulte exemplo find_example.php e classes modelo

```php
$addresses = new Address();
$address = $addresses->findById(22);
//get user data to this->user->[all data]
$address->user();
var_dump($address);
```

#### count

```php
<?php
use Example\Models\User;
$model = new User();

$count = $model->find()->count();
```

#### save create

```php
<?php
use Example\Models\User;
$user = new User();

$user->first_name = "Paul";
$user->last_name = "Joseph";
$userId = $user->save();
```

#### save update

```php
<?php
use Example\Models\User;
$user = (new User())->findById(2);

$user->first_name = "Robson";
$userId = $user->save();
```

#### destroy

```php
<?php
use Example\Models\User;
$user = (new User())->findById(2);

$user->destroy();
```

#### fail

```php
<?php
use Example\Models\User;
$user = (new User())->findById(2);

if($user->fail()){
    echo $user->fail()->getMessage();
}
```

#### custom data method

````php
class User{
    //...

    public function fullName(): string 
    {
        return "{$this->first_name} {$this->last_name}";
    }
    
    public function document(): string
    {
        return "Restrict";
    }
}

echo $this->full_name; //Nome Completo
echo $this->document; //Restrict
```` 
