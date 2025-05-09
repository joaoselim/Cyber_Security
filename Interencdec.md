# Interencdec
###### Solved by @joaoselim
>This is a CTF about criptografia
## About the challenge
Abrindo a questão nos é dado um arquivo e é perguntado se conseguimos tirar algum sentido do arquivo
## Solution
Primeira coisa a fazer foi baixar o arquivo, em seguida abri como '.txt'. Dentro dele havia
"YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg==", identificando que a criptografia estava na 
[base 64](https://pt.wikipedia.org/wiki/Base64) por encerrar com "==" e decriptografando chegamos no código
"b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzc4MjUwaG1qfQ=='", e novamente decriptando em base 64 chegamos em "wpjvJAM{jhlzhy_k3jy9wa3k_78250hmj}". Se pode observar que já possui um formato de flag, mas ainda
criptografado, com as letras erradas, portanto associei a criptografia como [cifra de ceasar](https://pt.wikipedia.org/wiki/Cifra_de_C%C3%A9sar), assim então, pegando o começo do texto "wpjvJAM" e vendo quanto que precisariamos avançar para chegar em "picoCTF", e fazendo
o mesmo para a parte interna das chaves, e decriptografando chegamos na flag.
>'picoCTF{caesar_d3cr9pt3d_78250afc}'
