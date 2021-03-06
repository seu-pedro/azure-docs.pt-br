---
title: 'Início Rápido: Criar um aplicativo Web usando a API do Azure Cosmos DB para Mongo DB e o SDK do Java'
description: Aprenda a criar um exemplo de código Java que você pode usar para se conectar e consultar usando a API para MongoDB do Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: java
ms.topic: quickstart
ms.date: 12/26/2018
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 05a796e5bf197bf9ea4f8f47adfbf30851b300ca
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445499"
---
# <a name="quickstart-create-a-console-app-with-java-and-the-mongodb-api-in-azure-cosmos-db"></a>Início Rápido: Criar um aplicativo de console com Java e a API do MongoDB no Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Neste início rápido, você usará a API do Azure Cosmos DB para Mongo DB e o SDK Java para criar um aplicativo Web de console. O Azure Cosmos DB permite criar e consultar rapidamente documentos, pares chave-valor e bancos de dados do grafo. Todos se beneficiam de recursos de escala horizontal e distribuição global no núcleo do Cosmos DB.

Este início rápido demonstra como criar uma conta do Cosmos com a [API do Azure Cosmos DB para MongoDB](mongodb-introduction.md). Você compilará e implantará um aplicativo de console compilado usando o [driver MongoDB Java](https://docs.mongodb.com/ecosystem/drivers/java/). 

## <a name="prerequisites"></a>Prerequisites

Antes que possa executar esta amostra, você deverá ter os seguintes pré-requisitos:
* [Instalar o JDK do Azure e o JDK do Azure Stack versão 8](https://aka.ms/azure-jdks)
* Maven (execute `apt-get install maven` se você não tiver o Maven)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

## <a name="create-a-database-account"></a>Criar uma conta de banco de dados

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a>Adicionar uma coleção

Nomeie o novo banco de dados, **db** e sua nova coleção, **coll**.

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)] 

## <a name="clone-the-sample-application"></a>Clonar o aplicativo de exemplo

Agora, clonaremos um aplicativo do GitHub, definiremos a cadeia de conexão e o executaremos. Você verá como é fácil trabalhar usando dados de forma programática. 

1. Abra um prompt de comando, crie uma nova pasta chamada exemplos de git e feche o prompt de comando.

    ```bash
    md "C:\git-samples"
    ```

2. Abra uma janela de terminal de git, como git bash, e use o comando `cd` para alterar para a nova pasta para instalar o aplicativo de exemplo.

    ```bash
    cd "C:\git-samples"
    ```

3. Execute o comando a seguir para clonar o repositório de exemplo. Este comando cria uma cópia do aplicativo de exemplo no seu computador.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. Em seguida, abra o código em seu editor favorito. 

## <a name="review-the-code"></a>Examine o código

Esta etapa é opcional. Se você estiver interessado em aprender como os recursos de banco de dados são criados no código, poderá examinar os snippets de código a seguir. Caso contrário, você poderá pular para [Atualizar sua cadeia de conexão](#update-your-connection-string). 

Todos os snippets de código a seguir são retirados do arquivo Program.java.

* O DocumentClient é inicializado.

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* Um novo banco de dados e uma nova coleção são criados.

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* Alguns documentos são inseridos usando `MongoCollection.insertOne`

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* Algumas consultas são executadas usando `MongoCollection.find`

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a>Atualizar sua cadeia de conexão

Agora, volte ao portal do Azure para obter informações sobre a cadeia de conexão e copiá-las para o aplicativo.

1. Da conta, selecione **Início Rápido**, selecione **Java** e, em seguida, copie a cadeia de conexão para a área de transferência.

2. Abra o arquivo `Program.java`, substitua o argumento para o construtor MongoClientURI pela cadeia de conexão. Agora, você atualizou o aplicativo com todas as informações necessárias para se comunicar com o Azure Cosmos DB. 
    
## <a name="run-the-console-app"></a>Execute o aplicativo de console

1. Executar `mvn package` em um terminal para instalar os módulos npm necessários

2. Execute `mvn exec:java -D exec.mainClass=GetStarted.Program` em um terminal para iniciar o aplicativo Java.

Agora você pode usar [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) para consultar, modificar e trabalhar com esses novos dados.

## <a name="review-slas-in-the-azure-portal"></a>Examinar SLAs no Portal do Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpar os recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você aprendeu como criar uma conta do Cosmos, criar uma coleção e executar um aplicativo de console. Agora você pode importar dados adicionais para o banco de dados Cosmos.

> [!div class="nextstepaction"]
> [Importar dados do MongoDB no Azure Cosmos DB](mongodb-migrate.md)
