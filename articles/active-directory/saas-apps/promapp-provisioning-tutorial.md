---
title: 'Tutorial: configurar o Promapp para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar Azure Active Directory para provisionar e desprovisionar automaticamente contas de usuário para o Promapp.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 41190ba0-9032-4725-9d2c-b3dd9c7786e6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2019
ms.author: Zhchia
ms.openlocfilehash: 3dd7786b4a976e11b263d89b66cbe5c34285606a
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74535230"
---
# <a name="tutorial-configure-promapp-for-automatic-user-provisioning"></a>Tutorial: configurar o Promapp para o provisionamento automático de usuário

O objetivo deste tutorial é demonstrar as etapas a serem executadas no Promapp e no Azure Active Directory (Azure AD) para configurar o Azure AD para provisionar e desprovisionar automaticamente usuários e/ou grupos no Promapp.

> [!NOTE]
> Este tutorial descreve um conector compilado na parte superior do Serviço de Provisionamento de Usuário do Microsoft Azure AD. Para detalhes importantes sobre o que esse serviço faz, como funciona e as perguntas frequentes, consulte [Automatizar o provisionamento e desprovisionamento de usuários para aplicativos SaaS com o Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Atualmente, esse conector está em versão prévia pública. Para obter mais informações sobre os Termos de uso gerais do Microsoft Azure para a versão prévia de recursos, confira [Termos de uso adicionais para versões prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes pré-requisitos:

* Um locatário do Azure AD
* [Um locatário do Promapp](https://www.promapp.com/licensing/)
* Uma conta de usuário no Promapp com permissões de administrador.

## <a name="assigning-users-to-promapp"></a>Atribuindo usuários ao Promapp

Azure Active Directory usa um conceito chamado *atribuições* para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de usuário, somente os usuários e/ou grupos que foram atribuídos a um aplicativo no Azure AD são sincronizados.

Antes de configurar e habilitar o provisionamento automático de usuário, você deve decidir quais usuários e/ou grupos no Azure AD precisam de acesso ao Promapp. Depois de decidir, você pode atribuir esses usuários e/ou grupos ao Promapp seguindo as instruções aqui:
* [Atribuir um usuário ou um grupo a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-promapp"></a>Dicas importantes para atribuir usuários ao Promapp

* É recomendável que um único usuário do Azure AD seja atribuído ao Promapp para testar a configuração automática de provisionamento de usuário. Outros usuários e/ou grupos podem ser atribuídos mais tarde.

* Ao atribuir um usuário ao Promapp, você deve selecionar qualquer função específica do aplicativo válida (se disponível) na caixa de diálogo de atribuição. Usuários com a função **Acesso padrão** são excluídos do provisionamento.

## <a name="setup-promapp-for-provisioning"></a>Configurar o Promapp para provisionamento

1. Entre no console do [administrador do Promapp](https://freetrial.promapp.com/axelerate/Login.aspx). Sob o nome de usuário, navegue até **meu perfil**.

    ![Console de administração do Promapp](media/promapp-provisioning-tutorial/admin.png)

2.  Em **tokens de acesso** , clique no botão **criar token** .

    ![Promapp adicionar SCIM](media/promapp-provisioning-tutorial/addtoken.png)

3.  Forneça qualquer nome no campo **Descrição** e selecione **scim** no menu suspenso **escopo** . Clique no ícone salvar.

    ![Promapp adicionar nome](media/promapp-provisioning-tutorial/addname.png)

4.  Copie o token de acesso e salve-o, pois essa será a única vez que você pode exibi-lo. Esse valor será inserido no campo token secreto na guia provisionamento do seu aplicativo Promapp no portal do Azure.

    ![Promapp criar token](media/promapp-provisioning-tutorial/token.png)

## <a name="add-promapp-from-the-gallery"></a>Adicionar o Promapp da galeria

Antes de configurar o Promapp para o provisionamento automático de usuário com o Azure AD, você precisará adicionar o Promapp da Galeria de aplicativos do Azure AD à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Promapp da Galeria de aplicativos do Azure AD, execute as seguintes etapas:**

1. No **[portal do Azure](https://portal.azure.com)** , no painel de navegação à esquerda, selecione **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um novo aplicativo, selecione o botão **novo aplicativo** na parte superior do painel.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, insira **Promapp**, selecione **Promapp** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Promapp na lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-promapp"></a>Configurando o provisionamento automático de usuário para o Promapp 

Esta seção orienta você pelas etapas para configurar o serviço de provisionamento do Azure AD para criar, atualizar e desabilitar usuários e/ou grupos no Promapp com base em atribuições de usuário e/ou grupo no Azure AD.

> [!TIP]
> Você também pode optar por habilitar o logon único baseado em SAML para o Promapp seguindo as instruções fornecidas no tutorial de [logon único do Promapp](https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-tutorial). O logon único pode ser configurado independentemente do provisionamento automático de usuário, embora esses dois recursos se complementem.

### <a name="to-configure-automatic-user-provisioning-for-promapp-in-azure-ad"></a>Para configurar o provisionamento automático de usuário para Promapp no Azure AD:

1. Entre no [portal do Azure](https://portal.azure.com). Selecione **aplicativos empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Promapp**.

    ![O link do Promapp na lista de Aplicativos](common/all-applications.png)

3. Selecione a guia **Provisionamento**.

    ![Guia provisionamento](common/provisioning.png)

4. Defina o **Modo de Provisionamento** como **Automático**.

    ![Guia provisionamento](common/provisioning-automatic.png)

5. Na seção **credenciais de administrador** , insira `https://api.promapp.com/api/scim` na **URL do locatário**. Insira o valor do **token de autenticação scim** recuperado anteriormente no **token secreto**. Clique em **testar conexão** para garantir que o Azure ad possa se conectar ao Promapp. Se a conexão falhar, verifique se sua conta do Promapp tem permissões de administrador e tente novamente.

    ![URL do locatário + token](common/provisioning-testconnection-tenanturltoken.png)

6. No campo **Notificação por Email**, insira o endereço de email de uma pessoa ou grupo que deverá receber as notificações de erro de provisionamento e selecione a caixa de seleção **Enviar uma notificação por email quando ocorrer uma falha**.

    ![Email de notificação](common/provisioning-notification-email.png)

7. Clique em **Save** (Salvar).

8. Na seção **mapeamentos** , selecione **sincronizar Azure Active Directory usuários para Promapp**.

    ![Mapeamentos de usuário Promapp](media/promapp-provisioning-tutorial/usermappings.png)

9. Examine os atributos de usuário que são sincronizados do Azure AD para o Promapp na seção **mapeamento de atributos** . Os atributos selecionados como propriedades **correspondentes** são usados para corresponder as contas de usuário no Promapp para operações de atualização. Selecione o botão **Salvar** para confirmar as alterações.

    ![Atributos de usuário do Promapp](media/promapp-provisioning-tutorial/userattributes.png)

11. Para configurar filtros de escopo, consulte as seguintes instruções fornecidas no [tutorial do Filtro de Escopo](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Para habilitar o serviço de provisionamento do Azure AD para o Promapp, altere o **status de provisionamento** para **ativado** na seção **configurações** .

    ![Status do provisionamento ativado](common/provisioning-toggle-on.png)

13. Defina os usuários e/ou grupos que você deseja provisionar para o Promapp escolhendo os valores desejados no **escopo** na seção **configurações** .

    ![Escopo de provisionamento](common/provisioning-scope.png)

14. Quando estiver pronto para provisionar, clique em **Salvar**.

    ![Salvando a configuração de provisionamento](common/provisioning-configuration-save.png)

Essa operação inicia a sincronização inicial de todos os usuários e/ou grupos definidos no **Escopo** na seção **Configurações**. Observe que a sincronização inicial levará mais tempo do que as sincronizações subsequentes, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço de provisionamento do Microsoft Azure Active Directory esteja em execução. Você pode usar a seção **detalhes de sincronização** para monitorar o progresso e seguir os links para o relatório de atividade de provisionamento, que descreve todas as ações executadas pelo serviço de provisionamento do Azure AD no Promapp.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciamento do provisionamento de conta de usuário para Aplicativos Empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Próximos passos

* [Saiba como fazer revisão de logs e obter relatórios sobre atividade de provisionamento](../manage-apps/check-status-user-account-provisioning.md)

