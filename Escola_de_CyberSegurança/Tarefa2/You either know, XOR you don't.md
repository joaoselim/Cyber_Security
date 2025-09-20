# You either know, XOR you don't
###### Solved by @joaoselim
> This CTF is about Crypto
## About the challenge
Somos informados de que a flag foi encriptada através de uma chave secreta, e nós somos desafiados a descobri-la e recebemos a mensagem encripatada. `0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104`

Para concluir recebemos a dica de nos atentarmos ao formato da flag e em como isso pode nos ajudar.
## Solution
Para conseguirmos descobrir a chave, usaremos do que já é possível de se saber.

A flag sempre começa com o mesmo formato `crypto{`, sabendo da propriedade de inversão que o XOR possui, vamos utilizar da ferramenta Cyberchef para primeiro converter a mensagem encriptada para [UTF8](https://developer.mozilla.org/pt-BR/docs/Glossary/UTF-8) realizar um XOR `mensagem em UTF8 ^ crypto{ = chave secreta`, ainda não chegamos na chave completa, para tentar completar testamos completar a flag com algum digito, assim usando `crypto{1` chegamos na chave `myXORkey`.

[![Captura-de-tela-2025-06-05-220952.png](https://i.postimg.cc/GmRM2LF3/Captura-de-tela-2025-06-05-220952.png)](https://postimg.cc/PP6bVkV0)

Agora em posse da chave secreta é só usar ela para decifrar a mensagem fazendo outro XOR `mensagem em UTF8 ^ myXORkey = FLAG`, assim chegando na flag.

[![Captura-de-tela-2025-06-05-222323.png](https://i.postimg.cc/T1qDH6pQ/Captura-de-tela-2025-06-05-222323.png)](https://postimg.cc/9w00rsPq)

Com isso foi possível ver de uma forma melhor a flexibilidade na ordem das variáveis em um XOR e sua versatilidade.

>'crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}'
