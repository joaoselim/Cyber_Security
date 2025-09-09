# Secrets
###### Solved by @joaoselim
> This CTF is about Web

## About the Challenge
A questão [Secrets](https://play.picoctf.org/practice/challenge/296) contém o enunciado
`Temos diversas paginas escondidas. Você consegue encontrar a que contém a flag?`

## Solution
A questão já fornece uma informação, procurar por páginas escondidas.

Dando iniciando ao desafio, surge o link do site.
Abrindo se tem acesso a 3 páginas
[![tela-inicial.png](https://i.postimg.cc/xjJ44sMs/tela-inicial.png)](https://postimg.cc/dDF67jWd)
[![segunda-tela.png](https://i.postimg.cc/MZCF2yYz/segunda-tela.png)](https://postimg.cc/grKD3L57)
[![terceira-tela.png](https://i.postimg.cc/BngYKRfv/terceira-tela.png)](https://postimg.cc/VJJWyG6y)

Verificando a página fonte do site `[Ctrl+U]`

```

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <meta name="description" content="" />
    <!-- Bootstrap core CSS -->
    <link href="vendor/bootstrap/css/bootstrap.min.css" rel="stylesheet" />
    <!-- title -->
    <title>home</title>
    <!-- css -->
    <link href="secret/assets/index.css" rel="stylesheet" />
  </head>
  <body>
    <!-- ***** Header Area Start ***** -->
    <div class="topnav">
      <a class="active" href="#home">Home</a>
      <a href="about.html">About</a>
      <a href="contact.html">Contact</a>
    </div>

    <div class="imgcontainer">
      <img
        src="secret/assets/DX1KYM.jpg"
        alt="https://www.alamy.com/security-safety-word-cloud-concept-image-image67649784.html"
        class="responsive"
      />
      <div class="top-left">
        <h1>If security wasn't your job, would you do it as a hobby?</h1>
      </div>
    </div>
  </body>
</html>
```

Observa-se a existência de um diretório com o nome de `secret`.
```
<link href="secret/assets/index.css" rel="stylesheet" />
```

Retornando à página inicial e inserindo esse diretório, se chega na segunda página
[![secret.png](https://i.postimg.cc/Pf15kMk4/secret.png)](https://postimg.cc/nMcJ4Brj)

Novamente verificando o código fonte da página
```
<!DOCTYPE html>
<html>
  <head>
    <title></title>
    <link rel="stylesheet" href="hidden/file.css" />
  </head>

  <body>
    <h1>Finally. You almost found me. you are doing well</h1>
    <img src="https://media1.tenor.com/images/0a6aff9f825af62c05adfbd75039cc7b/tenor.gif?itemid=4648337" alt="Something Like That GIF - Andy Parksandrecreation Wtf GIFs" style="max-width: 833px; background-color: rgb(151, 121, 85);" width="833" height="937.125">
  </body>
</html>
```

Se verifica a existencia de outro diretório com nome suspeito de `hidden`, (traduzindo: escondido). Retornando à página `secret/` e adicionando também o `hidden/` vamos para
[![secret-hidden.png](https://i.postimg.cc/k4Xzhq9z/secret-hidden.png)](https://postimg.cc/jWmMDVD4)

Novamente ao acessar o código fonte
```

<!DOCTYPE html>
<html>
  <head>
    <title>LOGIN</title>
    <!-- css -->
    <link href="superhidden/login.css" rel="stylesheet" />
  </head>
  <body>
    <form>
      <div class="container">
        <form method="" action="/secret/assets/popup.js">
          <div class="row">
            <h2 style="text-align: center">
              Login with Social Media or Manually
            </h2>
            <div class="vl">
              <span class="vl-innertext">or</span>
            </div>

            <div class="col">
              <a href="#" class="fb btn">
                <i class="fa fa-facebook fa-fw"></i> Login with Facebook
              </a>
              <a href="#" class="twitter btn">
                <i class="fa fa-twitter fa-fw"></i> Login with Twitter
              </a>
              <a href="#" class="google btn">
                <i class="fa fa-google fa-fw"></i> Login with Google+
              </a>
            </div>

            <div class="col">
              <div class="hide-md-lg">
                <p>Or sign in manually:</p>
              </div>

              <input
                type="text"
                name="username"
                placeholder="Username"
                required
              />
              <input
                type="password"
                name="password"
                placeholder="Password"
                required
              />
              <input type="hidden" name="db" value="superhidden/xdfgwd.html" />

              <input
                type="submit"
                value="Login"
                onclick="alert('Thank you for the attempt but oops! try harder. better luck next time')"
              />
            </div>
          </div>
        </form>
      </div>

      <div class="bottom-container">
        <div class="row">
          <div class="col">
            <a href="#" style="color: white" class="btn">Sign up</a>
          </div>
          <div class="col">
            <a href="#" style="color: white" class="btn">Forgot password?</a>
          </div>
        </div>
      </div>
    </form>
  </body>
</html>
```
Se verifica a existencia de outro diretório suspeito,`superhidden`.
```
<link href="superhidden/login.css" rel="stylesheet" />
```

Retornando à página `secret/hidden/`, adicionando o `superhidden/`, se chega na última página
[![secret-hidden-superhidden.png](https://i.postimg.cc/QM0hM6ML/secret-hidden-superhidden.png)](https://postimg.cc/JyD965JK)

O texto informa que a flag está aqui, só não conseguimos ver. Porém se selecionar todo o conteúdo da página, aparece escondido a flag:
[![secret-hidden-superhidden-selecionado.png](https://i.postimg.cc/MZyxMKHb/secret-hidden-superhidden-selecionado.png)](https://postimg.cc/XX7ttWwp)

Também é possível encontrar-la acessando novamente o código fonte.
```
<!DOCTYPE html>
<html>
  <head>
    <title></title>
    <link rel="stylesheet" href="mycss.css" />
  </head>

  <body>
    <h1>Finally. You found me. But can you see me</h1>
    <h3 class="flag">picoCTF{succ3ss_@h3n1c@10n_790d2615}</h3>
  </body>
</html>
```

Assim chegando na flag.
>'picoCTF{succ3ss_@h3n1c@10n_790d2615}'
