---
title: Mover um recurso de VM do Windows no Azure
description: Mova uma VM Windows para outro grupo de recursos ou outra assinatura do Azure no modelo de implantação do Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/03/2019
ms.author: cynthn
ms.openlocfilehash: f5b4bf14be264d16109ddc10cd3b667e728642c6
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980698"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Mover uma VM Windows para outra assinatura ou outro grupo de recursos do Azure
Este artigo explica como mover uma VM (máquina virtual) do Windows entre grupos de recursos ou assinaturas. A movimentação entre assinaturas poderá ser útil se você tiver originalmente criado uma VM em uma assinatura pessoal e agora deseja movê-la para a assinatura da sua empresa a fim de continuar seu trabalho. Você não precisa iniciar a VM para movê-la e ela deve continuar a ser executada durante a movimentação.

> [!IMPORTANT]
>Novas IDs de recurso são criadas como parte da mudança. Depois que a VM for movida, você precisará atualizar suas ferramentas e scripts para usar as novas IDs de recursos.
>
>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Usar o Powershell para mover uma VM

Para mover uma máquina virtual para outro grupo de recursos, você precisa ter certeza de também mover todos os recursos dependentes. Para obter uma lista com a ID do recurso de cada um desses recursos, use o cmdlet [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource).

```azurepowershell-interactive
 Get-AzResource -ResourceGroupName <sourceResourceGroupName> | Format-list -wrap -Property ResourceId
```

Você pode usar a saída do comando anterior como uma lista de IDs do recurso separada por vírgula para [Move-AzResource](https://docs.microsoft.com/powershell/module/az.resources/move-azresource) para mover cada recurso para o destino.

```azurepowershell-interactive
Move-AzResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```

Para mover os recursos para outra assinatura, inclua o parâmetro **-DestinationSubscriptionId** .

```azurepowershell-interactive
Move-AzResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Quando for solicitado que você confirme que deseja mover os recursos especificados, insira **Y** para confirmar.

## <a name="next-steps"></a>Próximos passos
Você pode mover vários tipos diferentes de recursos entre grupos de recursos e assinaturas. Para obter mais informações, consulte [Mover recursos para um novo grupo de recursos ou assinatura](../../azure-resource-manager/management/move-resource-group-and-subscription.md).    
