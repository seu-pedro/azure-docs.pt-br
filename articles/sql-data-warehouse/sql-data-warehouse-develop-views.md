---
title: Usando exibições do T-SQL
description: Dicas para usar exibições T-SQL no Azure SQL Data Warehouse para desenvolvimento de soluções.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 14ff990fa21f4af3c77b4dba10b4fea758ff32b6
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685763"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Exibições no Azure SQL Data Warehouse
As exibições podem ser usadas em diversas maneiras diferentes de melhorar a qualidade de sua solução. 

O Azure SQL Data Warehouse dá suporte a exibições padrão e materializadas. Ambas são tabelas virtuais criadas com expressões SELECT e apresentadas a consultas como tabelas lógicas. As exibições encapsulam a complexidade da computação de dados comum e adicionam uma camada de abstração às alterações de computação para que não seja necessário reescrever consultas.

## <a name="standard-view"></a>Exibição padrão
Uma exibição padrão calcula seus dados a cada vez que a exibição é usada. Não há dados armazenados em disco. Normalmente, as pessoas usam modos de exibição padrão como uma ferramenta que ajuda a organizar os objetos lógicos e as consultas em um banco de dados. Para usar um modo de exibição padrão, uma consulta precisa fazer referência direta a ele. Para saber mais, consulte a documentação [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql).

Os modos de exibição no SQL Data Warehouse são armazenados somente como metadados. Consequentemente, as opções a seguir não estão disponíveis:
* Não há opção de associação de esquema
* Tabelas base não podem ser atualizadas por meio da exibição
* Exibições não podem ser criadas em tabelas temporárias
* Não há suporte para as dicas EXPAND / NOEXPAND
* não há exibições indexadas no SQL Data Warehouse

As exibições padrão podem ser utilizadas para impor junções otimizadas de desempenho entre tabelas. Por exemplo, uma exibição pode incorporar uma chave de distribuição redundante como parte dos critérios de junção para minimizar a movimentação dos dados. Outro benefício de uma exibição pode ser forçar uma dica de consulta ou junção específica. Usar exibições dessa maneira garante que as junções sempre sejam executadas de forma ideal, evitando a necessidade dos usuários lembrarem a construção correta de suas junções.

## <a name="materialized-view"></a>Exibição materializada
Uma exibição materializada computa previamente, armazena e mantém seus dados no Azure SQL Data Warehouse assim como uma tabela. Não há necessidade de recomputação toda vez que uma exibição materializada é usada. À medida que os dados são carregados em tabelas base, o Azure SQL Data Warehouse atualiza de forma síncrona as exibições materializadas.  O otimizador de consulta usa automaticamente exibições materializadas implantadas para melhorar o desempenho da consulta, mesmo se as exibições não forem referenciadas na consulta.  As consultas que mais se beneficiam com exibições materializadas são consultas complexas (normalmente consultas com junções e agregações) em tabelas grandes que produzem pequenos conjuntos de resultados.  

Para obter detalhes sobre a sintaxe de exibição materializada e outros requisitos, consulte [criar exibição materializada como SELECT](https://docs.microsoft.com/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest).  

Para obter diretrizes de ajuste de consulta, verifique o [ajuste de desempenho com exibições materializadas](https://docs.microsoft.com/azure/sql-data-warehouse/performance-tuning-materialized-views). 

## <a name="example"></a>Exemplo
Um padrão de aplicativo comum é recriar tabelas usando CREATE TABLE AS SELECT (CTAS) seguido por um objeto de renomeação do padrão durante o carregamento dos dados.  O exemplo a seguir adiciona novos registros de data para uma dimensão de data. Observe como uma nova tabela, DimDate_New, é criada pela primeira vez e renomeada para substituir a versão original da tabela.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```
No entanto, essa abordagem pode resultar em tabelas que aparecem e desaparecem da exibição do usuário, bem como nas mensagens de erro "a tabela não existe". As exibições podem ser usadas para fornecer aos usuários uma camada de apresentação consistente enquanto os objetos subjacentes são renomeados. Fornecendo acesso a dados por meio de exibições, os usuários não precisam de visibilidade para as tabelas subjacentes. Essa camada fornece uma experiência de usuário consistente enquanto garante que os designers do data warehouse podem desenvolver o modelo de dados. Capacidade para desenvolver as tabelas subjacentes significa que os designers podem usar CTAS para maximizar o desempenho durante o processo de carregamento de dados.   

## <a name="next-steps"></a>Próximas etapas
Para obter mais dicas de desenvolvimento, confira [Visão geral sobre o desenvolvimento no SQL Data Warehouse](sql-data-warehouse-overview-develop.md).


