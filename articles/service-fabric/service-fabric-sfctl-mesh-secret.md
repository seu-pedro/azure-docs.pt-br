---
title: CLI do Azure Service Fabric-segredo de malha sfctl
description: Saiba mais sobre o sfctl, a interface de linha de comando Service Fabric do Azure. Inclui uma lista de comandos para obter e excluir Service Fabric recursos secretos de malha.
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: f43f4fba0d7550ccb09e54f178a78d01f01cfc9d
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645337"
---
# <a name="sfctl-mesh-secret"></a>sfctl mesh secret
Obter e excluir recursos de segredo da malha.

## <a name="commands"></a>Comandos

|Comando|Description|
| --- | --- |
| excluir | Exclui o recurso de segredo. |
| list | Lista todos os recursos de segredo. |
| mostrar | Obtém o recurso de segredo com o nome fornecido. |

## <a name="sfctl-mesh-secret-delete"></a>sfctl mesh secret delete
Exclui o recurso de segredo.

Exclui o recurso de segredo especificado e todos os seus valores nomeados.

### <a name="arguments"></a>Argumentos

|Argumento|Description|
| --- | --- |
| --name -n [Obrigatório] | O nome do recurso de segredo. |

### <a name="global-arguments"></a>Argumentos globais

|Argumento|Description|
| --- | --- |
| --debug | Aumente o detalhamento do log para mostrar todos os logs de depuração. |
| --help -h | Mostrar esta mensagem de ajuda e sair. |
| --output -o | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Padrão\: json. |
| --query | Cadeia de caracteres de consulta JMESPath. Veja http\://jmespath.org/ para saber mais e obter exemplos. |
| --verbose | Aumentar o detalhamento do log. Use --debug para logs de depuração completos. |

## <a name="sfctl-mesh-secret-list"></a>sfctl mesh secret list
Lista todos os recursos de segredo.

Obtém as informações sobre todos os recursos de segredo em um determinado grupo de recursos. As informações incluem a descrição e outras propriedades do segredo.

### <a name="global-arguments"></a>Argumentos globais

|Argumento|Description|
| --- | --- |
| --debug | Aumente o detalhamento do log para mostrar todos os logs de depuração. |
| --help -h | Mostrar esta mensagem de ajuda e sair. |
| --output -o | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Padrão\: json. |
| --query | Cadeia de caracteres de consulta JMESPath. Veja http\://jmespath.org/ para saber mais e obter exemplos. |
| --verbose | Aumentar o detalhamento do log. Use --debug para logs de depuração completos. |

## <a name="sfctl-mesh-secret-show"></a>sfctl mesh secret show
Obtém o recurso de segredo com o nome fornecido.

Obtém as informações sobre o recurso de segredo com o nome fornecido. As informações incluem a descrição e outras propriedades do segredo.

### <a name="arguments"></a>Argumentos

|Argumento|Description|
| --- | --- |
| --name -n [Obrigatório] | O nome do recurso de segredo. |

### <a name="global-arguments"></a>Argumentos globais

|Argumento|Description|
| --- | --- |
| --debug | Aumente o detalhamento do log para mostrar todos os logs de depuração. |
| --help -h | Mostrar esta mensagem de ajuda e sair. |
| --output -o | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Padrão\: json. |
| --query | Cadeia de caracteres de consulta JMESPath. Veja http\://jmespath.org/ para saber mais e obter exemplos. |
| --verbose | Aumentar o detalhamento do log. Use --debug para logs de depuração completos. |


## <a name="next-steps"></a>Próximos passos
- [Configurar](service-fabric-cli.md) a CLI do Service Fabric.
- Saiba como usar a CLI do Service Fabric usando os [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).