---
title: Suporte de Armazenamento de Tabelas do Azure no Azure Cosmos DB
description: Saiba como as Tabelas de Armazenamento do Azure e a API de Tabela do Azure Cosmos DB funcionam juntas compartilhando o mesmo modelo de dados de tabela em operações
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 12/02/2019
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: 4f742ce34ec429f0fc822562c1ad4ad07f3e7965
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74870421"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Desenvolver com a API de Tabela do Azure Cosmos DB e o Armazenamento de Tabelas do Azure

A API de Tabela do Azure Cosmos DB e o Armazenamento de Tabelas do Azure compartilham o mesmo modelo de dados de tabela e expõem as mesmas operações de criar, excluir, atualizar e consultar por meio de seus SDKs.

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Desenvolver com a API de Tabela do Azure Cosmos DB

Nesse momento, a [API de Tabelas do Azure Cosmos DB](table-introduction.md) tem quatro SDKs disponíveis para desenvolvimento: 

* [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table): SDK do .NET. Essa biblioteca é direcionada ao .NET Standard e tem as mesmas classes e assinaturas de método do [SDK do Armazenamento do Microsoft Azure](https://www.nuget.org/packages/WindowsAzure.Storage) público, mas também tem a capacidade de se conectar às contas do Azure Cosmos DB usando a API de Tabela. É recomendado que os usuários da biblioteca [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table/) do .NET Framework façam upgrade para [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table), pois ela está no modo de manutenção e será descontinuada em breve.

* [SDK do Python](table-sdk-python.md): O novo SDK em Python do Azure Cosmos DB é o único SDK que dá suporte ao Armazenamento de Tabelas do Azure em Python. Esse SDK conecta com o armazenamento de Tabelas do Azure e com a API de Tabelas do Azure Cosmos DB.

* [SDK do Java](table-sdk-java.md): Esse SDK do Armazenamento do Azure pode conectar a contas do Azure Cosmos DB com a API de Tabelas.

* [SDK do Node.js](table-sdk-nodejs.md): Esse SDK do Armazenamento do Azure pode conectar a contas do Azure Cosmos DB com a API de Tabelas.


Mais informações sobre como trabalhar com a API de Tabela estão disponíveis no artigo [Perguntas frequentes: Desenvolver com a API de Tabela](faq.md#table).

## <a name="developing-with-azure-table-storage"></a>Desenvolvendo com o armazenamento de Tabelas do Azure

O armazenamento de Tabelas do Azure possui os seguintes SDKs disponíveis para desenvolvimento:

- [SDK do WindowsAzure.Storage .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). Essa biblioteca permite que você trabalhe com o serviço Tabela de armazenamento.
- [SDK do Python](https://github.com/Azure/azure-cosmos-table-python). O SDK de Tabela do Azure Cosmos DB para Python dá suporte ao Serviço de Armazenamento de Tabelas (porque o Armazenamento de Tabelas do Azure e a API de Tabela do Cosmos DB compartilham os mesmos recursos e funcionalidades e, em um esforço para fatorar nossos esforços de desenvolvimento de SDK, recomendamos o uso desse SDK).
- [SDK do Armazenamento do Azure para Java](https://github.com/azure/azure-storage-java). Esse SDK de Armazenamento do Azure fornece uma biblioteca de clientes em Java para consumir o armazenamento de Tabela do Azure.
- [SDK do Node.js](https://github.com/Azure/azure-storage-node). Esse SDK fornece um pacote de Node.js e uma biblioteca de clientes compatível com o navegador JavaScript para consumir o serviço de Tabela de armazenamento.
- [Módulo do PowerShell do AzureRmStorageTable](https://www.powershellgallery.com/packages/AzureRmStorageTable). Este módulo do PowerShell apresenta cmdlets para trabalhar com Tabelas de armazenamento.
- [Biblioteca de Clientee de Armazenamento do Azure](https://github.com/Azure/azure-storage-cpp/). Essa biblioteca permite que você crie aplicativos no Armazenamento do Azure.
- [Biblioteca de Cliente da Tabela de Armazenamento do Azure para Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Este projeto fornece um pacote Ruby que facilita o acesso aos serviços de Tabela de armazenamento do Azure.
- [Biblioteca de Clientes PHP da Tabela de Armazenamento do Azure](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Este projeto fornece uma biblioteca de clientes PHP que facilita o acesso aos serviços de Tabela de armazenamento do Azure.


   





