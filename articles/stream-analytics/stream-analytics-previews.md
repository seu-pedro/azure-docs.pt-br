---
title: Recursos de visualização do Azure Stream Analytics
description: Este artigo lista os recursos do Azure Stream Analytics que estão atualmente na visualização.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 59bb866d7a339608555f0bb802e1716eba5d3255
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431587"
---
# <a name="azure-stream-analytics-preview-features"></a>Recursos de visualização do Azure Stream Analytics

Este artigo resume todos os recursos atualmente em visualização do Azure Stream Analytics. O uso de recursos de visualização em um ambiente de produção não é recomendado.

## <a name="public-previews"></a>Visualizações públicas

Os seguintes recursos estão em pré-visualização pública. Você pode aproveitar esses recursos hoje, mas não os use em seu ambiente de produção.

### <a name="online-scaling"></a>Dimensionamento online

Com o dimensionamento online, não é necessário interromper seu trabalho se você precisar alterar a alocação de SU. Você pode aumentar ou diminuir a capacidade de SU de um trabalho em execução sem precisar interrompê-lo. Isso se baseia na promessa do cliente de pipelines de missão crítica de longa execução que Stream Analytics oferece hoje. Para obter mais informações, consulte [configurar Azure Stream Analytics unidades de streaming](stream-analytics-streaming-unit-consumption.md#configure-stream-analytics-streaming-units-sus).

### <a name="c-custom-de-serializers"></a>C#desserializadores personalizados
Os desenvolvedores podem aproveitar o poder do Azure Stream Analytics para processar dados em Protobuf, XML ou qualquer formato personalizado. Você pode implementar [desserializadores personalizados](custom-deserializer-examples.md) no C#, que podem ser usados para desserializar eventos recebidos por Azure Stream Analytics.

### <a name="extensibility-with-c-custom-code"></a>Extensibilidade com C# código personalizado

Os desenvolvedores que criam Stream Analytics módulos na nuvem ou em IoT Edge podem gravar ou reutilizar funções personalizadas C# e chamá-las diretamente na consulta por meio de [funções definidas pelo usuário](stream-analytics-edge-csharp-udf-methods.md).

### <a name="managed-identity-authentication-with-power-bi"></a>Autenticação de identidade gerenciada com Power BI

O Azure Stream Analytics oferece suporte completo à autenticação baseada em identidade gerenciada com Power BI para uma experiência de painel dinâmico.

### <a name="anomaly-detection"></a>Detecção de Anomalias

Azure Stream Analytics os modelos de aprendizado de máquina têm suporte para detecção de *pico* e *DIPs* , além de detecção bidirecional, positiva lenta e baixa de tendências negativas. Para obter mais informações, visite [detecção de anomalias em Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="debug-query-steps-in-visual-studio"></a>Depurar etapas de consulta no Visual Studio

Você pode visualizar facilmente o conjunto de linhas intermediários em um diagrama de dados ao fazer testes locais no Azure Stream Analytics Tools para Visual Studio. 

### <a name="local-testing-with-live-data-in-visual-studio-code"></a>Teste local com dados dinâmicos no Visual Studio Code

Você pode testar suas consultas em dados dinâmicos em seu computador local antes de enviar o trabalho para o Azure. Cada iteração de teste leva menos de dois a três segundos em média, resultando em um processo de desenvolvimento muito eficiente.

### <a name="visual-studio-code-for-azure-stream-analytics"></a>Visual Studio Code para Azure Stream Analytics

Os trabalhos do Azure Stream Analytics podem ser criados no Visual Studio Code. Veja nosso [tutorial de introdução ao vs Code](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).


### <a name="anomaly-detection"></a>Detecção de Anomalias

O Azure Stream Analytics apresenta novos modelos de aprendizado de máquina com suporte para a detecção de *spike* e *dips*, além da detecção de tendências negativas bidirecionais, lentas positivas e lentas. Para obter mais informações, visite [detecção de anomalias em Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).


### <a name="integration-with-azure-machine-learning"></a>Integração com o Azure Machine Learning

Você pode dimensionar os trabalhos do Stream Analytics com as funções de Aprendizado de Máquina (ML). Para saber mais sobre como usar as funções do ML em seu trabalho do Stream Analytics, visite [Dimensionar seu trabalho do Stream Analytics com as funções do Azure Machine Learning](stream-analytics-scale-with-machine-learning-functions.md). Confira um cenário do mundo real com [Realizando análise de sentimentos usando o Azure Stream Analytics e o Aprendizado de Máquina do Azure](stream-analytics-machine-learning-integration-tutorial.md).


### <a name="live-data-testing-in-visual-studio"></a>Em tempo real de dados de teste no Visual Studio

As ferramentas do Visual Studio para o Azure Stream Analytics aprimoram o recurso de teste local que permite testar suas consultas em fluxos de eventos ao vivo de fontes de nuvem, como Hub de Eventos ou Hub de IoT. Saiba como [Testar dados ao vivo localmente usando as ferramentas do Azure Stream Analytics para o Visual Studio](stream-analytics-live-data-local-testing.md).


### <a name="net-user-defined-functions-on-iot-edge"></a>Funções definidas pelo usuário .NET no IoT Edge

Com as funções definidas pelo usuário padrão do .NET, você pode executar o código do .NET Standard como parte de seu fluxo de pipeline. Você pode criar classes C# simples ou importar projetos e bibliotecas completos. Experiência completa de autoria e depuração é suportada no Visual Studio. Para obter mais informações, visite [Desenvolver funções definidas pelo usuário do .NET Standard para tarefas do Edge Analytics Broadcast do Azure](stream-analytics-edge-csharp-udf-methods.md).

## <a name="other-previews"></a>Outras visualizações

Os recursos a seguir também estão disponíveis em visualização na solicitação.

### <a name="real-time-high-performance-scoring-with-custom-ml-models-managed-by-azure-machine-learning"></a>Pontuação de alto desempenho em tempo real com modelos de ML personalizados gerenciados por Azure Machine Learning

O Azure Stream Analytics dá suporte a pontuação em tempo real de alto desempenho, aproveitando modelos de Machine Learning pré-treinados personalizados gerenciados por Azure Machine Learning e hospedados no AKS (serviço kubernetes do Azure) ou ACI (instâncias de contêiner do Azure), usando um fluxo de trabalho Isso não exige que você escreva código. [Inscrever-se](https://aka.ms/asapreview1) para visualização

### <a name="support-for-azure-stack"></a>Suporte para Azure Stack
Esse recurso habilitado no tempo de execução de Azure IoT Edge, aproveita os recursos de Azure Stack personalizados, como suporte nativo para entradas locais e saídas em execução em Azure Stack (por exemplo, hubs de eventos, Hub IoT, armazenamento de BLOBs). Essa nova integração permite que você crie arquiteturas híbridas que podem analisar seus dados perto de onde são gerados, reduzindo a latência e maximizando as informações.
Esse recurso permite que você crie arquiteturas híbridas que podem analisar seus dados perto de onde são gerados, reduzindo a latência e maximizando as informações. Você deve [se inscrever](https://aka.ms/asapreview1) para esta versão prévia.
