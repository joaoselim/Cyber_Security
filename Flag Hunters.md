# Flag Hunters
###### Solved by @joaoselim
> This CTF is about reverse engineering, code analysis
## About the Challenge
Nosso enunciado faz referencia a um "refrão oculto" cujo o programa não imprime por padrão, e que há algo nele para nós, a flag.
 E então nos é fornceido o [codigo do progama](https://challenge-files.picoctf.net/c_verbal_sleep/1c1896b0e29fad87c1415f743b063161cad42d8f636ece348a6361e5be89309d/lyric-reader.py)
e a linha de comando para nos conectarmos ao netcat:

`$ nc verbal-sleep.picoctf.net 60492`
## Solution
Primeira coisa a se fazer é baixar o programa fornecido.

Em seguida rodamos o comando `nc verbal-sleep.picoctf.net 60492` que nos foi fornecido no [CMD (prompt de comando)](https://canaltech.com.br/utilitarios/o-que-e-um-prompt-de-comando/), e nos aparece

[![Captura-de-tela-2025-05-27-181538.png](https://i.postimg.cc/3RW1qsH3/Captura-de-tela-2025-05-27-181538.png)](https://postimg.cc/62JC213P)

Nesse momento vamos nos voltar para o código do programa para entender oque fazer a partir daqui. Abrindo o código temos em meio às linhas de código, trechos de uma música e um `secret_intro` que é o refrão oculto que nos havia sido mencionado antes e é onde nossa flag está escondida

![image](https://github.com/user-attachments/assets/aeea54fe-bf1d-4962-8ec5-653bdaf46105)

Lendo o código entende-se que se rodar o `print` do `secret_intro` se roda o acesso de um arquivo onde está a nossa flag

[![Captura-2-de-tela-2025-05-27-183049.png](https://i.postimg.cc/NM7XpbKQ/Captura-2-de-tela-2025-05-27-183049.png)](https://postimg.cc/ygxWNmhG)

Nossa música começa a rodar a partir do VERSE1 (primeiro verso), então temos que fazer o código voltar para o começo onde está a nossa flag.
Para isso utilizamos uma [injeção de código](https://en.wikipedia.org/wiki/Code_injection) através do CMD.

Entendendo o código, é possível entender três partes importantes. Primeiramente, no comando CROWD, o programa lê a linha `CROWD (Singalong here!)` e chama um `input()`, assim substituindo o conteúdo daquela linha por `Crowd: <oque eu escrever>`, então temos uma entrada de informação no código do programa

[![Captura-de-tela-2025-05-28-111229.png](https://i.postimg.cc/mrBYbxWq/Captura-de-tela-2025-05-28-111229.png)](https://postimg.cc/zy2bjcBS)

Segundo, que o código utiliza de `;` para dividir a linha atual em partes separadas, então oque quer que eu coloque após isso será lido como parte do código

[![Captura-de-tela-2025-05-28-112200.png](https://i.postimg.cc/Z576tJS5/Captura-de-tela-2025-05-28-112200.png)](https://postimg.cc/LJPJjK5c)

E terceiro, o código decide qual parte da música tocar utilizando `RETURN[0-9]+`, então incerindo o return certo é possível voltar ao refrão oculto

[![Captura-de-tela-2025-05-28-112858.png](https://i.postimg.cc/ZnyPb2Jy/Captura-de-tela-2025-05-28-112858.png)](https://postimg.cc/rRMRgh08)

Agora já temos um lugar por onde podemos fazer a nossa injeção de código, e o formato do que devemos escrever para o código nos fornecer a flag.

Sendo assim então, incerindo `<uma_string_qualquer>;RETURN 0` o código nos retorna

[![Captura-de-tela-2025-05-28-115039.png](https://i.postimg.cc/zvPshkXT/Captura-de-tela-2025-05-28-115039.png)](https://postimg.cc/JHZYVZXh)

>'picoCTF{70637h3r_f0r3v3r_509142d4}'
