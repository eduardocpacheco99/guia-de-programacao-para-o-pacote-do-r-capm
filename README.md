# Guia de programação para o pacote do R `capm`
#### Oswaldo Santos Baquero (coordenador)
#### Markos Amaku
#### Fernando Ferreira
<br>
Contato: <oswaldosant@gmail.com>
<br>
Última revisão: 15 de novembro de 2015  
Em caso de não estar lendo a versão online, está versão pode estar desatualizada.  
Clique [aqui](https://www.gitbook.com/book/oswaldosantos/guia-de-programacao-para-o-pacote-do-r-capm/details) para acessar a página do livro.
<br><br><br><br>

O propósito deste guia é mostrar como implementar o [fluxo de trabalho](http://oswaldosantos.github.io/capm) suportado pelo `capm`, sem entrar em detalhes teóricos ou em implicações práticas (isto será o foco de artigos acadêmicos e documentação complementar).  
 
#### Pressupostos para reproduzir este guia:

* Conhecimento mínimo de programação no R.
* Familiaridade com o formato das páginas de ajuda das funções.
* `capm` versão 0.8.0 instalada.
* A área de trabalho atual deve conter os seguintes arquivos: 
 * pilot.csv
 * psu.ssu.csv
 * santos.dbf
 * santos.prj
 * santos.shp
 * santos.shx
 * survey.data.csv

Esses arquivos podem ser baixados [aqui](https://github.com/oswaldosantos/programming-guide-for-the-capm-r-package) usando o botão "Download ZIP" no canto inferior direito.  

Não usaremos todas as funções do `capm` e das funções usadas só exploraremos alguns argumentos. Por favor consulte as páginas de ajuda das funções para ver os detalhes.