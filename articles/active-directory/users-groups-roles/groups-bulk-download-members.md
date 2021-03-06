---
title: Lista de associação de grupo de download em massa-portal de Azure Active Directory | Microsoft Docs
description: Adicione usuários em massa no centro de administração do Azure.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e29aacb1357509e2b000a9d05c5ced8f9a30dce
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517168"
---
# <a name="bulk-download-members-of-a-group-preview-in-azure-active-directory"></a>Baixar em massa membros de um grupo (versão prévia) no Azure Active Directory

Usando o portal do Azure Active Directory (AD do Azure), você pode fazer o download em massa dos membros de um grupo em sua organização para um arquivo CSV (valores separados por vírgula).

## <a name="to-bulk-download-group-membership"></a>Para baixar em massa a associação ao grupo

1. Entre no [portal do Azure](https://portal.azure.com) com uma conta de administrador de usuário na organização. Os proprietários do grupo também podem fazer o download em massa de membros de grupos que possuem.
1. No Azure AD, selecione **grupos**  > **todos os grupos**.
1. Abra o grupo cuja associação você deseja baixar e, em seguida, selecione **Membros**.
1. Na página **Membros** , selecione **baixar Membros** para baixar um arquivo CSV listando os membros do grupo.

   ![O comando baixar Membros está na página de perfil do grupo](./media/groups-bulk-download-members/download-panel.png)

## <a name="check-download-status"></a>Verificar status de download

Você pode ver o status de todas as suas solicitações em massa pendentes na página **resultados da operação em massa (versão prévia)** .

   ![A página resultados de operações em massa mostra o status de solicitação em massa](./media/groups-bulk-download-members/bulk-center.png)

## <a name="bulk-download-service-limits"></a>Limites do serviço de download em massa

Cada atividade em massa para baixar uma lista de membros do grupo pode ser executada por até uma hora. Isso permite que você baixe uma lista de pelo menos 500.000 membros.

## <a name="next-steps"></a>Próximos passos

- [Membros do grupo de importação em massa](groups-bulk-import-members.md)
- [Remover membros do grupo em massa](groups-bulk-download-members.md)
