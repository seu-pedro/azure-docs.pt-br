---
title: Detectar o idioma com a API REST de Análise de Texto
titleSuffix: Azure Cognitive Services
description: Detecte o idioma usando a API REST de Análise de Texto dos Serviços Cognitivos do Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: d34f3a03e1bcd35c270d13c4dda57d0394a36e4b
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70387798"
---
# <a name="example-detect-language-with-text-analytics"></a>Exemplo: Detectar o idioma com a Análise de Texto

O recurso [Detecção de Idioma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) da API REST de Análise de Texto do Azure avalia a entrada de texto de cada documento e retorna os identificadores de idioma com uma pontuação que indica a força da análise.

Esse recurso é útil para conteúdo armazena esse texto arbitrário de coleção, onde o idioma é desconhecido. Você pode analisar os resultados dessa análise para determinar qual idioma é usado no documento de entrada. A resposta também retorna uma pontuação que reflete a confiança do modelo. O valor de pontuação está entre 0 e 1.

O recurso Detecção de Idioma pode detectar uma ampla variedade de idiomas, variantes, dialetos e alguns idiomas regionais ou culturais. A lista exata de idiomas para esse recurso não foi publicada.

Caso você tenha um conteúdo expresso em um idioma usado com menos frequência, experimente o recurso Detecção de Idioma para ver se ele retorna um código. A resposta para idiomas que não podem ser detectados é `unknown`.

> [!TIP]
> A Análise de Texto também fornece um Docker baseado em imagem de contêiner do Linux para detecção de idioma, para que você possa [instalar e executar o contêiner de análise de texto](text-analytics-how-to-install-containers.md) perto de seus dados.

## <a name="preparation"></a>Preparação

Você deve ter documentos JSON neste formato: ID e texto.

O tamanho do documento precisa ter menos de 5.120 caracteres por documento. Você pode ter até 1.000 itens (IDs) por coleção. A coleção é enviada no corpo da solicitação. A seguinte amostra é um exemplo de conteúdo que você pode enviar para detecção de idioma:

```json
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },
            {
                "id": "5",
                "text": "Этот документ на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Etapa 1: Estruturar a solicitação

Para obter mais informações sobre a definição de solicitação, confira [Chamar a API de Análise de Texto](text-analytics-how-to-call-api.md). Os seguintes pontos são redeclarados para conveniência:

+ Crie uma solicitação POST. Para examinar a documentação da API para essa solicitação, confira a [API de Detecção de Idioma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7).

+ Defina o ponto de extremidade HTTP para a detecção de idioma. Use um recurso da Análise de Texto no Azure ou uma instância criada de um contêiner da [Análise de Texto](text-analytics-how-to-install-containers.md). Você precisa incluir `/text/analytics/v2.1/languages` na URL. Por exemplo: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/languages`.

+ Defina um cabeçalho de solicitação para incluir a [chave de acesso](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) para operações de Análise de Texto do Azure Machine Learning.

+ No corpo da solicitação, forneça a coleção de documentos JSON preparada para esta análise

> [!Tip]
> Use o [Postman](text-analytics-how-to-call-api.md) ou abra o **console de teste da API** na [documentação](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) para estruturar uma solicitação e POSTAR no serviço.

## <a name="step-2-post-the-request"></a>Etapa 2: Fazer a solicitação POST

A análise é executada após o recebimento da solicitação. Para obter informações sobre o tamanho e o número de solicitações que você pode enviar por minuto e segundo, confira a seção [Limites de dados](../overview.md#data-limits) na visão geral.

Lembre-se de que o serviço é sem estado. Nenhum dado é armazenado em sua conta. Os resultados são retornados imediatamente na resposta.


## <a name="step-3-view-the-results"></a>Etapa 3: Exibir os resultados

Todas as solicitações POST retornam uma resposta formatada em JSON com as IDs e as propriedades detectadas.

A saída é retornada imediatamente. Você pode transmitir os resultados para um aplicativo que aceita JSON ou salvar a saída em um arquivo no sistema local. Em seguida, importe a saída para um aplicativo que você possa usar para classificar, pesquisar e manipular os dados.

Resultados para o exemplo de solicitação devem parecer com o JSON a seguir. Observe que se trata de um documento com vários itens. A saída é em inglês. Identificadores de idioma incluem um nome amigável e um código de idioma no formato [ISO 639-1](https://www.iso.org/standard/22109.html).

Uma pontuação positiva de 1.0 expressa o nível mais alto de confiança possível da análise.

```json
    {
        "documents": [
            {
                "id": "1",
                "detectedLanguages": [
                    {
                        "name": "English",
                        "iso6391Name": "en",
                        "score": 1
                    }
                ]
            },
            {
                "id": "2",
                "detectedLanguages": [
                    {
                        "name": "Spanish",
                        "iso6391Name": "es",
                        "score": 1
                    }
                ]
            },
            {
                "id": "3",
                "detectedLanguages": [
                    {
                        "name": "French",
                        "iso6391Name": "fr",
                        "score": 1
                    }
                ]
            },
            {
                "id": "4",
                "detectedLanguages": [
                    {
                        "name": "Chinese_Simplified",
                        "iso6391Name": "zh_chs",
                        "score": 1
                    }
                ]
            },
            {
                "id": "5",
                "detectedLanguages": [
                    {
                        "name": "Russian",
                        "iso6391Name": "ru",
                        "score": 1
                    }
                ]
            }
        ],
        "errors": []
    }
```

### <a name="ambiguous-content"></a>Conteúdo ambíguo

Em alguns casos, pode ser difícil eliminar a ambiguidade de idiomas com base na entrada. Você pode usar o parâmetro `countryHint` para especificar um código de país de duas letras. Por padrão, a API está usando o "US" como o countryHint padrão, para remover esse comportamento, você pode redefinir esse parâmetro configurando esse valor como uma cadeia de caracteres vazia `countryHint = ""`.

Por exemplo, "Impossible" é comum entre inglês e francês e se for fornecida com contexto limitado, a resposta será baseada na dica de país “US”. Se a origem do texto for conhecida por ser proveniente da França, isso poderá ser fornecido como uma dica.

**Input**

```json
    {
        "documents": [
            {
                "id": "1",
                "text": "impossible"
            },
            {
                "id": "2",
                "text": "impossible",
                "countryHint": "fr"
            }
        ]
    }
```

O serviço agora tem contexto adicional para fazer um melhor julgamento: 

**Saída**

```json
    {
        "documents": [
            {
                "id": "1",
                "detectedLanguages": [
                    {
                        "name": "English",
                        "iso6391Name": "en",
                        "score": 1
                    }
                ]
            },
            {
                "id": "2",
                "detectedLanguages": [
                    {
                        "name": "French",
                        "iso6391Name": "fr",
                        "score": 1
                    }
                ]
            }
        ],
        "errors": []
    }
```

Se o analisador não conseguir analisar a entrada, ele retornará `(Unknown)`. Um exemplo é se você enviar um bloco de texto que consiste apenas em numerais árabes.

```json
    {
        "id": "5",
        "detectedLanguages": [
            {
                "name": "(Unknown)",
                "iso6391Name": "(Unknown)",
                "score": "NaN"
            }
        ]
    }
```

### <a name="mixed-language-content"></a>Conteúdo de idioma misto

O conteúdo de idioma misto no mesmo documento retorna o idioma com a representação maior no conteúdo, mas com uma classificação positiva inferior. A classificação reflete a força marginal da avaliação. No exemplo a seguir, a entrada é uma combinação de francês, espanhol e inglês. O analisador de contagens de caracteres em cada segmento determina o idioma predominante.

**Input**

```json
    {
      "documents": [
        {
          "id": "1",
          "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
        }
      ]
    }
```

**Saída**

A saída resultante consiste no idioma predominante, com uma pontuação inferior a 1,0, o que indica um nível mais fraco de confiança.

```json
    {
      "documents": [
        {
          "id": "1",
          "detectedLanguages": [
            {
              "name": "Spanish",
              "iso6391Name": "es",
              "score": 0.9375
            }
          ]
        }
      ],
      "errors": []
    }
```

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu os conceitos e o fluxo de trabalho para detecção de idioma usando a Análise de Texto nos Serviços Cognitivos do Azure. Os seguintes pontos foram explicados e demonstrados:

+ A [Detecção de Idioma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) está disponível para uma ampla variedade de idiomas, variantes, dialetos e alguns idiomas regionais ou culturais.
+ Documentos JSON no corpo da solicitação incluem uma ID e texto.
+ A solicitação POST é feita para um ponto de extremidade `/languages` usando uma [chave de acesso personalizada e um ponto de extremidade](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) que é válido para sua assinatura.
+ A saída de resposta consiste em identificadores de idioma para cada ID de documento. A saída pode ser transmitida para qualquer aplicativo que aceite JSON. Os aplicativos de exemplo incluem Excel e Power BI, para citar alguns.

## <a name="see-also"></a>Consulte também

 [Visão geral da Análise de Texto](../overview.md) [Perguntas frequentes](../text-analytics-resource-faq.md)</br>
 [Página de produto de Análise de Texto do Azure Machine Learning](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Analisar sentimento](text-analytics-how-to-sentiment-analysis.md)
