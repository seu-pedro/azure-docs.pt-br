---
title: Criar um registro e uma zona DNS para um nome de domínio - CLI do Azure - Azure DNS
description: Este exemplo de script da CLI do Azure mostra como criar um registro e uma zona DNS para um nome de domínio
services: dns
author: asudbring
ms.service: dns
ms.topic: sample
ms.date: 09/20/2019
ms.author: allensu
ms.openlocfilehash: 8f5dde0a35f31d2c33ab2857659efc88339c2ead
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74210390"
---
# <a name="azure-cli-script-example-create-a-dns-zone-and-record"></a>Exemplo de script da CLI do Azure: Criar uma zona e registro de DNS

Este exemplo de script da CLI do Azure cria um registro e uma zona DNS para um nome de domínio. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/dns/create-dns-zone-and-record/create-dns-zone-and-record.sh "Create DNS zone and record")]

## <a name="clean-up-deployment"></a>Limpar a implantação 

Execute o comando a seguir para remover o grupo de recursos, a zona DNS e todos os recursos relacionados.

```azurecli
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Explicação sobre o script

Esse script usa os seguintes comandos para criar um grupo de recursos, uma máquina virtual, um conjunto de disponibilidade, um balanceador de carga e todos os recursos relacionados. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network dns zone create](/cli/azure/network/dns/zone#az-network-dns-zone-create) | Cria uma zona DNS do Azure. |
| [az network dns record-set a add-record](/cli/azure/network/dns/record-set) | Adiciona um registro *A* a uma zona DNS. |
| [az network dns record-set list](/cli/azure/network/dns/record-set) | Lista todos os conjuntos de registros *A* em uma zona DNS. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Exclui um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

