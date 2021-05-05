---
title: Sobre Shinobi
description: Shinobi e uma protocolo completamente decentralizado por trocando na Ubiq.
createdAt:
---

## O que é Shinobi

Shinobi é uma protocolo por criando liquidez e trocando de tokens ERC-20 na Ubiq.  Shinobi elimina a necessidade de intermediários confiáveis e outras formas de tarifas desnecessárias, permitindo transações rápidas e eficientes.  Descentralização, a resistência à censura e a segurança são priorizadas.  Shinobi é software livre, baseado em UniswapV2, usando licença GPL.

## Como funciona o Shinobi?

Shinobi e um protocol de liquidez automatizado. Na terminologia comum, isso significa que existe smart contracts de modelo que definir um método padrão para criar reservas (ou "pools") de liquidez e mercados correspondentes que são compatível.  Nesse sistema, não existe um bloco de pedido, entidade centralizada ou facilitador central de trocas. Os reservatórios sao definidos pelo um smart contract que inclui algumas funções para facilitar trocando tokens, adicionando liquidez e mais.  Essencialmente, cada "pool" use o função x*y=k para manter uma curva ao longo da qual as trocas podem acontecer.  Os "pools" monitora as reservas e as atualiza após cada negociação ocorrer.  Porque as reservas são reequilibradas automaticamente, os pools de Shinobi pode sempre ser usado para comprar ou vender um token sem exigir uma contraparte do outro lado da troca.

## Como os preços são determinados?

Os preços são determinados pela quantidade de cada token em um pool. O smart contract mantém uma constante usando a seguinte função: x*y=k.  Nesse caso, x = token0, y = token1, k = constante.  Durante cada troca, uma quantidade de um token é removida do pool por uma quantidade do outro token. Para manter k, os saldos mantidos pelo smart contract são ajustados durante a execução da troca, alterando o preço.

## Como faço para adicionar um logo para um token?

Shinobi, assim como Ubiqscan, puxa do repositório de ativos trustwallet no github bifurcado por Octano. https://github.com/octanolabs/assets Adicione seu ícone de token a esse repositório e ele aparecerá no frontend e no site de informações.

*nota: ao contrário do repositório original, a edição Octano não cobra por acréscimos..*

## Como posso adicionar um token ao Shinobi?

Shinobi é compatível com qualquer token ERC-20 no ecossistema Ubiq.  Se você quer seu projeto seja pesquisável em sua interface, você deve procurar ser adicionado a uma lista de tokens confiável ou compartilhar um link para seu token usando parâmetros de consulta.  Depois de carregado por meio de um link, o token será adicionado à interface.

Outra opção é abrir uma solicitação usando Github Issues.

O time Octano não dá garantias nem fornece qualquer cronograma para tais solicitações. O time nunca cobrará ou solicitará fundos. Adicionamos muitos características de UX para facilitar apresentar um novo token com as comunidades - características como suporte de armazenamento local e links personalizados. Por favor, faça uso deles.
