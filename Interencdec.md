# Interencdec
###### Solved by @joaoselim
>This is a CTF about criptografia
## About the challenge
Abrindo a questão nos é dado um arquivo e é perguntado se conseguimos tirar algum sentido do arquivo
## Solution
Primeira coisa a fazer foi baixar o arquivo, em seguida abri como '.txt'. Dentro dele havia
"YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6YzRNalV3YUcxcWZRPT0nCg==", identificando que a criptografia estava na base 64 e decriptografando chegamos no código
"b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzc4MjUwaG1qfQ=='", que decriptando novamente em base 64 chegamos em "wpjvJAM{jhlzhy_k3jy9wa3k_78250hmj}". Já tendo o formato de flag, mas ainda
criptografado, identificando a criptografia como cifra de ceasar (cifra de ceasar é um tipo de criptografia que se consiste em pegar uma letra e avançar no alfabeto uma quantidade especifica
de letras, essa quantidade é escolhida por quem faz a criptografia), assim então, pegando o começo do texto "wpjvJAM" e vendo quanto que precisamos avançar para chegar em "picoCTF", e fazendo
o mesmo para a parte interna das chaves. E decriptografando chegamos na flag.
>'picoCTF{caesar_d3cr9pt3d_78250afc}'
