# Novo-Modulo
Instruções sobre como criar um novo módulo para opencart 3.x

# Introdução 👋
## Considerações Iniciais
  Olá, esse readme tem a intenção de facilitar sua caminhada através do Opencart e a criação do seu módulo, tudo o que apresentar aqui deriva das minhas pesquisas pessoais durante minha experiência criando módulo e trabalhando com a plataforma Opencart, por isso é não está isento de equívocos e faltas, por mais que eu tente sempre seguir documentações oficiais e fontes confiáveis. Caso você encontre uma incoerência ou erro por favor abra uma issue e caso tenha conteudo para agregar abra uma PR, ademais espero que seja de grande ajuda, enjoy it 🤗. 

# 1 - Requisitos
Para facilitar a criação do módulo, alguns requistos prévios são necessarios, vou listar aqui quais e onde encontra conteúdo:

1. Extensão OCMOD e seu funcionamento : https://webocreation.com/blog/ocmod-documentation/ && https://github.com/opencart/opencart/wiki/Modification-System
1. - Estrutura de uma extensão/módulo : http://docs.opencart.com/en-gb/developer/module/
1. - Tutorial completo de como desenolver o módulo : https://webocreation.com/blog/opencart-3-custom-module-development-tutorial-hello-world-module/ && https://medium.com/@justinasjbeinorius/building-your-first-opencart-3-extension-d766df28821b

## Resumo 🗒️
A estrutura básica de uma extensão para Opencart consiste em 1 arquivo e um diretório : **Install.xml** e **UPLOAD**.

* **Install.xml** : Consiste no arquivo que dá instruções sobre a extensão e também permite que se escreva dentro de arquivos pré-existentes no projeto opencart.
* **UPLOAD** : Esse diretório vai armazenar todos os arquivos da sua extensão, seguindo a estrutura do projeto opencart, esses arquivos serão integrados ao projeto.

![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)

## Observações 👀
* Tente criar extensões que correspondam aos seus respectivos tipos, por exemplo, uma extensão alimentadora como a integração com um ERP corresponde melhor a um "alimentador" do que a um "módulo", uma extensão que integra uma nova forma de pagamento é explicitamente do tipo "pagamento" e uma função para um popup de confirmação de maior idade corresponde a um "módulo" pois implementa uma funcionalidade que afeta diretamente a experiência de navegar do cliente.
* Alguns módulos podem necessitar de "variações" como um módulo de banners ou de parcelamento, para isso implemente conforme o link **2** em requisitos, já outros módulos não precisam de tais implementações, pra tais recomendo armazenar todas as configurações do módulo na tabela "oc_settings" ao invés da "oc_module".
* É de extrema importância para integridade do e-commerce que os métodos ```function install()``` e ```function uninstall()``` sejam implementados, principalmente se você criar novas tabelas no banco de dados, esse métodos criam e destroem respectivamente informações necessarias a extensão no banco de dados do projeto.
* Lembre-se de sempre ao instalar a extensão de recarregar as modificações em **Extensões -> modificações** no painel do administrador.

![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)

* Caso tenha problemas com permissão ao instalar a nova extensão se certifique que possui permissões para alterar o caminho correspondente a este em **Configurações->gerenciar usuários->grupo de usuários** seleciona seu tipo de usuario e procure pelo caminho da extensão e caso não esteja marcada a marque e salve as configurações.

![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)
# Recomendações
* Se possivel opte por salvar informações da configuração da extensão na tabela oc_setting.
* O opencart oferece diversas funções que podem te ajudar a regastar informações que você talvez precisa como por exemplo o método ```$this->cart()``` que oferece informações referentes ao carrinho da sessão ou ```$this->user()``` para resgatar informações sobre o usuário corrente, para mais informações acesse [Funções Úteis](#funções-úteis).

# Estilos 🎨
  O Opencart 3.x utiliza como framework para estilo o [Bootstrap 3.3.x](https://getbootstrap.com/docs/3.3/css/) o qual recomendo que você também utilize para suas extensões, como pacote de ícones ele utiliza o [FontAwesome](https://fontawesome.com/v4.7/icons/) que também recomendo que use para seguir o padrão, por padrão o opencart utiliza dois botões na tela de configurações de extensões, um para salvar outro para cancelar/voltar, para manter o padrão de estetica tambem recomendo o uso destes: 
       ```<div class="pull-right">
         <button type="submit" form="form-module" data-toggle="tooltip" title="{{ button_save }}" class="btn btn-primary"><i class="fa fa-save"></i></button>
        <a href="{{ action.cancel }}" data-toggle="tooltip" title="{{ button_cancel }}" class="btn btn-default"><i class="fa fa-reply"></i></a>
        </div>```.

## Twig 🌳
  O Twig é um criador de templates HTML que é utilizado por padrão no Opencart e você pode encontra a documentação dele [aqui](https://twig.symfony.com/doc/3.x/),
a principal útilidade do twig é a sintaxe fácil com a qual podemos usar PHP dentro dele, você pode ler sobre essa ferramenta [aqui](https://twig.symfony.com/doc/3.x/coding_standards.html) mas vou citar um breve resumo de como funciona:

* Use ```{``` e ```}``` como demilitadores e sempre coloque um espaço depois de abrir e antes de fechar os delimitadores, e então o texto compreendido entre eles será interpretado como código PHP, exemplo:
            ``` {{ foo }}```

* É possivel usar operadores lógicos como em PHP 
  ``` {% if foo %}{% endif %} ```
  
* Tambem é possivel acessar índices de um array, como por exemplo, caso você tenha ```$data['usuario']['nome']``` você pode utilizar:
  ``` {{ usuario.nome }} ```
  para acessá-lo. 
  
  
# Back-End
  O opencart utiliza da estrutura MVC-L para estruturar pastas e arquivos, vamos navegar um pouco por cada uma dessas siglas:
![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)

## Model
  O model é o diretório onde são armazenadas as instruções referentes a operações no banco de dados, por padrão apenas estas são armazenadas em arquivos dentro do model, comumente as funções pertencentes a uma classe model se resumem a "querys", consultas ou operações a um banco de dados, você pode encontrar mais informações sobre querys em [Funções do Banco de dados](#Funções-do-Bando-de-dados).
  
## View
  A View é o diretório onde são armazenados os arquivos do front-end, comos os templates Twig, o bootstrap e scripts javascript, nosso projeto base utiliza o tema [Journal](https://www.journal-theme.com/), por tanto você provavelmente irá se deparar com arquivos e diretórios que contenham esse nome ao navegar pelo projeto, principalmente na no diretório View, alguns e-commerces utilizam o tema em alguma parte do site e outros não, por isso recomendo sempre que alterar um arquivo via **install.xml** que você incluia os arquivos do Journal3 a sua alteração, para evitar comportamentos indesejados e retrabalho.
  
## Controller
  O controller é o diretório onde são armazenados os arquivos principais do projeto, os arquivos do tipo controller são os únicos "chamados" pela aplicação e por consquencia são eles quem "chamam" as views e os models, por padrão a função principal e que executa todas as chamadas é a função ``` index() ``` , nela são definidas os conteúdos que serão enviados para o front-end, nessa classe também podem ser criados outros métodos como o ```validate()``` que válida autorizações, você pode e é bem provavel que precise criar seus próprios métodos, para isso recomendo  que crie métodos de responsabildiade única e use [camelCase](https://pt.wikipedia.org/wiki/CamelCase) para nomeá-los.
  
## Language
  O Language é o diretório que armazena o vocabulário do Opencart, o que facilita o reuso de algumas palavras que podem aparecer com frequência no projeto. Por padrão o opencart armazena o texto no array **_** você pode declarar um novo texto dessa forma : ```$_['texto-chave'] = 'Texto Valor'; ```.
  
# Funções úteis
  Por padrão o opencart possui algumas classes que trazem métodos que podemos implementar durante a construção de extensões e que facilitam o trabalho, nessa sessão vou citar os mais útilizados por mim:
  
 ## Loader
  * Load : Para carregar métodos de Model,Controller,Languages e Views podemos utilizar ```$this->load->``` passando o tipo de arquivo que estamos carregando assim como seu caminho, como por exemplo ```$this->load->model('extension/module/meumodulo')``` a partir disso caso precise implementar uma função do model **meumodulo** apenas faça ```$this->model_extension_module_meumodulo->minhafuncao()```.
  
 ## DB
  * Query : Caso precise fazer uma query no seu banco de dados, utilize ```$this->db->query($sql)``` passando como argumento a sua consulta, para obter o resultado da sua consulta você pode utilizar ```$this->db->query($consulta)->row()``` caso queira apenas a primeira linha do resultado ou ```$this->db->query($consulta)->rows()``` caso queira um array com todas as linhas resultantes, ademais recomendo sempre conferir sua sintaxe Mysql e se lembrar que essa função realiza apenas uma consulta por vez.
  * Escape : É importante sempre "esterilizar" uma string caso formos utiliza-lá em uma consulta, principalmente se ela é proveniente de um Input, para evitar [Sql Injections](https://www.devmedia.com.br/sql-injection/6102) dito isso o Opencart oferece o método **escape** que pode ser chamado por ```$this->db->escape($value)``` recebendo como argumento sua string.

## Cart
  * GetProducts : Esse método resgata informações sobre os produtos do carrinho corrente e pode ser chamado por ```$this->cart->getProducts()```.
  * Add : Esse método adiciona um novo item ao carrinho sua chamada pode ser feita por ```$this->cart->add($product_id, $quantity = 1, $option = array(), $recurring_id = 0) ``` e recebe como argumento, o id do produto, a quantidade, as opções do mesmo.
  * HasProducts : Confere se existem produtos no carrinho pode ser chamada através de ``` $this->cart->hasProducts() ``` e não recebe argumentos.
  * GetTotal : Fornece o valor total no carrinho pode ser chamado através de ``` $this->cart->getTotal() ``` e não recebe argumentos.
  
## Customer
  Aqui é possivel também acessar todas as propriedades do objeto customer instânciado na sessão, através de seus métodos acessores, aqui vão os possivelmente mais úteis :
  **ps:** todos os métodos acessores são chamados seguindo essa estrutura : ```$this->customer->getPropriedade() ```.
  * getId : retorna o Id do cliente.
  * getFistName : retorna o primeiro nome do cliente.
  * getLasName : retorna o sobrenome do cliente.
  * getEmail : retorna o email do cliente.
  * getTelephone : retorna o telefone do cliente.
  * getAdressId : retorna o id do endereço cadastrado.
 
# API 🖥️
Em desenvolvimento.:hammer:	

# Considerações Finais.
Por fim gostaria de complementar com um pequeno lembrete, se possivel sempre **idente e documente** seu código pra facilitar a manutenção, uma ótima referência é o [PSR-12](https://www.php-fig.org/psr/psr-12/) que ensina boas práticas de pradronização pra desenvolvimento em php, é bom lembrar que ainda tenho muito a aprender, então se você reparar qualquer equívoco em qualquer instrução escrita aqui não exite em me contatar e é claro PR são sempre bem vindas, espero que esse conteudo facilite um pouco sua jornada através do Opencart, valeu e até proxima 👋 🤗
