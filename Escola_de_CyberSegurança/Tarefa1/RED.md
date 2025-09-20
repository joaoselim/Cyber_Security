# RED
###### Solved by @joaoselim
> this CTF is about Forense
## About the Challenge
No enunciado só se tem `"RED, RED, RED, RED. Download the image: red.png"`
## Solution
Primeira coisa a se fazer é baixar o [arquivo fornecido](https://challenge-files.picoctf.net/c_verbal_sleep/831307718b34193b288dde31e557484876fb84978b5818e2627e453a54aa9ba6/red.png).

Abrindo o arquivo baixado, nos deparamos com uma imagem toda em vermelho: [![Captura-de-tela-2025-05-27-150435.png](https://i.postimg.cc/63bmg1Nr/Captura-de-tela-2025-05-27-150435.png)](https://postimg.cc/G8YKsKtt)

Vendo que não há nada explícito na imagem, seguimos a verificar se não há algo escondido no arquivo.

Abrindo o arquivo com o bloco de notas, se tem um poema escondido, e pegando a primeira letra de cada linha se tem uma dica. [![Captura-de-tela-2025-05-27-150911.png](https://i.postimg.cc/qB8m8RNc/Captura-de-tela-2025-05-27-150911.png)](https://postimg.cc/c6LMWdM6)
`Check LSB`, LSB (Least Significant Byte, bit menos significativo) é um tipo de [esteganografia](https://www.kaspersky.com.br/resource-center/definitions/what-is-steganography), em que se altera de maneira mínima a cor de pixels de uma imagem, de maneira que não é perceptivel ao olho, mas o computador é capaz de identificar.

Utilizando a ferramenta [CyberChef](https://gchq.github.io/CyberChef/) para decifrar se tem:
[![Captura-de-tela-2025-05-27-153306.png](https://i.postimg.cc/XNtbvxrH/Captura-de-tela-2025-05-27-153306.png)](https://postimg.cc/ThjS7Vqg)
`cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==`

Ainda criptografado, utilizando [DCode](https://www.dcode.fr/cipher-identifier), para identificar em que base esta a criptografia, se descobre que esta em `base64`

podemos utilizar tanto o próprio Dcode quanto o CyberChef para decifrar base 64, assim o fazendo chegamos a flag
>'picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}'
