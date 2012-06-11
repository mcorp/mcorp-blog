---
author: vyper
date: '2010-05-27 16:57:36'
layout: post
slug: mapas-interativos-e-geoprocessamento
status: publish
title: Mapas interativos e Geoprocessamento
categories:
- desenvolvimento
tags:
- geo
- geoprocessamento
- georeferenciamento
- geoserver
- google
- google maps
- java
- jni
- mapa
- mapscript
- mapserver
- openlayers
- panoramio
- postgis
- postgresql
- street view
---

Atualmente vemos muitas aplicações e sites utilizando mapas (nem que seja
apenas pra mostrar o endereço da empresa), essa facilidade que o
[Google](http://www.google.com.br) trouxe com o [Google Maps](http://www.google.com.br/maps) é muito grande, ajudou muito na
disseminação dessa tecnologia.

[![](http://www.mcorp.com.br/wp-content/uploads/2010/05/pico-parana-300x201.jpg "Foto por: Everson Novka")](http://www.mcorp.com.br/wp-content/uploads/2010/05/pico-parana.jpg)

Essa facilidade de implementação, nos dá maiores opções para criar sistemas e
interfaces bem diferenciadas, como o [Zoomii Books](http://zoomii.com) (mesmo
não usando Google Maps) que implementou uma loja virtual de livros e o
[Trulia](http://www.trulia.com/), um site de uma imobiliária americana que
chega até o nível de visualizar a casa como se estivessemos em frente a frente
(utilizando o _Street View_).

Abaixo vemos o Google Maps e integrado ao
[Panoramio](http://www.panoramio.com/), dando até a opção de navegação
panorâmica das fotos georeferenciadas.

[![](http://www.mcorp.com.br/wp-content/uploads/2010/05/pico-parana-google-maps-150x150.png)](http://www.google.com.br/maps?ll=-25.255525,-48.845794&spn=0.027751,0.054846&t=h&z=15&iwloc=lyrftr:com.panoramio.all,3787114327184159595,-25.255525,-48.845773&lci=com.panoramio.all) [![](http://www.mcorp.com.br/wp-content/uploads/2010/05/pico-parana-vista-panoramica-150x150.png)](http://www.google.com.br/maps?ll=-25.255525,-48.85397&spn=0,0.054846&t=h&z=15&lci=com.panoramio.all&layer=c&cbll=-25.255525,-48.845794&cbp=12,0,,0,5&photoid=po-18636660)

Mas acontece que o pessoal do Google abstraiu muito a dificuldade que é
trabalhar com informações georeferenciadas. Fez com que todas essas
funcionalidades (busca de endereços, pontuação, pesquisa por polígonos) e
informações (mapas, imagens de satélites, rotas) ficasse algo simples e
totalmente transparente para o desenvolvedor. O desenvolvedor sabe que
passando o endereço, aparece um ponto no mapa, e isso as vezes basta, mas
quando precisamos ir mais além, vemos que a coisa não é tão simples assim.

Senti essa dificuldade tempos atrás, onde tiver que implementar um projeto
utilizando tecnologias livres e nos requisitos estavam:
[Java](http://java.sun.com), [MapServer](http://www.mapserver.org),
[MapScript](http://www.mapserver.org/mapscript/) e
[PostgreSQL](http://www.postgresql.org) (com
[PostGIS](http://postgis.refractions.net)). Descobri que não é só utilizando
endereço e, a inválivel dupla, latitude/longitude que identificaremos os
pontos no mapa, descobri também a existência das coordenadas
[UTM](http://pt.wikipedia.org/wiki/Universal_Transversa_de_Mercator) e a
diferença entre as regiões.

Posteriormente descobri que uma alternativa de desenvolvimento ao MapServer, é
o [GeoServer](http://geoserver.org/), escrito nativamente em Java, que
facilitaria - e muito - o meu trabalho, pois o MapScript é uma biblioteca em C
e por isso foi necessário utilizar [JNI](http://pt.wikipedia.org/wiki/JNI).

Com tudo isso, descobri que geoprocessamento é bem mais do que um mapa com
alguns pontos e alguns polígonos, tem muito mais coelho nesse mato. O
geoprocessamento e georeferenciamento não é algo tão simples, não é a toa que
existem os [Engenheiros Cartográficos](http://pt.wikipedia.org/wiki/Engenharia_geogr%C3%A1fica).

Depois desse projeto, vi que curti demais a área e estou me aprofundando na
medida do possível, todos os estudos que eu for realizando relatarei por aqui.
E o primeiro estudo que relatarei será o
[OpenLayers](http://www.openlayers.org).

