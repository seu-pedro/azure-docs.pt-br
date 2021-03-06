---
title: 'Início Rápido: Treinar com rótulos usando a API REST e o Python – Reconhecimento de Formulários'
titleSuffix: Azure Cognitive Services
description: Saiba como usar o recurso de dados rotulados do Reconhecimento de Formulários com a API REST e o Python para treinar um modelo personalizado.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 10/16/2019
ms.author: pafarley
ms.openlocfilehash: 9b9600c1ccb9fb9a87d4c98e4e9042ed71029057
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450926"
---
# <a name="train-a-form-recognizer-model-with-labels-using-rest-api-and-python"></a>Treinar um modelo do Reconhecimento de Formulários com rótulos usando a API REST e o Python

Neste início rápido, você usará a API REST do Reconhecimento de Formulários com o Python para treinar um modelo personalizado usando dados rotulados manualmente. Confira a seção [Treinar com rótulos](../overview.md#train-with-labels) da visão geral para saber mais sobre esse recurso.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Prerequisites

Para concluir este início rápido, é necessário ter:
- Acesso à versão prévia de acesso limitado do Reconhecimento de Formulários. Para obter acesso à versão prévia, preencha e envie o formulário de [Solicitação de acesso ao Reconhecimento de Formulários](https://aka.ms/FormRecognizerRequestAccess).
- [Python](https://www.python.org/downloads/) instalado (se quiser executar o exemplo localmente).
- Um conjunto com pelo menos seis formulários do mesmo tipo. Você usará esses dados para treinar o modelo e testar um formulário. Você pode usar um [conjunto de dados de exemplo](https://go.microsoft.com/fwlink/?linkid=2090451) para este início rápido. Faça upload dos arquivos de treinamento na raiz de um contêiner de Armazenamento de Blobs em uma conta do Armazenamento do Azure.

## <a name="set-up-training-data"></a>Configurar dados de treinamento

Primeiro, você precisará configurar os dados de entrada necessários. O recurso de dados rotulados tem requisitos de entrada especiais além daqueles necessários para treinar um modelo personalizado. 

Verifique se todos os documentos de treinamento têm o mesmo formato. Se você tiver formulários em vários formatos, organize-os em subpastas com base no formato comum. Ao treinar, você precisará direcionar a API para uma subpasta.

Para treinar um modelo usando dados rotulados, você precisará dos arquivos a seguir como entradas na subpasta. Você aprenderá a criar esses arquivos abaixo.

* **Formulários de origem** – os formulários dos quais extrair dados. Os tipos compatíveis são JPEG, PNG, BMP, PDF e TIFF.
* **Arquivos de layout de OCR** – arquivos JSON que descrevem os tamanhos e as posições de todo o texto legível em cada formulário de origem. Você usará a API de layout do Reconhecimento de Formulários para gerar esses dados. 
* **Arquivos de rótulo** – arquivos JSON que descrevem os rótulos de dados que um usuário inseriu manualmente.

Todos esses arquivos devem ocupar a mesma subpasta e estar no seguinte formato:

* input_file1.pdf 
* input_file1.pdf.ocr.json
* input_file1.pdf.labels.json 
* input_file2.pdf 
* input_file2.pdf.ocr.json
* input_file2.pdf.labels.json
* ...

> [!TIP]
> Quando você rotula formulários usando a [ferramenta de rotulação de exemplo](./label-tool.md) do Reconhecimento de Formulários, a ferramenta cria esses arquivos de layout de OCR e de rótulo automaticamente.

### <a name="create-the-ocr-output-files"></a>Criar os arquivos de saída de OCR

Você precisa de arquivos de resultado de OCR para que o serviço considere os arquivos de entrada correspondentes para o treinamento rotulado. Para obter os resultados de OCR para um determinado formulário de origem, siga as etapas abaixo:

1. Chame a API **[Analisar layout](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeLayoutAsync)** no contêiner de layout de leitura com o arquivo de entrada como parte do corpo da solicitação. Salve a ID encontrada no cabeçalho de **Operação-Localização** da resposta.
1. Chame a API **[Obter resultado da análise de layout](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/GetAnalyzeLayoutResult)** , usando a ID da operação da etapa anterior.
1. Obtenha a resposta e grave o conteúdo em um arquivo. Para cada formulário de origem, o arquivo de OCR correspondente deve ter o nome de arquivo original anexado com `.ocr.json`. A saída JSON do OCR deve ter o formato a seguir. Confira o [arquivo OCR de exemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.ocr.json) para obter um exemplo completo. 

    ```json
    {
    "status": "succeeded",
    "createdDateTime": "2019-11-12T21:18:12Z",
    "lastUpdatedDateTime": "2019-11-12T21:18:17Z",
    "analyzeResult": {
        "version": "2.0.0",
        "readResults": [
            {
                "page": 1,
                "language": "en",
                "angle": 0,
                "width": 8.5,
                "height": 11,
                "unit": "inch",
                "lines": [
                    {
                        "language": "en",
                        "boundingBox": [
                            0.5384,
                            1.1583,
                            1.4466,
                            1.1583,
                            1.4466,
                            1.3534,
                            0.5384,
                            1.3534
                        ],
                        "text": "Contoso",
                        "words": [
                            {
                                "boundingBox": [
                                    0.5384,
                                    1.1583,
                                    1.4466,
                                    1.1583,
                                    1.4466,
                                    1.3534,
                                    0.5384,
                                    1.3534
                                ],
                                "text": "Contoso",
                                "confidence": 1
                            }
                        ]
                    },
                    ...
    ```

### <a name="create-the-label-files"></a>Criar os arquivos de rótulo

Os arquivos de rótulo contêm associações de chave-valor que um usuário inseriu manualmente. Eles são necessários para o treinamento de dados rotulados, mas nem todos os arquivos de origem precisam ter um arquivo de rótulo correspondente. Os arquivos de origem sem rótulos serão tratados como documentos de treinamento comuns. Recomendamos cinco ou mais arquivos rotulados para um treinamento confiável.

Ao criar um arquivo de rótulo, você pode opcionalmente especificar regiões, que são posições exatas dos valores no documento. Isso concederá uma precisão ainda maior ao treinamento. As regiões são formatadas como um conjunto de oito valores correspondentes a quatro coordenadas X,Y: superior esquerdo, superior direito, inferior direito e inferior esquerdo. Os valores de coordenadas estão entre zero e um, dimensionados para as dimensões da página.

Para cada formulário de origem, o arquivo de rótulo correspondente deve ter o nome de arquivo original anexado com `.labels.json`. O arquivo de rótulo deve ter o formato a seguir. Confira o [arquivo de rótulo de exemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/Invoice_1.pdf.labels.json) para obter um exemplo completo.

```json
{
    "document": "Invoice_1.pdf",
    "labels": [
        {
            "label": "Provider",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Contoso",
                    "boundingBoxes": [
                        [
                            0.06334117647058823,
                            0.1053,
                            0.17018823529411767,
                            0.1053,
                            0.17018823529411767,
                            0.12303636363636362,
                            0.06334117647058823,
                            0.12303636363636362
                        ]
                    ]
                }
            ]
        },
        {
            "label": "For",
            "key": null,
            "value": [
                {
                    "page": 1,
                    "text": "Microsoft",
                    "boundingBoxes": [
                        [
                            0.6122941176470589,
                            0.1374,
                            0.6841764705882353,
                            0.1374,
                            0.6841764705882353,
                            0.14682727272727272,
                            0.6122941176470589,
                            0.14682727272727272
                        ]
                    ]
                },
                {
                    "page": 1,
                    "text": "1020",
                    "boundingBoxes": [
                        [
                            0.6121882352941176,
                            0.156,
                            0.6462941176470588,
                            0.156,
                            0.6462941176470588,
                            0.1653181818181818,
                            0.6121882352941176,
                            0.1653181818181818
                        ]
                    ]
                },
                ...
```

> [!NOTE]
> Você só pode aplicar um rótulo a cada elemento de texto, sendo que cada rótulo só pode ser aplicado uma vez por página. No momento, não é possível aplicar um rótulo em várias páginas.


## <a name="train-a-model-using-labeled-data"></a>Treinar um modelo usando dados rotulados

Para treinar um modelo usando dados rotulados, chame a API **[Treinar um modelo personalizado](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** executando o código Python a seguir. Antes de executar o código, faça estas alterações:

1. Substitua `<Endpoint>` pela URL do ponto de extremidade do recurso do Reconhecimento de Formulários.
1. Substitua `<SAS URL>` pela URL da Assinatura de Acesso Compartilhado (SAS) do contêiner de armazenamento de Blobs do Azure. Para recuperar a URL de SAS, abra o Gerenciador de Armazenamento do Microsoft Azure, clique com o botão direito do mouse no seu contêiner e selecione **Obter assinatura de acesso compartilhado**. Verifique se as permissões de **Leitura** e **Lista** estão marcadas e clique em **Criar**. Em seguida, copie o valor na seção **URL**. Deve ter o formato: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
1. Substitua `<Blob folder name>` pelo nome da pasta no contêiner de blob em que os dados de entrada estão localizados. Ou, se os dados estiverem na raiz, deixe-o em branco e remova o campo `"prefix"` do corpo da solicitação HTTP.

```python
########### Python Form Recognizer Labeled Async Train #############
import json
import time
from requests import get, post

# Endpoint URL
endpoint = r"<Endpoint>"
post_url = endpoint + r"/formrecognizer/v2.0-preview/custom/models"
source = r"<SAS URL>"
prefix = "<Blob folder name>"
includeSubFolders = False
useLabelFile = True

headers = {
    # Request headers
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '<subsription key>',
}

body =  {
    "source": source,
    "sourceFilter": {
        "prefix": prefix,
        "includeSubFolders": includeSubFolders
    },
    "useLabelFile": useLabelFile
}

try:
    resp = post(url = post_url, json = body, headers = headers)
    if resp.status_code != 201:
        print("POST model failed (%s):\n%s" % (resp.status_code, json.dumps(resp.json())))
        quit()
    print("POST model succeeded:\n%s" % resp.headers)
    get_url = resp.headers["location"]
except Exception as e:
    print("POST model failed:\n%s" % str(e))
    quit() 
```

## <a name="get-training-results"></a>Obter os resultados do treinamento

Depois de iniciar a operação de treinamento, use a ID retornada para obter o status da operação. Adicione o código a seguir à parte inferior do script Python. Isso usa o valor de ID da chamada de treinamento em uma nova chamada à API. A operação de treinamento é assíncrona, portanto, esse script chama a API em intervalos regulares, até que o status do treinamento seja "concluído". É recomendável fazer um intervalo de um segundo ou mais.

```python 
n_tries = 15
n_try = 0
wait_sec = 5
max_wait_sec = 60
while n_try < n_tries:
    try:
        resp = get(url = get_url, headers = headers)
        resp_json = resp.json()
        if resp.status_code != 200:
            print("GET model failed (%s):\n%s" % (resp.status_code, json.dumps(resp_json)))
            quit()
        model_status = resp_json["modelInfo"]["status"]
        if model_status == "ready":
            print("Training succeeded:\n%s" % json.dumps(resp_json))
            quit()
        if model_status == "invalid":
            print("Training failed. Model is invalid:\n%s" % json.dumps(resp_json))
            quit()
        # Training still running. Wait and retry.
        time.sleep(wait_sec)
        n_try += 1
        wait_sec = min(2*wait_sec, max_wait_sec)     
    except Exception as e:
        msg = "GET model failed:\n%s" % str(e)
        print(msg)
        quit()
print("Train operation did not complete within the allocated time.")
```

Quando o processo de treinamento for concluído, você receberá uma resposta `201 (Success)` com conteúdo JSON semelhante ao mostrado a seguir. A resposta foi abreviada para maior simplicidade.

```json
{ 
  "modelInfo":{ 
    "status":"ready",
    "createdDateTime":"2019-10-08T10:20:31.957784",
    "lastUpdatedDateTime":"2019-10-08T14:20:41+00:00",
    "modelId":"1cfb372bab404ba3aa59481ab2c63da5"
  },
  "trainResult":{ 
    "trainingDocuments":[ 
      { 
        "documentName":"invoices\\Invoice_1.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_2.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_3.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_4.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_5.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      }
    ],
    "errors":[ 

    ]
  },
  "keys":{ 
    "0":[ 
      "Address:",
      "Invoice For:",
      "Microsoft",
      "Page"
    ]
  }
}
```

Copie o valor `"modelId"` para usá-lo nas etapas a seguir.

[!INCLUDE [analyze forms](../includes/python-custom-analyze.md)]

Quando o processo for concluído, você receberá uma resposta `202 (Success)` com conteúdo JSON no formato a seguir. A resposta foi abreviada para maior simplicidade. As principais associações de chave/valor estão no nó `"documentResults"`. Os resultados da API de layout (o conteúdo e as posições de todo o texto no documento) estão no nó `"readResults"`.

```json
{ 
    "analyzeResult":{ 
      "version":"2.0.0",
      "readResults":[ 
        { 
          "page":1,
          "language":"en",
          "angle":0,
          "width":8.5,
          "height":11,
          "unit":"inch",
          "lines":[ 
            { 
              "language":"en",
              "boundingBox":[ 
                0.5375,
                1.1349,
                2.6064,
                1.1349,
                2.6064,
                1.354,
                0.5375,
                1.354
              ],
              "text":"Contoso Suites",
              "words":[ 
                { 
                  "boundingBox":[ 
                    0.5375,
                    1.1402,
                    1.6595,
                    1.1402,
                    1.6595,
                    1.354,
                    0.5375,
                    1.354
                  ],
                  "text":"Contoso",
                  "confidence":1
                },
                { 
                  "boundingBox":[ 
                    1.758,
                    1.1349,
                    2.6064,
                    1.1349,
                    2.6064,
                    1.3534,
                    1.758,
                    1.3534
                  ],
                  "text":"Suites",
                  "confidence":1
                }
              ]
            },
            ...
          ]
        }
      ],
      "pageResults":[ 
        { 
          "page":1,
          "tables":[ 
            { 
              "rows":2,
              "columns":6,
              "cells":[ 
                { 
                  "rowIndex":0,
                  "columnIndex":0,
                  "text":"Invoice Number",
                  "boundingBox":[ 
                    0.5075,
                    2.8088,
                    1.9061,
                    2.8088,
                    1.9061,
                    3.3219,
                    0.5075,
                    3.3219
                  ],
                  "elements":[ 
                    "#/readResults/0/lines/7/words/0",
                    "#/readResults/0/lines/7/words/1"
                  ]
                },
                { 
                  "rowIndex":0,
                  "columnIndex":1,
                  "text":"Invoice Date",
                  "boundingBox":[ 
                    1.9061,
                    2.8088,
                    3.3074,
                    2.8088,
                    3.3074,
                    3.3219,
                    1.9061,
                    3.3219
                  ],
                  "elements":[ 
                    "#/readResults/0/lines/8/words/0",
                    "#/readResults/0/lines/8/words/1"
                  ]
                },
                ...     
              ]
            }
          ]
        }
      ],
      "documentResults":[ 
        { 
          "docType":"Analyze",
          "pageRange":[ 
            1,
            1
          ],
          "fields":{ 
            "total":{ 
              "type":"string",
              "valueString":"$22,123.24",
              "text":"$22,123.24",
              "boundingBox":[ 
                5.29,
                3.41,
                5.9750000000000009,
                3.41,
                5.9750000000000009,
                3.54,
                5.29,
                3.54
              ],
              "page":1,
              "confidence":1,
              "elements":[ 
                { 
                  "$ref":"#/analyzeResult/readResults/0/lines/15/words/0"
                }
              ],
              "fieldName":"total"
            },
            "invoice #":{ 
              "type":"string",
              "valueString":"7689302",
              "text":"7689302",
              "boundingBox":[ 
                0.54,
                3.41,
                1.065,
                3.41,
                1.065,
                3.515,
                0.54,
                3.515
              ],
              "page":1,
              "confidence":1,
              "elements":[ 
                { 
                  "$ref":"#/analyzeResult/readResults/0/lines/12/words/0"
                }
              ],
              "fieldName":"invoice #"
            },
            "vat":{ 
              "type":"string",
              "valueString":"QR",
              "text":"QR",
              "boundingBox":[ 
                6.2250000000000009,
                3.41,
                6.425,
                3.41,
                6.425,
                3.52,
                6.2250000000000009,
                3.52
              ],
              "page":1,
              "confidence":0.9839357733726502,
              "elements":[ 
                { 
                  "$ref":"#/analyzeResult/readResults/0/lines/16/words/0"
                }
              ],
              "fieldName":"vat"
            },
            ...
          }
        }
      ]
    },
    "status":"succeeded",
    "createdDateTime":"2019-11-12T21:26:19+00:00",
    "lastUpdatedDateTime":"2019-11-12T21:27:27.0488571+00:00"
}
```

## <a name="improve-results"></a>Aprimorar os resultados

Examine os valores de `"confidence"` para cada resultado de chave-valor no nó `"documentResults"`. Você também deve examinar as pontuações de confiança no nó `"readResults"`, que correspondem à operação de layout. A confiança dos resultados de layout não afeta a confiança dos resultados de extração de chave-valor, portanto, você deve verificar ambos.
* Se as pontuações de confiança da operação de layout estiverem baixas, tente aprimorar a qualidade de seus documentos de entrada (consulte [Requisitos de entrada](../overview.md#input-requirements)).
* Se as pontuações de confiança para a operação de extração de chave-valor estiverem baixas, verifique se os documentos sendo analisados são do mesmo tipo que os documentos usados no conjunto de treinamento. Se os documentos no conjunto de treinamento tiverem variações de aparência, considere dividi-los em pastas diferentes e treinar um modelo para cada variação.

### <a name="avoid-cluttered-labels"></a>Evitar rótulos com aparência congestionada

Às vezes, quando você aplica rótulos diferentes dentro da mesma linha de texto, o serviço pode mesclar esses rótulos em um campo. Por exemplo, em um endereço, você pode rotular a cidade, o estado e o CEP como campos diferentes, mas durante a previsão, esses campos não são reconhecidos separadamente.

Entendemos que esse cenário é essencial para nossos clientes e estamos trabalhando para aprimorar isso no futuro. Atualmente, recomendamos que nossos usuários rotulem vários campos de aparência congestionada como um campo e, em seguida, separem os termos em um pós-processamento dos resultados de extração.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você apendeu como usar a API REST do Reconhecimento de Formulários com o Python para treinar um modelo usando dados rotulados manualmente. Em seguida, confira a [documentação de referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm) para explorar a API de Reconhecimento de Formulários de maneira mais aprofundada.
