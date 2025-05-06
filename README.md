# Cookie Monster Secret Recipe
###### Solved by @joaoselim
> This is a CTF about Web
## About the Challenge
O desafio se baseia em um monstro dos cookies que teria escondido sua receita em seu site. Nosso objetivo é então localizar essa receita no site dele, acessado através de um link fornecido.
## Solution
Primeiro, clicando no link fornecido, fomos direcionados para um site simples, com fundo branco, titulo em preto no centro na parte superior da tela, e uma área para inserir login e senha.
Pensando no próprio nome da questão "Cookie Monster", resolvi ir atrás dos cookies do site. Cliquei para inspecionar, em seguida selecionei 'Application' e 'Cookies'.
A princípio, não havia nada lá, então resolvi testar colocar algum login e senha quaisquer para ver oque acontecia. Assim o fazendo, além de ter sido mandado a uma pagina de 'acesso negado',
também apareceu um Cookie onde nós haviamos verificado anteriormente. Clicando para ver o valor do cookie, estava codificado. Copiei o código sem o URL, lancei no Dcode para identificar que 
era base 64 e então coloquei para transcrever, assim obtendo a flag.
>'picoCTF{c00k1e_m0nster_l0ves_c00kies_98D0603F}'
