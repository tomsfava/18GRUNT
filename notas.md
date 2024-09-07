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
vamos seguir com a estilização, começamos rodando o grunt (watch)