# SSTI1
#### Solved by @joaoselim
> This CTF is about web, SSTI

## About the challenge
A questão [SSTI1](https://play.picoctf.org/practice/challenge/492) possui o enunciado `I made a cool website where you can announce whatever you want! Try it out!` junto de um link gerado para o site

Simplesmente informa que foi criado um site em que nele se pode anunciar qualquer coisa.

## Solution
Ao abrir o site enocontra-se uma página simples com espaço para inserir texto



Sabendo pelo nome da questão, para chegar na flag tem de trabalhar com [SSTI](https://portswigger.net/web-security/server-side-template-injection#what-is-server-side-template-injection), que se consiste em injetar códigos em templates.
A primeira etapa é identificar qual o tipo de template com o qual se está lidando, para isso se tem alguns testes. Jinja2, {{7*'7'}}
