---
title: Início Rápido – Criar um conjunto de dimensionamento de máquinas virtuais no portal do Azure
description: Comece com suas implantações aprendendo a criar rapidamente um conjunto de dimensionamento de máquinas virtuais no portal do Azure.
services: virtual-machine-scale-sets.
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 10/23/2019
ms.author: cynthn
ms.openlocfilehash: 5a44aad7471c29a387337a5ee988e66287fca3ca
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75351133"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Início Rápido: Criar um conjunto de dimensionamento de máquinas virtuais no Portal do Azure

Um conjunto de dimensionamento de máquinas virtuais permite implantar e gerenciar um conjunto de máquinas virtuais idênticas de dimensionamento automático. É possível dimensionar o número de VMs manualmente no conjunto de dimensionamento ou definir as regras para o dimensionamento automático com base no uso de recursos, como CPU, demanda de memória ou tráfego de rede. Um balanceador de carga do Azure então distribui o tráfego para as instâncias de VM no conjunto de dimensionamento. Neste início rápido, você criará um conjunto de dimensionamento de máquinas virtuais no Portal do Azure.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="log-in-to-azure"></a>Fazer logon no Azure
Faça logon no Portal do Azure em https://portal.azure.com.

## <a name="create-a-load-balancer"></a>Criar um balanceador de carga

O [balanceador de carga](../load-balancer/load-balancer-overview.md) do Azure distribui o tráfego de entrada entre instâncias de máquina virtual íntegras. 

Primeiro, crie um Load Balancer Básico usando o portal. O nome e o endereço IP público que você criar são configurados automaticamente como o front-end do balanceador de carga.

1. Na caixa de pesquisa, digite **balanceador de carga**. Em **Marketplace** nos resultados da pesquisa, escolha **Balanceador de carga**.
1. Na guia **Básico** da página **Criar balanceador de carga**, insira ou selecione as seguintes informações:

    | Configuração                 | Valor   |
    | ---| ---|
    | Subscription  | Selecione sua assinatura.    |    
    | Resource group | Selecione **Criar novo** e digite *myVMSSResourceGroup* na caixa de texto.|
    | Nome           | *myLoadBalancer*         |
    | Região         | Selecione **Leste dos EUA**.       |
    | Type          | Selecione **Público**.       |
    | SKU           | Selecione **Padrão**.       |
    | Endereço IP público | Selecione **Criar novo**. |
    | Nome do endereço IP público  | *MyPip*   |
    | Atribuição| Estático |

1. Quando terminar, selecione **Revisar + criar** 
1. Depois de passar na validação, selecione **Criar**. 

![Criar um balanceador de carga](./media/virtual-machine-scale-sets-create-portal/load-balancer.png)

## <a name="create-virtual-machine-scale-set"></a>Criar conjunto de dimensionamento de máquinas virtuais
Você pode implantar um conjunto de dimensionamento com uma imagem do Windows Server ou do Linux como CentOS, RHEL, Ubuntu ou SLES.

1. Digite **Conjunto de dimensionamento** na caixa de pesquisa. Nos resultados, em **Marketplace**, selecione **Conjuntos de dimensionamento de máquinas virtuais**. A página **Criar um conjunto de dimensionamento de máquinas virtuais** será aberta. 
1. Na guia **Básico**, em **Detalhes do projeto**, verifique se a assinatura correta está selecionada e, em seguida, escolha **Criar** grupo de recursos. Digite *myVMSSResourceGroup* para o nome e, em seguida, selecione **OK**. 
1. Digite *myScaleSet* como o nome do conjunto de dimensionamento.
1. Em **Região**, selecione uma região próxima à sua área.
1. Deixe o valor padrão **VMs do conjunto de dimensionamento** para **Orquestrador**.
1. Selecione uma imagem do marketplace para **Imagem**. Neste exemplo, escolhemos *Ubuntu Server 18.04 LTS*.
1. Insira o nome de usuário desejado e selecione o tipo de autenticação que prefere.
   - Uma **Senha** deve ter pelo menos 12 caracteres e atender três dos quatro requisitos de complexidade a seguir: um caractere minúsculo, um caractere maiúsculo, um número e um caractere especial. Para obter mais informações, consulte [requisitos de senha e nome de usuário](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
   - Se você selecionar uma imagem de disco do SO Linux, em vez disso, você poderá escolher **chave pública SSH**. Apenas forneça sua chave pública, tal como *~/.ssh/id_rsa.pub*. Você pode usar o Azure Cloud Shell por meio do portal para [criar e usar as chaves de SSH](../virtual-machines/linux/mac-create-ssh-keys.md).
   
    ![Criar um conjunto de dimensionamento de máquinas virtuais](./media/virtual-machine-scale-sets-create-portal/quick-create-scaleset.png)

1. Selecione **Avançar** para acessar as outras páginas. 
1. Deixe os padrões nas páginas **Instância** e **Discos**.
1. Na página **Rede**, em **Balanceamento de carga**, selecione **Sim** para colocar as instâncias do conjunto de dimensionamento atrás de um balanceador de carga. 
1. Para **Opções de balanceamento de carga**, selecione **Azure Load Balancer**.
1. Em **Selecionar um balanceador de carga**, selecione o *myLoadBalancer* que você criou anteriormente.
1. Em **Selecionar um pool de back-end**, selecione **Criar novo**, digite *myBackendPool* e selecione **Criar**.
1. Quando terminar, selecione **Revisar + criar**. 
1. Depois de passar na validação, selecione **Criar** para implantar o conjunto de dimensionamento.


## <a name="clean-up-resources"></a>Limpar os recursos
Quando o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados não forem mais necessários, exclua-os. Para fazer isso, selecione o grupo de recursos do conjunto de dimensionamento e selecione **Excluir**.


## <a name="next-steps"></a>Próximas etapas
Neste início rápido, você criará um conjunto de dimensionamento básico no Portal do Azure. Para saber mais, siga para o tutorial sobre como criar e gerenciar um conjunto de dimensionamento de máquinas virtuais do Azure.

> [!div class="nextstepaction"]
> [Criar e gerenciar um conjunto de dimensionamento de máquinas virtuais do Azure](tutorial-create-and-manage-powershell.md)
