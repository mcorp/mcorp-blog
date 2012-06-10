---
author: vyper
date: '2010-03-08 20:03:58'
layout: post
slug: consumindo-um-servico-seguro-utilizando-php
status: publish
title: Consumindo um serviço seguro utilizando PHP
wordpress_id: '256'
categories:
- desenvolvimento
tags:
- data services
- dss
- esb
- framework
- java
- php
- ws-security
- wso2
---

Dia desses precisei colocar autenticação em um serviço que desenvolvi e
passeando pelas opções da interface administrativa da [WSO2 Enterprise Service
Bus](http://wso2.org/downloads/esb) encontrei facilmente minha solução. Um
pouco depois, li no twitter do @[WSO2](http://twitter.com/wso2) um comentário
sobre um novo artigo publicado sobre segurança nos [Data
Services](/glossario/#DataServices), onde ensinava a fazer toda a parte de
configuração: [Content Filtering in Data Services with User
Roles](http://wso2.org/library/articles/content-filtering-data-services-user-
roles).

Só que pouco tempo depois de configurar e entregar ao cliente o serviço, veio
o problema: como consumir em [PHP](http://www.php.net/) o serviço? No artigo a
solução entregue por eles para consumo é em [Java](http://java.sun.com/), como
a capacidade do cliente deixa a desejar, fiquei de fazer e enviar um exemplo
de consumo do serviço desenvolvido.

Então... mãos na massa!

## Configurando a ESB

Nesse ponto não irei escrever passo-a-passo a configuração que deve ser feita,
o trabalho realizado pelo pessoal do WSO2 está muito bem feito nessa parte do
artigo: [Step 4 – Enable User Authentication for the Data
Service](http://wso2.org/library/articles/content-filtering-data-services-
user-roles#step_4).

## Consumo sem segurança

Um pequeno trecho de PHP que mostra como consumir o serviço sem nenhum tipo de
segurança. Código pequeno, limpo e objetivo.

[code language='php'] $client = new
SoapClient("http://localhost:8280/services/Tutorial?wsdl"); try { $obj =
$client->searchProductsByGroupId(array("group_id" => 1)); print_r($obj);

} catch (Exception $e) { echo "ERRO: " . $e->getMessage(); }[/code]

Mas mesmo com toda essa objetividade, o problema do cliente não estava
resolvido e eu recebia o seguinte erro:

[code]ERRO: SOAP header missing[/code]

E, para resolver, faltava adicionar o cabeçalho com os dados de segurança.

## Consumo com segurança

Agora um não-tão-pequeno trecho em PHP.

[code language='php']// configurações de conexão $username = "admin";
$password = "admin"; $created = date ("Y-m-d\TH:i:s", mktime (date("H"),
date("i"), date("s"), date("m"), date("d"), date("Y")))."Z";

// definição de namespaces $ns = 'http://docs.oasis-
open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd'; $wsu =
'http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-
utility-1.0.xsd';

// criando elemento UsernameToken $token = new stdClass; $token->Username =
new SOAPVar($username, XSD_STRING, null, null, null, $ns); $token->Password =
new SOAPVar($password, XSD_STRING, null, null, null, $ns);

// criando elemento Timestamp $timestamp = new stdClass; $timestamp->Created =
new SOAPVar($created, XSD_STRING, null, null, null, $wsu);

// criando elemento Security $wsec = new stdClass; $wsec->UsernameToken = new
SoapVar($token, SOAP_ENC_OBJECT, null, null, null, $ns); $wsec->Timestamp =
new SoapVar($timestamp, SOAP_ENC_OBJECT, null, null, null, $wsu);

// criando header $headers = new SOAPHeader($ns, 'Security', $wsec);

// construtor do web-service (com cabeçalho) passando o endereço do WSDL
$client = new SoapClient("http://localhost:8280/services/Tutorial?wsdl");
$client->__setSOAPHeaders($headers);

// chamada do método try { $obj =
$client->searchProductsByGroupId(array("group_id" => 1)); print_r($obj);

} catch (Exception $e) { echo "ERRO: " . $e->getMessage(); }[/code]

E tudo rodou normalmente, sem problemas, o cliente ficou feliz e eu matei um
pouco da saudade de programar em PHP.

Na pesquisa por soluções, tentei utilizar o [WSO2 Web Services Framework _for
PHP_](http://wso2.org/projects/wsf/php) mas descobri que depende de instalação
de módulo no [Apache](http://httpd.apache.org) e não servia para meu cliente,
mas anotei na pauta para testá-lo em outro momento.

