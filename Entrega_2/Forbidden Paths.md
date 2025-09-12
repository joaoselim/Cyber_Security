# Forbidden Paths
###### Solved by @joaoselim
> This CTF is about Web

## About the Challenge
A questão [Forbidden Paths](https://play.picoctf.org/practice/challenge/270?page=1&search=forbi) apresenta o enunciado

`We know that the website files live in /usr/share/nginx/html/ and the flag is at /flag.txt but the website is filtering absolute file paths. Can you get past the filter to read the flag?`

Com isso, entende-se que ao abrir o site da questão, se é direcionado para dentro dos files `/usr/share/nginx/html/` e que a flag está contida em `/flag.txt`, que é onde se deve chegar.

## Solution
Abrindo o site, exibe-se um texto simples com opções de entrada `.txt` e uma área para a inserção de texto.
[![ex2.png](https://i.postimg.cc/wT1C6gXK/ex2.png)](https://postimg.cc/WhPW8RYS)

É possível verificar que requisições feitas, que não forem `divine-comedy.txt`, `oliver-twist.txt` ou `the-happy-prince.txt`, simplesmente não serão encontradas, a não ser que se consiga sair do diretório atual. 

Realisando [Path Traversal](https://portswigger.net/web-security/file-path-traversal), através de `../../../` se consegue sair do diretório atual e subir até a origem da estrutura dos diretórios.

Ao final do payload, se completa com `flag.txt` para realisar a busca pelo arquivo.

Payload utilizado:
```
../../../flag.txt
```
Inserindo se é direcionado para outra página

[![flagex2.png](https://i.postimg.cc/zGCbcmPW/flagex2.png)](https://postimg.cc/k25g2HCX)

Assim chegando na flag.
>'picoCTF{7h3_p47h_70_5ucc355_e5a6fcbc}'
