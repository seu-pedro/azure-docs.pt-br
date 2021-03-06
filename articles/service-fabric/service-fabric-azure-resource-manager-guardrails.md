---
title: Guardrails Azure Resource Manager de implantação do Service Fabric
description: Este artigo fornece uma visão geral dos erros comuns feitos ao implantar um cluster de Service Fabric por meio de Azure Resource Manager e como evitá-los.
services: service-fabric
documentationcenter: .net
author: peterpogorski
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: pepogors
ms.openlocfilehash: fe5ff2a5eeb4b2c73165d1577702eb6af7079b61
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426732"
---
# <a name="service-fabric-guardrails"></a>Service Fabric guardrails 
Ao implantar um cluster de Service Fabric, os guardrails são colocados em vigor, o que falhará em uma implantação de Azure Resource Manager no caso de uma configuração de cluster inválida. As seções a seguir fornecem uma visão geral dos problemas comuns de configuração de cluster e as etapas necessárias para mitigar esses problemas. 

## <a name="durability-mismatch"></a>Incompatibilidade de durabilidade
### <a name="overview"></a>Visão Geral
O valor de durabilidade para um Service Fabric tipo de nó é definido em duas seções diferentes de um modelo de Azure Resource Manager. A seção extensão do conjunto de dimensionamento de máquinas virtuais do recurso de conjunto de dimensionamento de máquinas virtuais e a seção tipo de nó do recurso de Cluster Service Fabric. É um requisito que o valor de durabilidade nessas seções corresponde, caso contrário, a implantação de recursos falhará.

A seção a seguir contém um exemplo de incompatibilidade de durabilidade entre a configuração de durabilidade da extensão do conjunto de dimensionamento de máquinas virtuais e a configuração de durabilidade do tipo de nó Service Fabric  

**Configuração de durabilidade do conjunto de dimensionamento de máquinas virtuais**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Configuração de durabilidade do tipo de nó Service Fabric** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>Mensagens de erro
* A incompatibilidade de durabilidade do conjunto de dimensionamento de máquinas virtuais não corresponde ao nível de durabilidade Service Fabric atual do tipo de nó
* A durabilidade do conjunto de dimensionamento de máquinas virtuais não corresponde ao nível de durabilidade do tipo de nó de Service Fabric
* A durabilidade do conjunto de dimensionamento de máquinas virtuais corresponde ao nível de durabilidade atual Service Fabric ou ao nível de durabilidade do tipo de nó de Service Fabric 


### <a name="mitigation"></a>Atenuação
Para corrigir uma incompatibilidade de durabilidade, que é indicada por qualquer uma das mensagens de erro acima:
1. Atualize o nível de durabilidade na seção extensão do conjunto de dimensionamento de máquinas virtuais ou Service Fabric tipo de nó do modelo de Azure Resource Manager para garantir que os valores correspondam.
2. Reimplante o modelo de Azure Resource Manager com os valores atualizados.

## <a name="next-steps"></a>Próximos passos
* Crie um cluster em VMs ou em computadores que estejam executando o Windows Server: [Criação de um cluster do Service Fabric para o Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Criar um cluster em VMs ou em computadores que estejam executando o Linux: [Criar um cluster do Linux](service-fabric-cluster-creation-via-portal.md)
* Solucionar problemas Service Fabric: [guias de solução de problemas](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
