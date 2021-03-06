---
title: Visão geral da solução de problemas de área de trabalho virtual do Windows – Azure
description: Uma visão geral para solucionar problemas durante a configuração de um ambiente de locatário de área de trabalho virtual do Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/06/2019
ms.author: helohr
ms.openlocfilehash: ce1af278d67f96873ed611db8dc05938205638dc
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980234"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Visão geral da solução de problemas, comentários e suporte

Este artigo fornece uma visão geral dos problemas que você pode encontrar ao configurar um ambiente de locatário de área de trabalho virtual do Windows e fornece maneiras de resolver os problemas.

## <a name="provide-feedback"></a>Fornecer comentários

Visite a [Comunidade Tecnológica da Área de Trabalho Virtual do Windows](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para comentar sobre o serviço da Área de Trabalho Virtual do Windows com a equipe do produto e membros ativos da comunidade.

## <a name="escalation-tracks"></a>Faixas de escalonamento

Use a tabela a seguir para identificar e resolver os problemas que você pode encontrar ao configurar um ambiente de locatário usando o Área de Trabalho Remota Client. Depois de configurar o locatário, você pode usar nosso novo [serviço de diagnóstico](https://docs.microsoft.com/azure/virtual-desktop/diagnostics-role-service) para identificar problemas em cenários comuns.

>[!NOTE]
> Temos um fórum da comunidade técnica que você pode visitar para discutir seus problemas com a equipe de produto e os membros ativos da Comunidade. Visite a [comunidade técnica de área de trabalho virtual do Windows](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para

| **Problema**                                                            | **Solução sugerida**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Criando um locatário                                                    | Se houver uma interrupção do Azure, entre em contato com o [suporte do Azure](https://azure.microsoft.com/support/options/); caso contrário, **abra uma solicitação de suporte para a área de trabalho virtual do Windows (computação)** .|
| Acessando modelos do Marketplace no portal do Azure       | Se houver uma interrupção do Azure, entre em contato com o [suporte do Azure](https://azure.microsoft.com/support/options/). <br> <br> Os modelos de área de trabalho virtual do Windows do Azure Marketplace estão disponíveis gratuitamente.|
| Acessando modelos de Azure Resource Manager do GitHub                                  | Consulte a seção "criando VMs do host de sessão de área de trabalho virtual do Windows" do [locatário e da criação do pool de hosts](troubleshoot-set-up-issues.md). Se o problema ainda não for resolvido, entre em contato com a [equipe de suporte do GitHub](https://github.com/contact). <br> <br> Se o erro ocorrer depois de acessar o modelo no GitHub, entre em contato com o [suporte do Azure](https://azure.microsoft.com/support/options/).|
| Configurações de rede virtual (VNET) do pool de hosts da sessão e de rota expressa               | Contate o **suporte do Azure (rede)** . |
| Criação de VM (máquina virtual) do pool de hosts da sessão quando Azure Resource Manager modelos fornecidos com a área de trabalho virtual do Windows não estão sendo usados | Contate o **suporte do Azure (computação)** . <br> <br> Para problemas com os modelos de Azure Resource Manager que são fornecidos com a área de trabalho virtual do Windows, consulte a seção criando locatário da área de trabalho virtual do Windows do [locatário e criação do pool de hosts](troubleshoot-set-up-issues.md). |
| Gerenciando o ambiente de host de sessão de área de trabalho virtual do Windows no portal do Azure    | Contate o **suporte do Azure**. <br> <br> Para problemas de gerenciamento ao usar o Serviços de Área de Trabalho Remota/Windows Virtual Desktop PowerShell, consulte [PowerShell da área de trabalho virtual do Windows](troubleshoot-powershell.md) ou **abra uma solicitação de suporte para a área de trabalho virtual do Windows (computação)** . |
| Gerenciando a configuração da área de trabalho virtual do Windows vinculada a pools de hosts e grupos de aplicativos      | Consulte [PowerShell da área de trabalho virtual do Windows](troubleshoot-powershell.md)ou **abra uma solicitação de suporte para área de trabalho virtual do Windows (computação)** . <br> <br> Se os problemas estiverem vinculados à GUI (interface gráfica do usuário) de exemplo, entre em contato com a Comunidade do Yammer.|
| Problemas de clientes de área de trabalho remota em iniciar                                                 | Consulte [solucionar problemas do cliente área de trabalho remota](troubleshoot-client.md) e, se isso não resolver o problema, **abra uma solicitação de suporte para a área de trabalho virtual do Windows (computação)** .  <br> <br> Se for um problema de rede, os usuários precisarão entrar em contato com o administrador da rede. |
| Conectado, mas sem feed                                                                 | Solucionar problemas usando a seção "o usuário se conecta, mas nada é exibido (sem feed)" de [solucionar problemas do cliente área de trabalho remota](troubleshoot-client.md). <br> <br> Se os usuários tiverem sido atribuídos a um grupo de aplicativos, **abra uma solicitação de suporte para a área de trabalho virtual do Windows (computação)** . |
| Problemas de descoberta de feed devido à rede                                            | Os usuários precisam entrar em contato com o administrador da rede. |
| Conectando clientes                                                                    | Consulte [conexões do serviço de área de trabalho virtual do Windows](troubleshoot-service-connection.md) e, se isso não resolver o problema, consulte [configuração de máquina virtual do host de sessão](troubleshoot-vm-configuration.md). |
| Capacidade de resposta de aplicativos remotos ou área de trabalho                                      | Se os problemas estiverem vinculados a um aplicativo ou produto específico, entre em contato com a equipe responsável pelo produto. |
| Mensagens de licenciamento ou erros                                                          | Se os problemas estiverem vinculados a um aplicativo ou produto específico, entre em contato com a equipe responsável pelo produto. |
| Problemas ao usar as ferramentas de área de trabalho virtual do Windows no GitHub (modelos de Azure Resource Manager, ferramenta de diagnóstico, ferramenta de gerenciamento) | Consulte [modelos de Azure Resource Manager para serviços de área de trabalho remota](https://github.com/Azure/RDS-Templates/blob/master/README.md) para relatar problemas. |

## <a name="next-steps"></a>Próximos passos

- Para solucionar problemas ao criar um pool de locatários e de host em um ambiente de área de trabalho virtual do Windows, confira [criação de locatário e pool de hosts](troubleshoot-set-up-issues.md).
- Para solucionar problemas durante a configuração de uma VM (máquina virtual) na área de trabalho virtual do Windows, consulte [configuração de máquina virtual do host de sessão](troubleshoot-vm-configuration.md).
- Para solucionar problemas com conexões de cliente de área de trabalho virtual do Windows, consulte [conexões do serviço área de trabalho virtual do Windows](troubleshoot-service-connection.md).
- Para solucionar problemas com clientes Área de Trabalho Remota, consulte [solucionar problemas do cliente área de trabalho remota](troubleshoot-client.md)
- Para solucionar problemas ao usar o PowerShell com a área de trabalho virtual do Windows, consulte [PowerShell da área de trabalho virtual do Windows](troubleshoot-powershell.md).
- Para saber mais sobre o serviço, consulte [ambiente de área de trabalho virtual do Windows](environment-setup.md).
- Para percorrer um tutorial de solução de problemas, consulte [tutorial: solucionar problemas de implantações de modelo do Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Para saber sobre as ações de auditoria, consulte [Auditar operações com o Gerenciador de Recursos](../azure-resource-manager/management/view-activity-logs.md).
- Para saber mais sobre as ações para determinar erros durante a implantação, consulte [Exibir operações de implantação](../azure-resource-manager/templates/deployment-history.md).
