---
author: vyper
date: '2011-10-25 09:52:00'
layout: post
slug: soap-vs-rest
status: publish
title: SOAP vs REST
wordpress_id: '631'
categories:
- arquitetura
- desenvolvimento
tags:
- json
- rest
- soa
- soap
- xml
---

Tenho visto vários artigos e discussões dizendo que SOAP acabou, que a nova
solução para todos os problemas do mundo é o REST (de preferência com JSON).
Porém, qualquer pessoa que pare para pensar - o mínimo que seja - notará que
no mundo real nem tudo é mil maravilhas como dizem ser o REST e nem tão ruim
quanto dizem ser o SOAP.

Para quem está envolvido com serviços no dia-a-dia poderia dar vários pontos
de vantagens e desvantagens sobre as duas tecnologias. Mas qualquer ponto será
levantado dependendo do contexto de quem aplica ou utiliza a tecnologia.

Mas, antes de discussão técnica e filosófica, um ponto que eu acho bastante
importante é: até onde vale a pena entrar na discussão da melhor solução e/ou
tecnologia? Sei lá, mas entrarei mesmo assim.

As pessoas deveriam se preocupar em resolver os problemas da melhor maneira e
não tentar encontrar/criar um padrão que resolva todos os problemas.

## Historinhas fictícias

### Historinha 1

<History type="ficção" context="enterprise" description="padrão perfeito X"
observation="Qualquer semelhança com a vida real será pura coincidência">
<Content> 1 - Temos o "padrão perfeito X", chamado a partir de agora ppX; 2 -
O ppX é grande, foi criado para resolver todo tipo de problema do mundo
tecnológico; 3 - Muitas pessoas enterprise (vulgo PE) usam e amam o ppX e são
felizes com ele; 4 - Toda palestra, evento e rodinha das PE só se ouve falar
do ppX, bem, é lógico; 5 - Usam ele para todo e qualquer problema; 6 - Eles
sabem que o ppX tem seus problemas, mas é super confiável; 7 - **Eles resolvem
seus problemas com o ppX e são felizes com o que fazem.** </Content>
</History>

### Historinha 2

history = { type: 'ficção', context: 'non-enterprise', description: 'padrão
perfeito X', observation: 'Qualquer semelhança com a vida real será pura
coincidência', content: ' 1 - Existe o "padrão perfeito X", chamado a partir
de agora ppX; 2 - O ppX é grande, foi criado para resolver todo tipo de
problema do mundo tecnológico; 3 - Muitas pessoas enterprise (vulgo PE) usam e
amam o ppX e são felizes com ele; 4 - Muitas pessoas não-enterprise (vulgo
PNE) pouco usam e odeiam o ppX; 5 - Toda palestra, evento e rodinha das PNE só
se ouve falar mal do ppX; 6 - Resolvem inventar o "padrão perfeito aberto X"
(adivinha? Errou. Vulgo A-ppX); 7 - Usam ele para todo e qualquer problema; 8
- Eles sabem que o ppX tem seus problemas, então fazem desses problemas a
causa da fome no mundo; 9 - Mostram a todos que o A-ppX é a perfeita solução
para todos os problemas do mundo, menos a fome; 10 - **Eles resolvem seus
problemas com o A-ppX e são felizes com o que fazem.** '}

## Se é que alguém chegou até aqui...

  
Cada um no seu quadrado **ou não**.

**Espertas** são as pessoas que conseguem juntar o melhor dos dois mundos indiferente do contexto onde ela esteja. Porque, no fim, o que importa REALMENTE é: **resolver seus problemas e ser feliz com o que faz**.

