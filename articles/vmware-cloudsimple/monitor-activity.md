---
title: Monitorar a atividade de nuvem privada
titleSuffix: Azure VMware Solution by CloudSimple
description: Descreve as informações disponíveis sobre a atividade na solução do Azure VMware por ambiente CloudSimple, incluindo alertas, eventos, tarefas e auditoria.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/13/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1228f3a54e02d4fe7a5133e2bfba55c38e34718a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75459209"
---
# <a name="monitor-vmware-solution-by-cloudsimple-activity"></a>Monitorar a solução VMware por atividade de CloudSimple

Os logs de atividade do CloudSimple fornecem uma percepção das operações realizadas no portal do CloudSimple.  A lista inclui alertas, eventos, tarefas e auditoria.  Use os logs de atividade para determinar quem, quando e quais operações foram executadas.  Os logs de atividades não incluem nenhuma operação de leitura feita por um usuário.

## <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Acessar o portal da CloudSimple

Acesse o [portal do CloudSimple](access-cloudsimple-portal.md).

## <a name="activity-information"></a>Informações da atividade

Para acessar as páginas de atividade, selecione **atividade** no menu lateral.

![Visão geral da página de atividade](media/activity-page-overview.png)

Para exibir detalhes sobre qualquer uma das atividades na página atividade, selecione a atividade. Um painel de detalhes é aberto à direita. As ações no painel dependem do tipo de atividade. Clique em **X** para fechar o painel.

Clique em um cabeçalho de coluna para classificar a exibição.  Você pode filtrar colunas para ver valores específicos.  Baixe o relatório de atividades clicando no ícone **baixar como CSV** .

## <a name="alerts"></a>Alertas

Alertas são notificações de qualquer atividade significativa em seu ambiente CloudSimple.  Os alertas incluem eventos que afetam a cobrança ou o acesso do usuário.

Para confirmar os alertas e removê-los da lista, selecione um ou mais na lista e clique em **reconhecer**.

As colunas de informações a seguir estão disponíveis para alertas. Clique em **Editar colunas** e selecione as colunas que você deseja exibir.

| Column | Description |
------------ | ------------- |
| Tipo de alerta | Categoria do alerta.|
| Tempo | Hora em que o alerta ocorreu. |
| Gravidade | Significância do alerta.|
| Nome do Recurso | Nome atribuído ao recurso, como o nome da nuvem privada. |
| Tipo de recurso | Categoria de recurso: nuvem privada, rack de nuvem. |
| ID de Recurso | Identificador do recurso. |
| Description | Descrição do que disparou o alerta. |
| Confirmado | Indica se o alerta é confirmado. |

## <a name="events"></a>Eventos

Eventos mostram a atividade de usuário e sistema no portal do CloudSimple. A página eventos lista a atividade associada a um recurso específico e a gravidade do impacto.

As colunas de informações a seguir estão disponíveis para alertas. Clique em **Editar colunas** e selecione as colunas que você deseja exibir.

| Column | Description |
------------ | ------------- |
| Tempo | Data e hora em que o evento ocorreu. |
| Tipo de evento | Código numérico que identifica o evento. |
| Gravidade | Severidade do evento.|
| Nome do Recurso | Nome atribuído ao recurso, como o nome da nuvem privada. |
| Tipo de recurso | Categoria de recurso: nuvem privada, rack de nuvem. |
| Description | Descrição do que disparou o alerta. |

## <a name="tasks"></a>Tarefas

As tarefas são atividades de nuvem privada que devem levar 30 segundos ou mais para serem concluídas. (As atividades que devem levar menos de 30 segundos são relatadas apenas como eventos.) Abra as páginas tarefas para acompanhar o progresso das tarefas de sua nuvem privada.

As colunas de informações a seguir estão disponíveis para alertas. Clique em **Editar colunas** e selecione as colunas que você deseja exibir.

| Column | Description |
------------ | ------------- |
| ID da Tarefa | Identificador exclusivo da tarefa. |
| Operação | Ação executada pela tarefa. |
| Usuário | Usuário atribuído para concluir a tarefa. |
| Nome do Recurso | Nome atribuído ao recurso. |
| Tipo de recurso | Categoria de recurso: nuvem privada, rack de nuvem. |
| ID de Recurso | Identificador do recurso. |
| Iniciar | Hora de início da tarefa. |
| Terminar | Hora de término da tarefa. |
| Status | Status da tarefa atual. |
| Tempo decorrido | Tempo que a tarefa levou para ser concluída (se estiver concluída) ou que está sendo executada (se em andamento). |
| Description | Descrição da tarefa. |

## <a name="audit"></a>Audit

Os logs de auditoria controlam a atividade do usuário. Você pode usar os logs de auditoria para monitorar a atividade do usuário para todos os usuários.

As colunas de informações a seguir estão disponíveis para alertas. Clique em **Editar colunas** e selecione as colunas que você deseja exibir.

| Column | Description |
------------ | ------------- |
| Tempo | Hora da entrada de auditoria. |
| Operação | Ação executada pela tarefa. |
| Usuário | Usuário atribuído à tarefa. |
| Nome do Recurso | Nome atribuído ao recurso. |
| Tipo de recurso | Categoria de recurso: nuvem privada, rack de nuvem. |
| ID de Recurso | Identificador do recurso. |
| Result | Resultado da atividade, como **êxito**. |
| Tempo Gasto | Tempo para concluir a tarefa. |
| Description | Descrição da ação. |

## <a name="next-steps"></a>Próximos passos

* [Consumir VMs VMware no Azure](quickstart-create-vmware-virtual-machine.md)
* Saiba mais sobre [nuvens privadas](cloudsimple-private-cloud.md)
