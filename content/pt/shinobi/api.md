---
title: API
description: Esta seção explica o subgráfico Shinobi e como interagir com ele.
createdAt:
---

<v-alert color="secondary" outlined text>Para simplificar a integração com os serviços e bases de código existentes, a API Shinobi imita o Uniswap V2 API 1: 1, os nomes das áreas podem incluir 'ETH' ou 'Uniswap', com o objetivo de economizar seu tempo.</v-alert>

## Sinopse

O subgráfico de Shinobi indexa dados dos contratos de Shinobi ao longo do tempo. Ele organiza dados sobre os pares, tokens, a totalidade do Shinobi e mais. O subgráfico é atualizado sempre que uma transação é feita no Shinobi.

### Recursos

* [Subgráfico de Shinobi](https://github.com/octanolabs/shinobi-subgraph) - código-fonte para o subgráfico dispensado

### Uso

O subgráfico do Shinobi corresponde ao 1:1 do Uniswap V2, se você já tem código que usa o subgráfico de Uniswap, pode simplesmente apontar seu cliente Apollo para o endpoint Shinobi hospedado por Octano: https://graphnode.octano.dev/subgraphs/name/octanolabs/shinobiraphs

O subgráfico fornece um apanhado do estado atual de Shinobi e também rastreia dados históricos. Atualmente é usado para alimentar https://shinobi-info.ubiq.ninja. Não se destina a ser usado como uma fonte de dados para estruturar transações (os contratos devem ser referenciados diretamente para os dados ativos mais confiáveis).

### Fazendo Consultas

To learn more about querying a subgraph refer to [The Graph’s documentation](https://thegraph.com/docs/introduction).

Para saber mais sobre como consultar um subgráfico, consulte [a documentação de The Graph] (https://thegraph.com/docs/introduction).

## Entidades

As entidades definem o esquema do subgráfico e representam os dados que podem ser consultados. Dentro de cada entidade existem conjuntos de áreas que armazenam informações úteis relacionadas à entidade. Abaixo está uma lista das entidades disponíveis no subgráfico Uniswap e descrições para as áreas disponíveis.

Para ver uma "sandbox" interativa de todas as entidades, consulte o [Graph Explorer] (https://thegraph.com/explorer/subgraph/uniswap/uniswap-v2).

Cada entidade é definida com um tipo de valor, que sempre será um AssemblyScript tipo base ou um tipo personalizado fornecido pela biblioteca TypeScript personalizada do Graph. Para obter mais informações sobre os tipos de valor, consulte [aqui] (https://thegraph.com/docs/assemblyscript-api#api-reference).

### Fábrica

A entidade `UniswapFactory` é responsável por armazenar informações agregadas em todos os pares de Shinobi. Ele pode ser usado para visualizar estatísticas sobre liquidez total, o volume, quantidade de pares e mais. Existe apenas uma entidade `UniswapFactory` no subgráfico.

| Nome da Área      | Tipo de Valor | Descrição                                                                     |
| ----------------- | ------------- | ----------------------------------------------------------------------------- |
| id                | ID            | o endereço da fábrica                                                         |
| pairCount         | Int           | quantidade de pares criados pela fábrica Shinobi                              |
| totalVolumeUSD    | BigDecimal    | o volume de USD em todos os pares de todos os tempos (USD é derivado)         |
| totalVolumeETH    | BigDecimal    | o volume de UBQ em todos os pares de todos os tempos (UBQ é derivado)         |
| totalLiquidityUSD | BigDecimal    | liquidez total em todos os pares armazenados como um montante derivado em USD |
| totalLiquidityETH | BigDecimal    | liquidez total em todos os pares armazenados como um montante derivado em UBQ |
| txCount           | BigInt        | o quantidade de transações em todos os pares do todos tempos                  |

### Token

Armazena informações agregadas para um token específico em todos os pares em que o token está incluído.

| Nome da Área       | Tipo de Valor | Descrição                                                                                                    |
| ------------------ | ------------- | ------------------------------------------------------------------------------------------------------------ |
| id                 | ID            | endereço do token                                                                                            |
| symbol             | String        | símbolo do token                                                                                             |
| name               | String        | nome do token                                                                                                |
| decimals           | BigInt        | quantidade de decimais do token                                                                              |
| tradeVolume        | BigDecimal    | quantidade de token trocado em todos os pares do todos tempos                                                |
| tradeVolumeUSD     | BigDecimal    | quantidade de tokens em USD trocado do todos tempos incluindo todos os pares (só tokens com liquidez acima do limite mínimo) |
| untrackedVolumeUSD | BigDecimal    | quantidade de tokens em USD trocado do todos tempos incluindo todos os pares (sem limite mínimo de liquidez) |
| txCount            | BigInt        | quantidade de transações de todos os tempos em pares que incluem o token                                     |
| totalLiquidity     | BigDecimal    | quantidade total do token fornecido como liquidez entre todos os pares                                       |
| derivedETH         | BigDecimal    | UBQ por token                                                                                                |

### Par

Informações sobre um par. Inclui referências a cada token dentro do par, informações de volume, informações de liquidez e mais. A entidade do par espelha o smart contract do par e também contém informações agregadas sobre o uso.

| Nome da Área         | Tipo de Valor       | Descrição                                                                                                         |
| -------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| id                   | ID                  | endereço de contrato do par                                                                                         |
| factory              | UniswapFactory      | referência à entidade de fábrica Shinobi                                                                             |
| token0               | Token               | referência ao token0 como armazenado no contrato de par                                                           |
| token1               | Token               | referência ao token1 como armazenado no contrato de par                                                           |
| reserve0             | BigDecimal          | reserva de token0                                                                                                   |
| reserve1             | BigDecimal          | reserva de token1                                                                                                   |
| totalSupply          | BigDecimal          | quantidade total de token de liquidez distribuído aos provedores de liquidez                                      |
| reserveETH           | BigDecimal          | total liquidity in pair stored as an amount of UBQ                                                                  |
| reserveUSD           | BigDecimal          | total liquidity amount in pair stored as an amount of USD                                                           |
| trackedReserveETH    | BigDecimal          | total liquidity with only tracked amount (see tracked amounts)                                                      |
| token0Price          | BigDecimal          | token0 per token1                                                                                                   |
| token1Price          | BigDecimal          | token1 per token0                                                                                                   |
| volumeToken0         | BigDecimal          | amount of token0 swapped on this pair                                                                               |
| volumeToken1         | BigDecimal          | amount of token1 swapped on this pair                                                                               |
| volumeUSD            | BigDecimal          | total amount swapped all time in this pair stored in USD (only tracked if USD liquidity is above minimum threshold) |
| untrackedVolumeUSD   | BigDecimal          | total amount swapped all time in this pair stored in USD, no minimum liquidity threshold                            |
| txCount              | BigInt              | all time amount of transactions on this pair                                                                        |
| createdAtTimestamp   | BigInt              | timestamp contract was created                                                                                      |
| createdAtBlockNumber | BigInt              | Ubiq block contract was created                                                                                 |
| liquidityPositions   | [LiquidityPosition] | array of liquidity providers, used as a reference to LP entities                                                    |

### User

A user entity is created for any address that provides liquidity to a pool on Shinobi. This entity
can be used to track open positions for users. LiquidyPosition entities can be referenced to get
specific data about each position.

| Field Name         | Value Type          | Description                                    |
| ------------------ | ------------------- | ---------------------------------------------- |
| id                 | ID                  | user address                                   |
| liquidityPositions | [LiquidityPosition] | array of all liquidity positions user has open |
| usdSwapped         | BigDecimal          | total USD value swapped                        |

### LiquidityPositiion

This entity is used to store data about a user's liquidity position. This information, along with
information from the pair itself can be used to provide position sizes, token deposits, and more.

| Field Name            | Value Type | Description                                               |
| --------------------- | ---------- | ----------------------------------------------------------|
| id                    | ID         | user address and pair address concatenated with a dash    |
| user                  | User       | reference to user                                         |
| pair                  | Pair       | reference to the pair liquidity is being provided on      |
| liquidityTokenBalance | BigDecimal | amount of LP tokens minted for this position              |

### Transaction

Transaction entities are created for each Ubiq transaction that contains an interaction within Shinobi contracts. This subgraph tracks Mint, Burn, and Swap events on the Shinobi core contracts. Each transaction contains 3 arrays, and at least one of these arrays has a length of 1.

| Field Name  | Value Type | Description                                               |
| ----------- | ---------- | --------------------------------------------------------- |
| id          | ID         | Ubiq transaction hash                                 |
| blockNumber | BigInt     | block transaction was mined in                            |
| timestamp   | BigInt     | timestamp for transaction                                 |
| mints       | [Mint]     | array of Mint events within the transaction, 0 or greater |
| burns       | [Burn]     | array of Burn events within transaction, 0 or greater     |
| swaps       | [Swap]     | array of Swap events within transaction, 0 or greater     |

### Mint

Mint entities are created for every emitted Mint event on the Shinobi core contracts. The Mint entity stores key data about the event like token amounts, who sent the transaction, who received the liquidity, and more. This entity can be used to track liquidity provisions on pairs.

| Field Name   | Value Type  | Description                                                 |
| ------------ | ----------- | ----------------------------------------------------------- |
| id           | ID          | Transaction hash plus index in the transaction mint array   |
| transaction  | Transaction | reference to the transaction Mint was included in           |
| timestamp    | BigInt      | timestamp of Mint, used to sort recent liquidity provisions |
| pair         | Pair        | reference to pair                                           |
| to           | Bytes       | recipient of liquidity tokens                               |
| liquidity    | BigDecimal  | amount of liquidity tokens minted                           |
| sender       | Bytes       | address that initiated the liquidity provision              |
| amount0      | BigDecimal  | amount of token0 provided                                   |
| amount1      | BigDecimal  | amount of token1 provided                                   |
| logIndex     | BigInt      | index in the transaction event was emitted                  |
| amountUSD    | BigDecimal  | derived USD value of token0 amount plus token1 amount       |
| feeTo        | Bytes       | address of fee recipient (if fee is on)                     |
| feeLiquidity | BigDecimal  | amount of liquidity sent to fee recipient (if fee is on)    |

### Burn

Burn entities are created for every emitted Burn event on the Shinobi core contracts. The Burn entity stores key data about the event like token amounts, who burned LP tokens, who received tokens, and more. This entity can be used to track liquidity removals on pairs.

| Field Name   | Value Type  | Description                                               |
| ------------ | ----------- | --------------------------------------------------------- |
| id           | ID          | Transaction hash plus index in the transaction burn array |
| transaction  | Transaction | reference to the transaction Burn was included in         |
| timestamp    | BigInt      | timestamp of Burn, used to sort recent liquidity removals |
| pair         | Pair        | reference to pair                                         |
| to           | Bytes       | recipient of tokens                                       |
| liquidity    | BigDecimal  | amount of liquidity tokens burned                         |
| sender       | Bytes       | address that initiated the liquidity removal              |
| amount0      | BigDecimal  | amount of token0 removed                                  |
| amount1      | BigDecimal  | amount of token1 removed                                  |
| logIndex     | BigInt      | index in the transaction event was emitted                |
| amountUSD    | BigDecimal  | derived USD value of token0 amount plus token1 amount     |
| feeTo        | Bytes       | address of fee recipient (if fee is on)                   |
| feeLiquidity | BigDecimal  | amount of tokens sent to fee recipient (if fee is on)     |

### Swap

Swap entities are created for each token swap within a pair. The Swap entity can be used to get things like swap size (in tokens and USD), sender, recipient and more. See the Swap overview page
for more information on amounts.

| Field Name  | Value Type  | Description                                           |
| ----------- | ----------- | ----------------------------------------------------- |
| id          | ID          | transaction hash plus index in Transaction swap array |
| transaction | Transaction | reference to transaction swap was included in         |
| timestamp   | BigInt      | timestamp of swap, used for sorted lookups            |
| pair        | Pair        | reference to pair                                     |
| sender      | Bytes       | address that initiated the swap                       |
| amount0In   | BigDecimal  | amount of token0 sold                                 |
| amount1In   | BigDecimal  | amount of token1 sold                                 |
| amount0Out  | BigDecimal  | amount of token0 received                             |
| amount1Out  | BigDecimal  | amount of token1 received                             |
| to          | Bytes       | recipient of output tokens                            |
| logIndex    | BigInt      | event index within transaction                        |
| amountUSD   | BigDecimal  | derived amount of tokens sold in USD                  |

### Bundle

The Bundle is used as a global store of derived UBQ price in USD. Because there is
no guaranteed common base token across pairs, a global reference of USD price is useful
for deriving other USD values. The Bundle entity stores an updated price retreived from the Octano price oracle (populated using coingecko's API)

| Field Name | Value Type | Description                                           |
| ---------- | ---------- | ----------------------------------------------------- |
| id         | ID         | constant 1                                            |
| ethPrice   | BigDecimal | derived price of UBQ in USD                           |

## Historical Entities

The subgraph tracks aggregated information grouped by days to provide insights to daily activity on Shinobi. While [time travel queries](https://blocklytics.org/blog/ethereum-blocks-subgraph-made-for-time-travel/) can be used for direct comparison against values in the past, it is much more expensive to query grouped data. For this reason the subgraph tracks information grouped in daily buckets, using timestamps provided by contract events. These entities can be used to query things like total volume on a given day, price of a token on a given day, etc.

For each DayData type, a new entity is created each day.

### UniswapDayData

Tracks data across all pairs aggregated into a daily bucket.

| Field Name        | Value Type      | Description                                                                      |
| ----------------- | --------------- | -------------------------------------------------------------------------------- |
| id                | ID              | unix timestamp for start of day / 86400 giving a unique day index                |
| date              | Int             | unix timestamp for start of day                                                  |
| dailyVolumeETH    | BigDecimal      | total volume across all pairs on this day, stored as a derived amount of UBQ     |
| dailyVolumeUSD    | BigDecimal      | total volume across all pairs on this day, stored as a derived amount of USD     |
| totalVolumeETH    | BigDecimal      | all time volume across all pairs in UBQ up to and including this day             |
| totalLiquidityETH | BigDecimal      | total liquidity across all pairs in UBQ up to and including this day             |
| totalVolumeUSD    | BigDecimal      | all time volume across all pairs in USD up to and including this day             |
| totalLiquidityUSD | BigDecimal      | total liquidity across all pairs in USD up to and including this day             |
| maxStored         | Int             | reference used to store most liquid tokens, used for historical liquidity charts |
| mostLiquidTokens  | [TokenDayData!] | tokens with most liquidity in Shinobi                                            |
| txCount           | BigInt          | number of transactions throughout this day                                       |

### Pair Day Data

Tracks pair data across each day.

| Field Name        | Value Type | Description                                                                                      |
| ----------------- | ---------- | ------------------------------------------------------------------------------------------------ |
| id                | ID         | pair contract address and day id (day start timestamp in unix / 86400) concatenated with a dash  |
| date              | Int        | unix timestamp for start of day                                                                  |
| pairAddress       | Bytes      | address for pair contract                                                                        |
| token0            | Token      | reference to token0                                                                              |
| token1            | Token      | reference to token1                                                                              |
| reserve0          | BigDecimal | reserve of token0 (updated during each transaction on pair)                                      |
| reserve1          | BigDecimal | reserve of token1 (updated during each transaction on pair)                                      |
| totalSupply       | BigDecimal | total supply of liquidity token distributed to LPs                                               |
| reserveUSD        | BigDecimal | reserve of token0 plus token1 stored as a derived USD amount                                     |
| dailyVolumeToken0 | BigDecimal | total amount of token0 swapped throughout day                                                    |
| dailyVolumeToken1 | BigDecimal | total amount of token1 swapped throughout day                                                    |
| dailyVolumeUSD    | BigDecimal | total volume within pair throughout day                                                          |
| dailyTxns         | BigInt     | amount of transactions on pair throughout day                                                    |

### TokenDayData

Tracks token data aggregated across all pairs that include token.

| Field Name          | Value Type    | Descrição                                                                                                                            |
| ------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| id                  | ID            | token address and day id (day start timestamp in unix / 86400) concatenated with a dash                                                         |
| date                | Int           | unix timestamp for start of day                                                                                                        |
| token               | Token         | reference to token entity                                                                                                              |
| dailyVolumeToken    | BigDecimal    | amount of token swapped across all pairs throughout day                                                                                |
| dailyVolumeETH      | BigDecimal    | amount of token swapped across all pairs throughout day stored as a derived amount of UBQ                                              |
| dailyVolumeUSD      | BigDecimal    | amount of token swapped across all pairs throughout day stored as a derived amount of USD                                              |
| dailyTxns           | BigInt        | amount of transactions with this token across all pairs                                                                                |
| totalLiquidityToken | BigDecimal    | token amount of token deposited across all pairs                                                                                       |
| totalLiquidityETH   | BigDecimal    | token amount of token deposited across all pairs stored as amount of UBQ                                                               |
| totalLiquidityUSD   | BigDecimal    | token amount of token deposited across all pairs stored as amount of USD                                                               |
| priceUSD            | BigDecimal    | price of token in derived USD                                                                                                          |
| maxStored           | Int           | amount of token deposited in pair with highest token liquidity - used only as a reference for storing most liquid pairs for this token |
| mostLiquidPairs     | [PairDayData] | pairs with most liquidity for this token                                                                                               |
