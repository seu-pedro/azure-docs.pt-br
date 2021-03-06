---
title: Perguntas frequentes sobre o provisionamento em nuvem do Azure AD Connect
description: Este documento descreve as perguntas frequentes sobre o provisionamento de nuvem.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbaafdce6f2510b58966f4b6c18e45a3fcd4a664
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997081"
---
# <a name="azure-active-directory-connect-faq"></a>Perguntas frequentes do Azure Active Directory Connect

Leia sobre as perguntas frequentes sobre o provisionamento de nuvem do Azure AD (Azure Active Directory) Connect.

## <a name="general-installation"></a>Instalação geral

**P: Com que frequência o provisionamento de nuvem é executado?**

O provisionamento de nuvem está agendado para ser executado a cada 2 minutos. A cada 2 minutos, quaisquer alterações de usuário, de grupo e de hash de senha serão provisionadas para o Azure AD.

**P: Estou vendo falhas de sincronização de hash de senha na primeira execução. Por quê?**

Isso é esperado. As falhas são devido ao objeto de usuário não estar presente no Azure AD. Depois que o usuário for provisionado no Azure AD, os hashes de senha deverão ser provisionados na execução subsequente. Aguarde algumas execuções e confirme se a sincronização de hash de senha já deixou de apresentar os erros.

**P: Qual é a diferença entre a sincronização e o provisionamento de nuvem do Azure AD Connect?**

Com a sincronização do Azure AD Connect, o provisionamento é executado no servidor de sincronização local. A configuração é armazenada no servidor de sincronização local. Com o provisionamento de nuvem do Azure AD Connect, a configuração de provisionamento é armazenada na nuvem e é executada na nuvem como parte do serviço de provisionamento do Azure AD. 

**P: Posso usar o provisionamento de nuvem para sincronizar de várias florestas do Active Directory?**

Sim. O provisionamento de nuvem pode ser usado para sincronizar de várias florestas do Active Directory. No ambiente de várias florestas, todas as referências (por exemplo, gerente) precisam estar dentro do domínio.  

**P: Como o agente é atualizado?**

Os agentes são atualizados automaticamente pela Microsoft. Para a equipe de TI, isso reduz a carga de ter que testar e validar novas versões de agente. 

**P: Posso desabilitar a atualização automática?**

Não há nenhuma maneira compatível de desabilitar a atualização automática.

**P: Posso alterar a âncora de origem para o provisionamento de nuvem?**

Por padrão, o provisionamento de nuvem usa o ms-ds-consistency-GUID com um fallback para ObjectGUID como âncora de origem. Não há nenhuma maneira compatível de alterar a âncora de origem.

**P: Vejo novas entidades de serviço com os nomes de domínio do AD ao usar o provisionamento de nuvem. Isso é esperado?**

Sim, o provisionamento de nuvem cria uma entidade de serviço para a configuração de provisionamento com o nome de domínio como o nome da entidade de serviço. Não faça nenhuma alteração na configuração da entidade de serviço.

**P: O que acontece quando é exigido de um usuário sincronizado que ele altere a senha no próximo logon?**

Se a sincronização de hash de senha estiver habilitada no provisionamento de nuvem e for exigido do usuário sincronizado que ele altere a senha no próximo logon no AD local, o provisionamento de nuvem não provisionará o hash de senha a ser alterado para o Azure AD. Depois que o usuário altera a senha, o hash de senha do usuário é provisionado do AD para o Azure AD.

**P: O provisionamento de nuvem dá suporte a write-back de ms-ds-consistencyGUID para algum objeto?**

Não, o provisionamento de nuvem não dá suporte a write-back de ms-ds-consistencyGUID para nenhum objeto (incluindo objetos de usuário). 

**P: Estou provisionando usuários usando o provisionamento de nuvem. Eu excluí a configuração. Por que eu ainda vejo os antigos objetos sincronizados no Azure AD?** 

Quando você exclui a configuração, o provisionamento de nuvem não limpa os objetos sincronizados no Azure AD. Para garantir que você não fique com objetos antigos, altere o escopo da configuração para um grupo vazio ou unidades organizacionais. Depois que o provisionamento é executado e limpa os objetos, desabilite e exclua a configuração. 

## <a name="next-steps"></a>Próximas etapas 

- [O que é provisionamento?](what-is-provisioning.md)
- [O que é o provisionamento em nuvem do Azure AD Connect?](what-is-cloud-provisioning.md)
