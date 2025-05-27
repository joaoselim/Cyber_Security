# Interencdec
###### Solved by @joaoselim
>This is a CTF about criptografia
## About the challenge
Abrindo a questão nos é dado um [arquivo](https://artifacts.picoctf.net/c_titan/2/enc_flag) e é perguntado se conseguimos tirar algum sentido dele
## Solution
Primeira coisa a se fazer é baixar o arquivo, em seguida abri como `.txt`, Dentro dele havia [![Captura-de-tela-2025-05-27-155734.png](https://i.postimg.cc/VLyXFQSj/Captura-de-tela-2025-05-27-155734.png)](https://postimg.cc/6T079Pf3)
`YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg==`

Utilizando a ferramenta [DCode](https://www.dcode.fr/cipher-identifier) para identificar qual a base em que esta a criptografia, identificamos que esta em [base 64](https://pt.wikipedia.org/wiki/Base64). Decriptografando chegamos no código [![Captura-de-tela-2025-05-27-161412.png](https://i.postimg.cc/cJfprN8X/Captura-de-tela-2025-05-27-161412.png)](https://postimg.cc/Lq6CbwD1)

`b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzc4MjUwaG1qfQ=='`.


Repetindo o processo de identificação e decriptação chegamos em `wpjvJAM{jhlzhy_k3jy9wa3k_78250hmj}`. Se pode observar que já possui um formato de flag, mas ainda esta criptografado, com as letras erradas.

Repetindo o processo de identificação de criptografia pelo DCode, identificamos como [cifra de ceasar](https://pt.wikipedia.org/wiki/Cifra_de_C%C3%A9sar), assim então, decriptografando, chegamos na flag.
>'picoCTF{caesar_d3cr9pt3d_78250afc}'
