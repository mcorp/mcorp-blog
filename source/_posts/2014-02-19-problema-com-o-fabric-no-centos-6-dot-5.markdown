---
author: Leonardo Saraiva
date: 2014-02-19 09:50
layout: post
title: "Problema com o fabric no CentOS 6.5"
slug: problema-com-o-fabric-no-centos-6-5
comments: true
status: publish
categories:
- arquitetura
- desenvolvimento
tags:
- ruby
- rails
- python
- fabric
- capistrano
- vagrant
- centos
- ubuntu
- deploy
- pip
- ecdsa
- pycrypto
---

Estava "brincando" com o [fabric](http://fabfile.org) para automatizar o deploy de uma aplicação que estou trabalhando, tudo bem que normalmente trabalho com aplicações [Ruby](http://ruby-lang.org) (e [Rails](http://rubyonrails.org)) e servidores [Ubuntu](http://ubuntu.com), então acabo sempre utilizando o [capistrano](http://capistranorb.com) para deploy, que é uma ótima opção.

Mas nesse projeto, que é um pouco diferente, estou usando "um" [CentOS](http://centos.org/) 6.5 no [vagrant](http://vagrantup.com), para deixar o mais próximo do ambiente de produção, que é um [Amazon Linux](http://aws.amazon.com/amazon-linux-ami/) e, para não ter que "instalar o Ruby", pensei: "vou de [Python](http://python.org) que tem em todas as distribuições, vai ser baba!". Afinal, o fabric é uma baita ferramenta e tem um perfil melhor para deploy de aplicação indiferente da linguagem... Mas foi aí que encontrei um problema que me fez perder um certo tempinho.

Eu não sei explicar bem porque ocorreu, imagino que seja alguma inconsistência entre os pacotes instalados pela distribuições (usando o yum) e os pacotes que instalei via pip. Mas, sinceramente, esperava um pouquinho mas da resolução de dependências do [pip](https://pypi.python.org/pypi/pip).

## O erro

Ao tentar executar qualquer comando com o fabric recebia a mensagem abaixo:

    $ fab --version
    Traceback (most recent call last):
     File "/usr/bin/fab", line 5, in <module>
       from pkg_resources import load_entry_point
     File "/usr/lib/python2.6/site-packages/pkg_resources.py", line 2655, in <module>
       working_set.require(__requires__)
     File "/usr/lib/python2.6/site-packages/pkg_resources.py", line 648, in require
       needed = self.resolve(parse_requirements(requirements))
     File "/usr/lib/python2.6/site-packages/pkg_resources.py", line 546, in resolve
       raise DistributionNotFound(req)
    pkg_resources.DistributionNotFound: ecdsa

## A solução

Para mim a solução foi instalar o tal do pacote [ecdsa](https://pypi.python.org/pypi/ecdsa) e uma versão específica da [pycryto](https://pypi.python.org/pypi/pycrypto), pois a distribuição vem com uma versão antiga, inicialmente apenas tentei atualizar e foi para a versão 2.4, mas continuei recebendo erro. Então descobri que na versão 2.3 o problema não ocorre.

    sudo pip install ecdsa
    sudo yum install python-devel
    sudo pip uninstall pycrypto
    sudo pip install PyCrypto==2.3

E bom uso do fabric sobre o CentOS 6.5! (:

*Observação:* tive outros problemas diversos e descobri que o "maior" problema é a versão do Python que estou utilizando, a 2.6, muita gente falou para atualizar para a 2.7 (ao menos). Mas por enquanto os maiores problemas eu consegui resolver.

