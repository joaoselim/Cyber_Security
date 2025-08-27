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

<article><svg/ onload=prompt(1)
</article>
