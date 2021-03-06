---
title: Cláusula de limite de deslocamento no Azure Cosmos DB
description: Saiba como usar a cláusula de limite de deslocamento para ignorar e pegar alguns valores ao consultar em Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: a8df220be211c3c8d8cdeab8a8aebfd35e77ebf8
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732579"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>Cláusula de limite de deslocamento no Azure Cosmos DB

A cláusula de limite de deslocamento é uma cláusula opcional para ignorar e, em seguida, pegar um número de valores da consulta. A contagem de deslocamento e a contagem de limites são necessárias na cláusula de limite de deslocamento.

Quando o limite de deslocamento é usado em conjunto com uma cláusula ORDER BY, o conjunto de resultados é produzido ao fazer a ação de ignorar e assumir os valores ordenados. Se nenhuma cláusula ORDER BY for usada, ela resultará em uma ordem determinística de valores.

## <a name="syntax"></a>Sintaxe
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Argumentos

- `<offset_amount>`

   Especifica o número inteiro de itens que os resultados da consulta devem ignorar.

- `<limit_amount>`
  
   Especifica o número inteiro de itens que os resultados da consulta devem incluir

## <a name="remarks"></a>Comentários
  
  A contagem de deslocamento e a contagem de limites são necessárias na cláusula de limite de deslocamento. Se uma cláusula opcional `ORDER BY` for usada, o conjunto de resultados será produzido fazendo o ignorar os valores ordenados. Caso contrário, a consulta retornará uma ordem fixa de valores. Essa cláusula agora tem suporte para consultas em uma única partição, bem como consultas entre partições.

## <a name="examples"></a>Exemplos

Por exemplo, aqui está uma consulta que ignora o primeiro valor e retorna o segundo valor (na ordem do nome da cidade residente):

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Os resultados são:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Aqui está uma consulta que ignora o primeiro valor e retorna o segundo valor (sem ordenação):

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Os resultados são:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Próximos passos

- [Guia de Introdução](sql-query-getting-started.md)
- [Cláusula SELECT](sql-query-select.md)
- [Cláusula ORDER BY](sql-query-order-by.md)
