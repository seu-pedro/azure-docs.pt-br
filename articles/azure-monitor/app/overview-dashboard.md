---
title: Painel de Visão geral do Azure Application Insights | Microsfoft Docs
description: Monitore aplicativos com a funcionalidade do Azure Application Insights e do Painel de Visão Geral.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/03/2019
ms.openlocfilehash: 45c04cc32733873061bd6ff44b2e5484b106a893
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75406649"
---
# <a name="application-insights-overview-dashboard"></a>Painel de visão geral do Application Insights

O Application Insights sempre forneceu um painel de visão geral resumida para permitir a avaliação rápida e prática da integridade e do desempenho do aplicativo. O novo painel de visão geral fornece uma experiência mais rápida e flexível.

## <a name="how-do-i-test-out-the-new-experience"></a>Como testar out a nova experiência?

O novo painel de visão geral agora inicia por padrão:

![Painel Versão prévia de visão geral](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>Melhor desempenho

A seleção de intervalo de tempo foi simplificada para uma interface de um clique simples.

![Intervalo de horas](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

O desempenho geral foi aumentado significativamente. Você tem acesso a recursos populares como **Pesquisa** e **Análise**. Cada bloco de KPI de atualização dinâmica padrão fornece insight sobre os recursos do Visual Studio Online Application Insights. Para saber mais sobre solicitações com falha, selecione **Falhas** no cabeçalho **Investigar**:

![Falhas](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Painel do aplicativo

O painel do aplicativo utiliza a tecnologia de painel existente no Azure para fornecer uma exibição de painel único totalmente personalizável do desempenho e integridade do aplicativo.

Para acessar o painel padrão, selecione _Painel do aplicativo_ no canto superior esquerdo.

![Exibição Painel](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

Se esta for a primeira vez que você acessa o painel, ele inicializará um modo de exibição padrão:

![Exibição Painel](./media/overview-dashboard/0001-dashboard.png)

Se você desejar, mantenha a visualização padrão. Ou você pode adicionar e excluir do painel para melhor atender às necessidades da sua equipe.

> [!NOTE]
> Todos os usuários com acesso ao recurso Application Insights compartilham a mesma experiência de painel do Application. As alterações feitas por um usuário modificarão a exibição para todos os usuários.

Para navegar de volta para a experiência de visão geral basta selecionar:

![Botão Visão geral](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>Solução de problemas

Se você selecionar **definir configurações de bloco** e definir um intervalo de tempo personalizado acima de 31 dias, seu painel não será exibido além de 31 dias de dados, mesmo com a retenção de dados padrão de 90 dias. Atualmente, não há nenhuma solução alternativa para esse comportamento.

## <a name="next-steps"></a>Próximos passos

- [Funis](../../azure-monitor/app/usage-funnels.md)
- [Retenção](../../azure-monitor/app/usage-retention.md)
- [Fluxos de Usuário](../../azure-monitor/app/usage-flows.md)
