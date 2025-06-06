# XOR Propriets
###### Solved by @joaoselim
> This CTF is about Crypto
## About the challenge
Desenvolvendo mais a relação com XOR em relação ao que se teve na Questão `XOR Starter`, nos é entregue 4 propriedades úteis para resolver a questão

[![Captura-de-tela-2025-06-05-184010.png](https://i.postimg.cc/vBpNQW8D/Captura-de-tela-2025-06-05-184010.png)](https://postimg.cc/mzywj1HG)

Em seguida também nos é dado uma chave e seu valor, o valor de 2 XOR's entre 3 chaves diferentes e a XOR das 3 chaves em conjunto com a flag

[![Captura-de-tela-2025-06-05-184537.png](https://i.postimg.cc/TYxtC2K8/Captura-de-tela-2025-06-05-184537.png)](https://postimg.cc/JtT3hL3K)

`KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313`

`KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e`

`KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1`

`FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf`

## Solution
Com as informações sabemos onde está a nossa flag, só temos que conseguir isolar o valor da flag das outras chaves.

Se baseando nas propriedades fornecidas é possível criar uma relação entre as variáveis. Definindo o valor da XOR `FLAG ^ KEY1 ^ KEY3 ^ KEY2` = C, é possível, usando a propriedade de identidade e auto inversão antes fornecidas, rearranjar a XOR para `C ^ KEY1 ^ KEY3 ^ KEY2 = Flag`


Como já temos a KEY1, KEY2 ^ KEY3 e o C, só precisamos realizar um XOR entre todos os valores e chegamos ao valor da chave em [hexadecimal](https://canaltech.com.br/produtos/o-que-e-sistema-hexadecimal/).

Para isso utilizamos um código desenvolvido realizar o XOR entre três variáveis

[![Captura-de-tela-2025-06-05-191040.png](https://i.postimg.cc/76Y2fc6W/Captura-de-tela-2025-06-05-191040.png)](https://postimg.cc/Z92RQw68)

Então fornecemos as chaves a serem usadas

[![Captura-de-tela-2025-06-05-191416.png](https://i.postimg.cc/pLfhkx5S/Captura-de-tela-2025-06-05-191416.png)](https://postimg.cc/HrLksq20)

E por ultimo rodamos o código

[![Captura-de-tela-2025-06-05-191604.png](https://i.postimg.cc/g2Hyj3sr/Captura-de-tela-2025-06-05-191604.png)](https://postimg.cc/K143skdy)

Assim chegando na flag em hexa `63727970746f7b7830725f69355f61737330633161743176337d`.

Agora só nos resta decriptar. Utilizando a ferramenta CyberChef chegamos na flag.

>'crypto{x0r_i5_ass0c1at1v3}'