---
author: Leonardo Saraiva
date: 2013-09-21 11:50
layout: post
title: "Melhorando o tempo de pesquisa utilizando Sphinx"
slug: melhorando-o-tempo-de-pesquisa-utilizando-sphinx
comments: true
status: publish
categories:
- arquitetura
tags:
- sphinx
- mysql
- postgresql
- indexação
- fulltext search
- solr
- lucene
- elasticsearch
- rails
- ruby
---

    SELECT firstname, lastname, nickname, email FROM users
    WHERE  firstname LIKE '%my query%' OR lastname LIKE '%my query%'
        OR nickname LIKE '%my query%' OR email LIKE '%my query%'

Quem nunca viu (ou fez) uma query como essa? Mas, esses, seus problemas acabaram! [TL;DR](#melhorando-o-tempo-de-pesquisa-utilizando-sphinx-TL-DR)

A busca é sempre um problema para nossas aplicações, porque sempre tentamos trazer o melhor e mais relevante resultado, o que nem sempre é fácil. Normalmente envolve diversas tabelas, busca textual em diversos campos e alguns outros problemas que estamos acostumados e que num sistema inicial nunca é um problema. Alguém já teve problema de performance com queries num sistema com apenas 1000 registros? Difícil não é mesmo?

Agora, se começarmos a pensar num sistema maior e que começa a atingir algo na casa dos milhões de registros, começa ficar complicado fazer queries como a que nós abrimos esse texto, não é mesmo? Ela até funciona, mas e como deixar isso rápido?

Soluções? Existem várias! Alguns dos mais conhecidos são: [Solr](http://lucene.apache.org/solr/) (que utiliza [Lucene](http://lucene.apache.org/) por baixo), [SphinxSearch](http://sphinxsearch.com/), [ElasticSearch](http://www.elasticsearch.org/) ou, ainda, utilizar a fulltext search que o seu banco de dados implementa (a maioria deles tem).

Mas irei abordar o uso do Sphinx em uma aplicação Rails, todos os engines terão suas vantagens e desvantagens, não adianta ficarmos discutindo “sexo dos anjos” e tentando convencê-los a usar um ou outro, vou apenas mostrar um pouco da maneira como trabalha e você identifica se resolve seu problema ou não. (;<a name="melhorando-o-tempo-de-pesquisa-utilizando-sphinx-TL-DR"></a>

## Rails + MySQL + Sphinx

No exemplo em que vou mostrar, em Rails, vamos utilizar a ótima gem thinking-sphinx, que nos ajuda em vários dos pontos dessa integração com o Sphinx e o MySQL.

### Instalação e configuração

Instalando os serviços e bibliotecas do MySQL e do Sphinx

    $ sudo aptitude install mysql-server sphinxsearch libmysqlclient-dev

Criando o projeto de exemplo (assumindo que você tenha todo o ambiente de Ruby e Rails funcionando)

    $ rails new sphinx-test

Adicione a gem thinking-sphinx e do mysql2 ao seu projeto

    $ cd sphinx-test
    $ echo 'gem "thinking-sphinx"' >> Gemfile
    $ echo 'gem "mysql2"' >> Gemfile
    $ bundle install

Edite seu config/database.yml para utilizar o MySQL

{% gist 6653262 database.yml %}

### Adicionando a configuração do índice

Agora vamos criar um model para usarmos como exemplo

    $ rake db:create
    $ rails generate model User firstname:string lastname:string nickname:string email:string
    $ rake db:migrate

E agora criar o arquivo com o índice (em app/indices/user_index.rb)

{% gist 6653262 user_index.rb %}

### Brincando com os dados usando o Sphinx

Gerando o arquivo de configuração e iniciando o serviço

    $ rake ts:configure
    $ rake ts:index
    $ rake ts:start

Agora temos a aplicação configurada e integrada com p sphinx, seu indexador está rodando e basta inserirmos alguns registros

    $ rails console
    > User.create([{ firstname: "Leonardo", lastname: "Saraiva", nickname: "Leonardo", email: "leonardo@mcorp.com.br"}, { firstname: "Alfred", lastname: "Hitchcock", nickname: "Ally", email: "alfred@hitchcock.com"}, { firstname: "Stephen", lastname: "King", nickname: "Stephen", email: "stephen@king.com" }])

E agora... buscar! (ainda no rails console)

    > User.search "Leonardo"
       Sphinx Query (3.9ms)  SELECT * FROM `user_core` WHERE MATCH('Leonardo') AND `sphinx_deleted` = 0 LIMIT 0, 20
       Sphinx  Found 0 results
    => []

Ué? O que rolou? Nada veio? Não funcionou? Não é bem isso. É que o Sphinx precisa saber que os novos registros foram inseridos, então é necessário avisá-lo. Ou seja, é necessário reindexar! Ensinarei aqui a maneira “manual” de reindexação, não existe uma melhor forma, isso deve ser analisado de projeto a projeto, índice a índice. Depende de muitas coisas, e dependendo do tamanho de tudo, pode ser demorado bastante demorado, necessitando fazer a reindexação de alguma maneira assíncrona ou ainda de tempos em tempos.

Mas para vocês verem funcionando, vamos aos comandos:

    $ rake ts:rebuild
    $ rails console
    > User.search "Leonardo"
      Sphinx Query (2.9ms)  SELECT * FROM `user_core` WHERE MATCH('Leonardo') AND `sphinx_deleted` = 0 LIMIT 0, 20
      Sphinx  Found 1 results
      User Load (0.8ms)  SELECT `users`.* FROM `users` WHERE `users`.`id` IN (1)
    => [#<User id: 1, firstname: "Leonardo", lastname: "Saraiva", nickname: "Leonardo", email: "leonardo@mcorp.com.br", created_at: "2013-09-21 18:27:31", updated_at: "2013-09-21 18:27:31">]

E, como podem ver, agora temos um retorno! O Sphinx tem diversas opções e a gem thiking-sphinx tem uma documentação bastante completa.

### Algumas dicas a mais

Outros exemplos a mais de busca utilizando operadores

    > User.search "Leonardo | Stephen" # Leonardo OU Stephen
    > User.search "Leonardo & Saraiva" # Leonardo E Saraiva

E aqui vão os comandos que o thinking-sphinx disponibiliza para nós

    $ rake -T | grep '\sts:'
    rake ts:clear               # Clear out Sphinx files
    rake ts:configure           # Generate the Sphinx configuration file
    rake ts:generate            # Generate fresh index files for real-time indices
    rake ts:index               # Generate the Sphinx configuration file and process all indices
    rake ts:rebuild             # Stop Sphinx, index and then restart Sphinx
    rake ts:regenerate          # Stop Sphinx, clear files, reconfigure, start Sphinx, generate files
    rake ts:restart             # Restart the Sphinx daemon
    rake ts:start               # Start the Sphinx daemon
    rake ts:stop                # Stop the Sphinx daemon

[]s
