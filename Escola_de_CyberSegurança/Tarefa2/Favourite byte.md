# Favourite byte
###### Solved by @joaoselim
> This CTF is about Crypto
## About the challenge
Para essa questão o enunciado nos informa que foi escondido alguma informação em `73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d`, provavelmente a nossa flag, utilizando XOR com um único byte cujo nós não temos conhecimento de qual é.

## Solution
Primeira coisa a se fazer, precisamos do byte secreto para realizar o XOR, podendo ser qualquer byte desde 0 à 255. Para descobrir qual é iremos utilizar o seguinte código de [brute force](https://pt.wikipedia.org/wiki/Ataque_de_for%C3%A7a_bruta).

[![Captura-de-tela-2025-06-05-203202.png](https://i.postimg.cc/QMrX4TZg/Captura-de-tela-2025-06-05-203202.png)](https://postimg.cc/DSBkmmZm)

Nele nós testamos todas as possibilidades de XOR até achar aquela que tem o formato igual a `"crypto{"` que sinalisa que foi achada a flag. Assim rodando o código `"qualquercoisa.py"` temos como resposta

[![Captura-de-tela-2025-06-05-203738.png](https://i.postimg.cc/TYLfT9Rq/Captura-de-tela-2025-06-05-203738.png)](https://postimg.cc/KR2dNt3R)

Recebemos uma flag e o byte em que foi encontrada. Testando a flag encontrada, confirmamos que esta correta.
>'crypto{0x10_15_my_f4v0ur173_by7e}'
