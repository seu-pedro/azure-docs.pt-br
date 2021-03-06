---
title: Criar configuração de diagnóstico para coletar logs e métricas no Azure
description: Crie configurações de diagnóstico para encaminhar logs da plataforma Azure para logs de Azure Monitor, armazenamento do Azure ou hubs de eventos do Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 22932121b97c1b0fe91c46b5eea0222a022a4e61
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75751076"
---
# <a name="create-diagnostic-setting-to-collect-platform-logs-and-metrics-in-azure"></a>Criar configuração de diagnóstico para coletar logs e métricas de plataforma no Azure
[Os logs de plataforma](platform-logs-overview.md) no Azure, incluindo logs de recursos e log de atividades do Azure, fornecem informações detalhadas de diagnóstico e auditoria para recursos do Azure e a plataforma do Azure da qual dependem. Este artigo fornece detalhes sobre como criar e definir configurações de diagnóstico para enviar logs de plataforma para destinos diferentes.

> [!IMPORTANT]
> Antes de criar uma configuração de diagnóstico para coletar o log de atividades, primeiro você deve desabilitar qualquer configuração herdada. Consulte [coletar log de atividades do Azure com configurações herdadas](diagnostic-settings-legacy.md) para obter detalhes.

Cada recurso do Azure requer sua própria configuração de diagnóstico, que define o seguinte:

- As categorias de logs e os dados de métrica enviados aos destinos definidos na configuração. As categorias disponíveis variam para diferentes tipos de recursos.
- Um ou mais destinos para enviar os logs. Os destinos atuais incluem workspace do Log Analytics, Hubs de Eventos e Armazenamento do Azure.
 
Uma única configuração de diagnóstico pode definir, no máximo, um de cada um dos destinos. Se você quiser enviar dados para mais de um tipo de destino específico (por exemplo, dois workspaces do Log Analytics diferentes), crie várias configurações. Cada recurso pode ter até cinco configurações de diagnóstico.


> [!NOTE]
> As [métricas de plataforma](metrics-supported.md) são coletadas automaticamente para [Azure monitor métricas](data-platform-metrics.md). As configurações de diagnóstico podem ser usadas para coletar métricas para determinados serviços do Azure em Azure Monitor logs para análise com outros dados de monitoramento usando [consultas de log](../log-query/log-query-overview.md).

## <a name="destinations"></a>Destinos 
Os logs de plataforma podem ser enviados para os destinos na tabela a seguir. A configuração de cada destino é executada usando o mesmo processo para a criação de configurações de diagnóstico descritas neste artigo. Siga cada link na tabela a seguir para obter detalhes sobre como enviar dados para esse destino.

| Destino | Description |
|:---|:---|
| [Espaço de Trabalho do Log Analytics](resource-logs-collect-workspace.md) | A coleta de logs em um Log Analytics espaço de trabalho permite analisá-los com outros dados de monitoramento coletados por Azure Monitor usando consultas de log poderosas e também para aproveitar outros recursos de Azure Monitor, como alertas e visualizações. |
| [Hubs de Eventos](resource-logs-stream-event-hubs.md) | O envio de logs para hubs de eventos permite que você transmita dados para sistemas externos, como SIEMs de terceiros e outras soluções do log Analytics. |
| [Conta de armazenamento do Azure](resource-logs-collect-storage.md) | O arquivamento de logs em uma conta de armazenamento do Azure é útil para auditoria, análise estática ou backup. |

## <a name="create-diagnostic-settings-in-azure-portal"></a>Criar configurações de diagnóstico no portal do Azure
Você pode definir as configurações de diagnóstico no portal do Azure no menu Azure Monitor ou no menu do recurso.

1. Onde você define as configurações de diagnóstico no portal do Azure depende do recurso.

   - Para um único recurso, clique em **configurações de diagnóstico** em **Monitor** no menu do recurso.

        ![Configurações de Diagnóstico](media/diagnostic-settings/menu-resource.png)

    - Para um ou mais recursos, clique em **configurações de diagnóstico** em **configurações** no menu Azure monitor e clique no recurso.
    
        ![Configurações de Diagnóstico](media/diagnostic-settings/menu-monitor.png)

    - Para o log de atividades, clique em **log de atividades** no menu **Azure monitor** e, em seguida, **configurações de diagnóstico**. Certifique-se de desabilitar qualquer configuração herdada para o log de atividades. Consulte [desabilitar configurações existentes](diagnostic-settings-legacy.md#disable-existing-settings) para obter detalhes.

        ![Configurações de Diagnóstico](media/diagnostic-settings/menu-activity-log.png)

2. Se nenhuma configuração existir no recurso que você selecionou, será solicitada a criação de uma configuração. Clique em **Adicionar configuração de diagnóstico**.

   ![Adicionar configuração de diagnóstico - nenhuma configuração existente](media/diagnostic-settings/add-setting.png)

   Se houver configurações existentes no recurso, você verá uma lista de configurações já configuradas. Clique em **Adicionar configuração de diagnóstico** para adicionar uma nova configuração ou **Editar configuração** para editar uma existente. Cada configuração não pode ter mais de um de cada um dos tipos de destino.

   ![Adicionar configuração de diagnóstico - configurações existentes](media/diagnostic-settings/edit-setting.png)

3. Dê um nome à sua configuração se ela ainda não tiver uma.
4. Marque a caixa para cada destino para enviar os logs. Clique em **Configurar** para especificar suas configurações, conforme descrito na tabela a seguir.

    | Configuração | Description |
    |:---|:---|
    | Espaço de trabalho do Log Analytics | Nome do espaço de trabalho. |
    | Conta de armazenamento | Nome da conta de armazenamento. |
    | Namespace do Hub de Eventos | O namespace em que o Hub de eventos é criado (se esta for a primeira vez que os logs de streaming) ou transmitido (se já houver recursos que estão transmitindo essa categoria de log para esse namespace).
    | Nome do Hub de Eventos | Opcionalmente, especifique um nome de Hub de eventos para enviar todos os dados na configuração. Se você não especificar um nome, um hub de eventos será criado para cada categoria de log. Se você estiver enviando várias categorias, talvez queira especificar um nome para limitar o número de hubs de eventos criados. Consulte [limites e cotas dos hubs de eventos do Azure](../../event-hubs/event-hubs-quotas.md) para obter detalhes. |
    | Nome da política do hub de eventos | Define as permissões que o mecanismo de streaming tem. |

    ![Adicionar configuração de diagnóstico - configurações existentes](media/diagnostic-settings/setting-details.png)

5. Marque a caixa para cada uma das categorias de dados a serem enviadas aos destinos especificados. A lista de categorias irá variar para cada serviço do Azure.

   > [!NOTE]
   > Atualmente, não há suporte para o envio da métrica multidimensional por meio das configurações de diagnóstico. As métricas com dimensões são exportadas como métricas dimensionais simples, agregadas nos valores da dimensão.
   >
   > *Por exemplo*: a métrica “Mensagens de Entrada” em um Hub de Eventos pode ser explorada e mapeada por nível da fila. No entanto, quando exportada por meio das configurações de diagnóstico, a métrica será representada como todas as mensagens de entrada em todas as filas no Hub de Eventos.

6. Clique em **Save** (Salvar).

Após alguns instantes, a nova configuração aparecerá na lista de configurações desse recurso e os logs serão transmitidos para os destinos especificados à medida que novos dados de evento forem gerados. Observe que pode haver até quinze minutos entre o momento em que um evento é emitido e quando ele [aparece em um espaço de trabalho log Analytics](data-ingestion-time.md).



## <a name="create-diagnostic-settings-using-powershell"></a>Criar configurações de diagnóstico usando o PowerShell
Use o cmdlet [set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) para criar uma configuração de diagnóstico com [Azure PowerShell](powershell-quickstart-samples.md). Consulte a documentação deste cmdlet para obter descrições de seus parâmetros.

> [!IMPORTANT]
> Você não pode usar esse método para o log de atividades do Azure. Em vez disso, use a [configuração criar diagnóstico no Azure monitor usando um modelo do Resource Manager](diagnostic-settings-template.md) para criar um modelo do Resource Manager e implantá-lo com o PowerShell.

Veja a seguir um exemplo de cmdlet do PowerShell para criar uma configuração de diagnóstico usando todos os três destinos.


```powershell
Set-AzDiagnosticSetting -Name KeyVault-Diagnostics -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault -Category AuditEvent -MetricCategory AllMetrics -Enabled $true -StorageAccountId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount -WorkspaceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace  -EventHubAuthorizationRuleId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```


## <a name="create-diagnostic-settings-using-azure-cli"></a>Criar configurações de diagnóstico usando CLI do Azure
Use o comando [AZ monitor Diagnostics-Settings Create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) para criar uma configuração de diagnóstico com [CLI do Azure](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest). Consulte a documentação deste comando para obter descrições de seus parâmetros.

> [!IMPORTANT]
> Você não pode usar esse método para o log de atividades do Azure. Em vez disso, use a [configuração criar diagnóstico no Azure monitor usando um modelo do Resource Manager](diagnostic-settings-template.md) para criar um modelo do Resource Manager e implantá-lo com a CLI.

Veja a seguir um exemplo de comando da CLI para criar uma configuração de diagnóstico usando todos os três destinos.



```azurecli
az monitor diagnostic-settings create  \
--name KeyVault-Diagnostics \
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault \
--logs    '[{"category": "AuditEvent","enabled": true}]' \
--metrics '[{"category": "AllMetrics","enabled": true}]' \
--storage-account /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace \
--event-hub-rule /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

### <a name="configure-diagnostic-settings-using-rest-api"></a>Definir configurações de diagnóstico usando a API REST
Consulte [configurações de diagnóstico](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) para criar ou atualizar as configurações de diagnóstico usando a [API REST do Azure monitor](https://docs.microsoft.com/rest/api/monitor/).


### <a name="configure-diagnostic-settings-using-resource-manager-template"></a>Definir configurações de diagnóstico usando o modelo do Resource Manager
Consulte [criar configuração de diagnóstico no Azure monitor usando um modelo do Resource Manager](diagnostic-settings-template.md) para criar ou atualizar configurações de diagnóstico com um modelo do Resource Manager.

## <a name="next-steps"></a>Próximos passos

* [Leia mais sobre os logs da plataforma Azure](platform-logs-overview.md)
