# Flag Hunters
###### Solved by @joaoselim
> This CTF is about Engenharia Reversa
## About the Challenge
Nosso enunciado faz referencia a um "refrão oculto" cujo o programa não imprime por padrão, e que há algo nele para nós, a flag.
 E então nos é fornceido o [codigo do progama](https://challenge-files.picoctf.net/c_verbal_sleep/1c1896b0e29fad87c1415f743b063161cad42d8f636ece348a6361e5be89309d/lyric-reader.py)
e a linha de comando para nos conectarmos ao netcat:

`$ nc verbal-sleep.picoctf.net 60492`
## Solution
Primeira coisa a se fazer é baixar o programa fornecido.

Em seguida rodamos o comando `nc verbal-sleep.picoctf.net 60492` que nos foi fornecido no CMD, e nos aparece

[![Captura-de-tela-2025-05-27-181538.png](https://i.postimg.cc/3RW1qsH3/Captura-de-tela-2025-05-27-181538.png)](https://postimg.cc/62JC213P)

Nesse momento vamos nos voltar para o código do programa para entender oque fazer a partir daqui. Abrindo o código temos trechos de uma música
e um `secret_intro` que é o refrão oculto que nos havia sido mencionado antes e é onde nossa flag está escondida

![image](https://github.com/user-attachments/assets/aeea54fe-bf1d-4962-8ec5-653bdaf46105)

Lendo o código entende-se que se rodar o `print` do `secret_intro` se roda o acesso de um arquivo onde está a nossa flag

[![Captura-2-de-tela-2025-05-27-183049.png](https://i.postimg.cc/NM7XpbKQ/Captura-2-de-tela-2025-05-27-183049.png)](https://postimg.cc/ygxWNmhG)

Nossa música começa a rodar a partir do VERSE1 (primeiro verso), então temos que fazer o código voltar para o começo onde está a nossa flag.
Para isso utilizamos uma [injeção de código](https://en.wikipedia.org/wiki/Code_injection) através do CMD.

