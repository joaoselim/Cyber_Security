# XOR Starter
###### Solved by @joaoselim
> This CTF is about Crypto
## About the challenge
Começando, nós temos uma breve explicação sobre como funciona uma operação XOR
Em seguida, nos e dado a string `label`, e nos pede para fazermos um XOR de cada caracter com o número inteiro `13`. Em seguida devemos converter de volta para string para achar a flag.
## Solution
Já sabemos oque devemos fazer, para realizar nosso passo a passo podemos ultilizar de um código 
`echo -n "label" | perl -pe 's/(.)/chr(ord($1) ^ 13)/ge'` é possível chegarmos a flag. Então entendendo oque ele faz:

`echo -n` imprime o texto na tela sem que ocorra nenhuma quebra de linha, e já envia `"label"` para o próximo comando(um script `perl`)

`-pe 's/(.)/chr(ord($1) ^ 13)/ge` lê, converte os caracteres para tabela ASCII, realiza o XOR com o `^ 13`, e em seguida converte o valor de voltar em um caracter `chr()` 

Rodando o código no prompt de comando no Kali, se obtem como saída `aloha`

[![Captura-de-tela-2025-06-05-153634.png](https://i.postimg.cc/3Nnmt1Dq/Captura-de-tela-2025-06-05-153634.png)](https://postimg.cc/MffnTy50)

colocando no formato `cryptohack{}` se tem a flag.

>'cryptohack{aloha}'
