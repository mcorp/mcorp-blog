---
layout: post
title: "Considerações da migração do Wordpress para Octopress"
date: 2012-06-11 03:41
comments: true
categories: 
- desenvolvimento
tags:
- wordpress
- octopress
---

Nesse feriadão fui 'obrigado' a reinstalar meu servidor. E, por não utilizar PHP em nenhum dos meus projetos, tentei encontrar uma maneira de não instalá-lo no servidor. Após uma pequena pesquisa, lembrei do [Octopress](http://www.octopress.org), que tem muita gente utilizando.

Brincando um pouco com ele, vi que era divertido, porém fiquei meio receoso com a migração... que poderia ser (e foi - até certo ponto) trabalhosa.

Quando for fazer a sua, ou antes disso, leve alguns itens em consideração:

* não confie inteiramente no [exitwp](https://github.com/thomasf/exitwp);
* se prepare, porque terá que passar em todos os posts para revisá-los;
* o exitwp perderá algumas formatações ou tags:
  * [caption] do Wordpress;
  * iframe (google calendar, google docs e youtube);
  * anchor (links ao estilo #atalho) tem que ser escritos com html mesmo;
  * table;
* o octopress não tem todas as frescuras do wordpress, então é possível que você sinta falta de algumas coisas (modal pra fotos é um exemplo);
* não tem a facilidade de instalação das coisas como temos com o Wordpress, então, se prepare que muita coisa será manual;
* o [exitwp](https://github.com/thomasf/exitwp) vai migrar suas fotos como fotos simples, se quiser posicionamento e afins, terá que usar a [image tag](http://octopress.org/docs/plugins/image-tag/), ou seja, revisar todas as fotos;
* para snippets de código encontrei três opções: 
  * usar a tag [code blocks](http://octopress.org/docs/plugins/codeblock/);
  * usar a tag [gist](http://octopress.org/docs/plugins/gist-tag/);
  * usar o simples markdown;
* apesar de suportar tags, elas não aparecem em lugar algum. Utilizei o plugin [tag-generator](https://github.com/robbyedwards/octopress-tag-pages) para gerar páginas de navegação nas tags e feed;
* seus posts importados pelo exitwp vão vir sem nenhuma configuração para comentários, caso queira por comentários neles use o código abaixo (isso é bash):
  {% gist 2908802 %}

Mas apesar de tudo, aparentemente, valeu a pena. Futuramente preciso verificar mais umas coisinhas:

* descobrir como fazer o mesmo 'fonte' suportar vários blogs;
* fiz uma pesquisa superficial e vi que é possível, só queria não ter que mexer em muita coisa pra suportar o deploy para o S3 da Amazon;
* achar um jeito das imagens abrirem em modal;
* personalizar o tema, afinal, quase todo mundo que instala o octopress mantém o tema padrão;
* descobrir a melhor maneira de fazer iframes, tables e outras tags htm.

**Update:**
* Procurando como fazer um formulário de contato com Octopress, encontrei esse [post](http://va.mu/WCGa), fica a dica;

E por enquanto é isso pessoal!

**Obs.**: esse é meu primeiro post 100% Octopress!
