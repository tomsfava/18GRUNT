# Módulo 18 - Automatize tarefas com GRUNT
## Notas
### Aula 1 - Configure o Grunt
#### **Sobre a aula**
* compreender o propósito e os benefícios da ferramenta Grunt para automação de tarefas no desenvolvimento de projetos web;
* instalar o Grunt globalmente e localmente em um projeto;
* criar arquivo de configuração (Gruntfile.js).
#### **Anotações**
as funções do grunt apresentadas pelo professor são as mesmas do gulp, compilar o sass, o less, compressão de imagens e arquivos;  
com o comando npm i -g grunt-cli nos instalamos o command line interface do grunt, que é uma forma de executar o grunt através da linha de comando;  
seguimos com npm init que cria package.json e em seguida instalamos o grun localmente com npm i --save-dev grunt que cria o package-lock.json;  
ao abrirmos chaves dentro dos parentes da função initconfig do grunt o professor disse que isso é um objeto de configuração;  
nessa aula foi só isso, exportamos uma função que recebe grunt como argumento, dentro dela chamamos grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
})
### Aula 2 - Crie tarefas
#### **Sobre a aula**
* criar tarefas personalizadas usando o Grunt;
* entender a ordem de execução das tarefas com o Grunt;
* simular tarefas demoradas e assíncronas.
#### **Anotações**
no grunt registramos tarefas através da função grunt.registerTask ela recebe como argumentos o nome da tarefa entre aspas e a função na qual a tarefa consiste;  
ao rodar npm run grunt temos a mesma mensagem de erro de que a tarefa default não foi encontrada, então para rodar a tarefa olaGrunt que criamos precisamos rodar npm run grunt olaGrunt, esse procedimento obteve sucesso;  
para simular uma tarefa mais demorada nós criamos dentro da função tarefa a função setTimeout que recebe como argumentos a função a ser executada com timeout e o tempo em milissegundos para a execução da tarefa, porem o grunt não aguardou a execução da tarefa, ele simplesmente declarou running 'olaGrunt' e sinalizou Done;  
para corrigir isso precisamos criar uma **const** done = this.async(); e depois do console.log da função chamar done(); isso faz com que o grunt perceba essa tarefa como tarefa assíncrona;  
agora para definir a tarefa default simplesmente registramos uma tarefa com o nome default, poderíamos então definir uma função para essa tarefa, mas podemos também criar um arranjo de tarefas, já que até então temos registrada somente a tarefa olaGrunt, colocamos ela entre aspas dentro dos colchetes do arranjo e agora ao chamar npm run grunt, a tarefa olaGrunt é rodada.
### Aula 3 - Use Grunt para compilar LESS
#### **Sobre a aula**
* instalar e configurar o plugin do LESS no Grunt;
* criar tarefas de compilação para ambientes de desenvolvimento e produção;
* utilizar o Grunt para compilar e automatizar tarefas.
#### **Anotações**
vamos instalar o plugin de less no Grunt através do comando npm i --save-dev grunt-contrib-less, em seguida vamos carregar o plugin através do grunt.loadNpmTaks('grunt-contrib-less') e em seguida configurá-lo dentro do grunt.initConfig(), vamos configurar uma ação para o ambiente de desenvolvimento, para isso, chamamos less: abrimos um bloco, nesse bloco abrimos um novo bloco development e nele um último bloco files com o nome do arquivo final 'main.css' seguido de dois pontos e o nome do arquivo inicial 'main.less';  
agora adicionamos a tarefa less dentro do array da tarefa default;  
foi preciso adicionar essa palavra development por que podemos estar criando configurações diferentes a partir do ambiente onde o grunt é executado, o ambiente padrão é o de desenvolvimento, development, podemos criar outra configuração para o ambiente de produção, que é o ambiente final, onde nosso site estará rodando, assim colocamos uma virgula após o bloco development e criamos um novo bloco production, dentro dele outro bloco options com o atributo compress: true, em seguida outro bloco files paralelo ao último com o nome do arquivo final main.min.css e o arquivo inicial main.less, assim o grunt vai criar um arquivo minificado para o ambiente de produção;  
para compilar sass precisamos de outro plugin: 'grunt-contrib-sass', ele também precisa ser carregado com grunt.loadNpmTasks e ser configurado dentro do grunt.initConfig, para isso vamos criar uma nova configuração para o sass, paralela a configuração do less, o professor usou o ambiente dist e nele configuramos files: na mesma ordem que anteriormente, arquivo final, e arquivo inicial;  
adicionamos então a tarefa sass dentro do array da tarefa default; poderiamos tambem rodar npm run grunt sass, para rodar o sass separadamente, chamamos npm run grunt sass, o sass por padrão cria um sourcemap;  
podemos remover esse padrão através da configuração options: { noSourceMap: true}, podemos também definir que o arquivo deve ser minificado através da configuração options: { style: 'compressed'};  
### Aula 4 - Execute tarefas de forma paralela
#### **Sobre a aula**
* compreender os desafios de executar tarefas de forma serial e como isso pode causar lentidão no processo de automação de tarefas no Grunt;
* utilizar o plugin concurrent para executar tarefas de forma paralela no Grunt.
#### **Anotações**
Executar tarefas de forma serial pode ser demorado, o professor deu o exemplo usando nossa tarefa olaGrunt;  
para executar tarefas paralelas precisamos do plugin grunt-concurrent, como é um plugin do npm precisamos do grunt.loadNpmTasks, e para configurar adicionamos uma nova propriedade paralela ao less e ao sass chamada concurrent, ele pede outra propriedade chamada target a qual recebe as tarefas que queremos executar de forma paralela dentro de um array;  
### Aula 5 - Inicie um projeto com o Grunt
#### **Sobre a aula**
* organizar um projeto com o Grunt, incluindo a criação de estruturas de pastas e a configuração de ambientes de desenvolvimento e produção;
* configurar tarefas no Grunt para compilar código CSS usando um pré=processador como o LESS;
* explorar o uso de variáveis e importações de fontes externas em projetos Grunt para melhorar a manutenção e o desenvolvimento de estilos CSS.
#### **Anotações**
a partir dessa aula vamos construir uma aplicação pratica utilizando o grunt, usaremos alguns conceitos, vamos entender a dinâmica do ambiente, o que é um ambiente de desenvolvimento e o que é um ambiente de produção, quais são as diferenças;  
a aplicação é uma aplicação simples, é um sorteador de número, ela recebe como input o número máximo que vai ser sorteado e tem um botão que executa o sorteio;  
além de se aprofundar no grunt vamos conhecer melhor a parte de matemática no javascript, vamos usar uma função;  
vamos começar organizando as pastas, removemos todos os arquivos css, sass e less criados até então;  
a pasta src vai conter uma pasta scripts e uma pasta styles, essa é a pasta onde o programador vai trabalhar, em scripts teremos o código javaScript e em styles nosso código de estilo, nesse caso vamos usar o LESS;  
primeiro criamos o index.html dentro de src, alteramos o title e o idioma, adicionamos o código das fontes Roboto e Lobster;  
agora criamos o arquivo main.less e precisamos reconfigurar o gruntfile, removemos as configurações e os carregamentos do sass e do concurrent, além de remover nossa função olaGrunt;  
queremos configurar no grunt uma maneira de que o trecho production seja executado apenas na vercel (ambiente de produção), ambiente da internet;  
registramos a tarefa 'build' com grunt.registerTask e atribuímos a ela less:production, também adicionamos um script no package.json com o nome de build e configuramos que esse script roda grunt build;  
por fim estilizamos um pouco mais da nossa página, na proxima aula aprenderemos a configurar um watch.
### Aula 6 - Observe mudanças com o Grunt
#### **Sobre a aula**
* instalar e configurar o plugin de observação no Grunt;
* entender o funcionamento do watch;
* utilizar a observação de arquivos para otimizar o fluxo de trabalho.
#### **Anotações**
para fazer o grunt acompanhar as atualizações vamos precisar instalar o plugin grunt-contrib-watch, fazemos o carregamento através da função grunt.loadNpmTasks;
para a configuração abrimos um bloco depois do bloco do less dentro do grunt.initConfig, dentro desse um novo bloco para less, que é o nome da tarefa, dentro dele um array files, com os arquivos que serão observados, usamos asterisco duas vezes para que seja acessada qualquer pasta, e usamos um asterisco para referenciar qualquer arquivo, no caso especificamos .less;  
em seguida abrimos outra array tasks e definimos a tarefa a ser executada, no caso less:development, agora só falta substituir a tarefa default por watch;  
### Aula 7 - Comprima HTML com o Grunt
#### **Sobre a aula** 
* otimizar o código HTML de um projeto web usando o Grunt;
* configurar e usar o plugin grunt-replace;
* configurar e usar o plugin grunt-contrib-htmlmin para realizar a minificação de HTML.
#### **Anotações**
vamos seguir com a estilização, começamos rodando o grunt (watch), centralizamos o elemento main, em seguida estilizamos o botão e o input (d-block e bgcolor transparent);  
temos um problema no html, a folha de estilo está apontando para o arquivo criado no ambiente de desenvolvimento, porem no ambiente de produção, nós queremos que o html use o css minificado, vamos fazer essa duplicação do html através de um plugin do grunt, o grunt-replace, e seguimos aquela velha ladainha, install, load e depois config;  
para o config primeiro abrimos um bloco dentro de replace para o ambiente dev, nesse bloco abrimos um novo bloco options e nesse bloco options abrimos o array patterns que vai receber entre chaves o atributo match que recebe uma espécie de id a qual chamamos de ENDERECO_DO_CSS e um segundo atributo replacement que recebe o valor que irá substituir a id 'encontrada' pelo plugin, para realizar esse encontro, adicionamos @@ENDERECO_DO_CSS no href do stylesheet, para que o plugin realize a substituição;  
em seguida, depois de options, abrimos o array files e dentro dele abrimos chaves com os atributos expand: true, flatten: true, src: ['src/index.html] e dest: 'dev/', ou seja definimos o arquivo fonte do 'trabalho' do plugin (o arquivo onde adicionamos o @@) e definimos o destino desse 'trabalho': a pasta dev, os atributos expand e flatten, fazem com que o plugin expanda sua busca para outros diretorios e remova os diretórios de origem no momento de copiar os arquivos para o destino, respectivamente;  
o resultado de rodar npm run grunt replace:dev é a criação de um novo arquivo index.html na pasta de desenvolvimento, esse arquivo recebe no href do stylesheet o main.css do ambiente de desenvolvimento;  
vamos agora configurar o replace para o ambiente de produção, mas adicionalmente faremos também a minificação do arquivo html;  
para isso vamos precisar instalar um outro plugin: grunt-contrib-htmlmin, dai carregamos o plugin com o loadNpmTasks e configuramos para o ambiente dist, dentro dele configuramos as options removeComments: true e collapseWhitespace: true, e outro bloco para files sendo 'prebuild/index.html' o arquivo final e 'src/index.html' o arquivo fonte;  
a tarefa htmlmin cria a pasta prebuild para que ela contenha o arquivo minificado do html ainda contendo o placeholder @@ENDERECO_DO_CSS;  
a partir disso configuramos agora a tarefa replace para o ambiente dist, alteramos o options replacemente para './styles/main.min.css' e alteramos o files src para \['prebuild/index.html'\] e o dest para 'dist/';  
por fim vamos configurar a tarefa build para que ela rode em serie o less:production, htmlmin:dist e o replace:dist', essa tarefa vai criar o arquivo css minificado na pasta dist (less:production), criar o arquivo html minificado na pasta prebuild (htmlmin:dist) e por fim substituir o placeholder e criar o arquivo minificado final html na pasta dist (replace:dist);  
passei alguns dias sem conseguir realizar a tarefa build corretamente, perguntei ao chat gpt e ele sugeriu inverter a ordem das tarefas htmlmin e replace, não me satisfiz com essa proposta já que ela fugia da proposta da aula, então enviei meus arquivos para os tutores da ebac, o tutor respondeu com um arquivo gruntfile.js corrigido mas ele não percebeu que o que faltava era um ;, ao fazer a checagem linha por linha eu notei essa falta, corrigi e rodei npm run build, a minificação ocorreu corretamente assim como o replace;  
podemos agora configurar o grunt para apagar a pasta prebuild (que é uma pasta temporária) depois que ela é utilizada, fazemos isso através do plugin grunt-contrib-clean, instalamos, carregamos e configuramos ela recebe um array e dentro desse array adicionamos 'prebuild', por fim adicionamos a tarefa clean no fim da tarefa build;  
pra finalizar na task watch, vamos fazê-la também observar o html, para que o replace aconteça sempre, fazemos isso configurando html após o less dentro de watch com files: \['src/index.html'\], tasks: \['replace:dev'\]; dessa forma enquanto rodarmos o watch, qualquer alteração que fizermos no arquivo html dentro de src sofrerá o replace;
### Aula 8 - Conheça o JavaScript Math
#### **Anotações**
Vamos continuar com o projeto, agora fazendo a parte de lógica pra gerar o número aleatório;  
Para fazer isso usamos a função Math.random(); por padrão ela devolve um número entre 0 e 1; para fazer com que ela devolva o número dentro do escopo do número máximo precisamos múltiplicar o resultado entre 0 e 1 pelo número máximo, nessa aula veremos também como arredondar o número;  
para arrendodar temos três funções diferentes  
todo o nosso script (main.js dentro da pasta scripts sem src) vai estar dentro de um document.addEventListener sendo que o evento vai ser 'DOMContentLoaded' o que significa que quando o documento HTML for completamente carregado e analisado pelo navegador, quando isso ocorrer, a função anonima vai rodar, e todo o nosso script estara dentro desse código;  
a primeira parte é adicionar outro ouvinte de eventos para o 'form-sorteador', fazemos isso com document.getElementById e adicionamos a este o addEventListener com o evento 'submit' e a função function com o parametro (evento), no bloco dessa função adicionamos evento.preventDefault(); para impedir que o submit recarregue a página;  
em seguida declaramos a **let** numeroMaximo como document.getElementById('numero-maximo).value, ou seja, a **let** numeroMaximo é equivalente ao valor do input digitado pelo usuário;  
em seguida atualizamos essa **let** para ser parseInt de si mesma, assim garantimos que o valor será um valor inteiro;  
em seguida criamos outra **let** numeroAleatorio que vai ser igual a Math.random()*numeroMaximo, depois atualizamos numeroAleatorio para ser Math.floor de si mesmo + 1;  
como já vimos, Math.floor arredonda para baixo, adicionamos +1 ao valor de numeroAleatorio para prevenir que o resultado final seja 0;  
agora precisamos criar a estrutura HTML que vai exibir o valor de numeroAleatorio, para isso, dentro de main (nosso container), criamos a div resultado que contém o texto 'O número sorteado foi:' seguido de um span com id='resultado-valor', que vai receber o valor da variável;  
quanto a estilização