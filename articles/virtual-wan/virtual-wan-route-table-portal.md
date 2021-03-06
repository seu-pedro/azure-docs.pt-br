---
title: 'WAN virtual: criar tabela de rotas de Hub virtual para NVA: portal do Azure'
description: Tabela de rotas do Hub virtual WAN virtual para direcionar o tráfego para uma solução de virtualização de rede usando o Portal.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a route table using the portal.
ms.openlocfilehash: c0681024b60827cf589906041c264d912ab209bb
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75612353"
---
# <a name="create-a-virtual-wan-hub-route-table-for-nvas-azure-portal"></a>Criar uma tabela de rotas do Hub WAN virtual para NVAs: portal do Azure

Este artigo mostra como direcionar o tráfego de um Branch (site local) conectado ao Hub WAN virtual a uma vnet spoke por meio de uma NVA (solução de virtualização de rede).

![Diagrama de WAN virtual](./media/virtual-wan-route-table/vwanroute.png)

## <a name="before-you-begin"></a>Antes de começar

Verifique se você atende aos seguintes critérios:

*  Você tem uma solução de virtualização de rede (NVA). Uma solução de virtualização de rede é um software de terceiros de sua escolha que normalmente é provisionado do Azure Marketplace em uma rede virtual.

    * Um endereço IP privado deve ser atribuído à interface de rede NVA.

    * O NVA não está implantado no Hub virtual. Ela deve ser implantada em uma VNet separada.

    *  A VNet NVA pode ter uma ou várias redes virtuais conectadas a ela. Neste artigo, nos referimos à VNet NVA como uma "VNet spoke indireta". Esses VNets podem ser conectados à VNet do NVA usando o emparelhamento VNet. Os links de emparelhamento vnet são representados por setas pretas na figura acima, entre vnet 1, vnet 2 e NVA vnet.
*  Você criou 2 VNets. Eles serão usados como spoke VNets.

    * Para este exercício, os espaços de endereço do spoke da VNet são: VNet1:10.0.2.0/24 e VNet2:10.0.3.0/24. Se você precisar de informações sobre como criar uma VNet, consulte [criar uma rede virtual](../virtual-network/quick-create-portal.md).

    * Verifique se não há nenhum gateway de rede virtual em nenhum dos VNets.
    * Para essa configuração, esses VNets não exigem uma sub-rede de gateway.

## <a name="signin"></a>1. entrar

Em um navegador, acesse o [Portal do Azure](https://portal.azure.com) e entre com sua conta do Azure.

## <a name="vwan"></a>2. criar uma WAN virtual

Crie uma WAN virtual. Para os fins deste exercício, você pode usar os seguintes valores:

* **Nome da WAN virtual:** myVirtualWAN
* **Grupo de recursos:** testRG
* **Local:** Oeste dos EUA

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="hub"></a>3. criar um hub

Crie o Hub. Para os fins deste exercício, você pode usar os seguintes valores:

* **Local:** Oeste dos EUA
* **Nome:** westushub
* **Espaço de endereço privado do Hub:** 10.0.1.0/24

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="route"></a>4. criar e aplicar uma tabela de rotas de Hub

Atualize o Hub com uma tabela de rotas de Hub. Para os fins deste exercício, você pode usar os seguintes valores:

* **Espaços de endereço da VNet do spoke:** (VNet1 e VNet2) 10.0.2.0/24 e 10.0.3.0/24
* **Endereço IP privado da interface de rede DMZ NVA:** 10.0.4.5

1. Navegue até sua WAN virtual.
2. Clique no Hub para o qual você deseja criar uma tabela de rotas.
3. Clique em **...** e em **Editar Hub virtual**.
4. Na página **Editar Hub virtual** , role para baixo e marque a caixa de seleção **usar tabela para roteamento**.
5. Na coluna **se o prefixo de destino for** , adicione os espaços de endereço. Na coluna **Enviar para o próximo salto** , adicione o endereço IP privado da interface de rede do DMZ NVA.
6. Clique em **confirmar** para atualizar o recurso de Hub com as configurações da tabela de rotas.

## <a name="connections"></a>5. criar as conexões de VNet

Crie uma conexão de vnet de cada VNet de spoke indireto (VNet1 e VNet2) para o Hub. Essas conexões vnet são descritas pelas setas azuis na figura acima. Em seguida, crie uma conexão de vnet da VNet NVA para o Hub (seta preta na figura). 

 Para esta etapa, você pode usar os seguintes valores:

| Nome da VNet| Nome da conexão|
| --- | --- |
| VNet1 | testconnection1 |
| VNet2 | testconnection2 |
| NVAVNet | testconnection3 |

Repita o procedimento a seguir para cada VNet que você deseja conectar.

1. Na página da WAN virtual, clique em **Conexões de rede virtual**.
2. Na página de conexão de rede virtual, clique em **+Adicionar conexão**.
3. Na página **Adicionar conexão**, preencha os seguintes campos:

    * **Nome da Conexão**: nomeie sua conexão.
    * **Hubs**: selecione o hub que você deseja associar a essa conexão.
    * **Assinatura**: verifique a assinatura.
    * **Rede virtual:** selecione a rede virtual que você deseja conectar a esse hub. A rede virtual não pode ter um gateway de rede virtual já existente.
4. Clique em **OK** para criar a conexão.

## <a name="next-steps"></a>Próximos passos

Para saber mais sobre a WAN Virtual, consulte a página [Visão geral de WAN Virtual](virtual-wan-about.md).
