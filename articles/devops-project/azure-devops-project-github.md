---
title: 'Tutorial: Criar um pipeline de CI/CD para seu código existente usando o Azure DevOps Projects'
description: O Azure DevOps Projects facilita o uso inicial do Azure. Em algumas etapas simples, o DevOps Projects ajuda você a usar seu próprio código e o repositório do GitHub para iniciar um aplicativo em um serviço do Azure.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/27/2019
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: a0496999a9a5dc3e9bfd57df0ec035e6db77d620
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615122"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Tutorial: Criar um pipeline de CI/CD para seu código existente usando o Azure DevOps Projects

O Azure DevOps Projects apresenta um processo simplificado para criar um pipeline de CI (integração contínua) e de CD (entrega contínua) para o Azure. Você pode trazer seu código existente e o repositório Git ou selecionar um aplicativo de exemplo.

Você vai:

> [!div class="checklist"]
> * Use o DevOps Projects para criar um pipeline de CI/CD
> * Configurar o acesso ao seu repositório do GitHub e escolher uma estrutura
> * Configurar o Azure DevOps e uma assinatura do Azure 
> * Confirmar alterações no GitHub e implantá-las automaticamente no Azure
> * Examinar o pipeline de CI/CD do Azure Pipelines
> * Limpar recursos

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Você pode obter uma gratuita por meio do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).
* Acesso ao GitHub ou a um repositório Git externo que contenha .NET, Java, PHP, Node.js, Python ou código da Web estático.

## <a name="sign-in-to-the-azure-portal"></a>Entre no Portal do Azure

O Azure DevOps Projects cria um pipeline de CI/CD no Azure Pipelines. É possível criar uma nova organização do Azure DevOps ou usar uma existente. O Azure DevOps Projects também cria recursos do Azure na assinatura do Azure de sua escolha.

1. Entre no [Portal do Azure](https://portal.azure.com).

2. No menu do portal do Azure, selecione **Criar um recurso**.

   ![Menu do portal do Azure — criar um recurso](_img/azure-devops-project-github/createaresource.png)

3. Selecione **DevOps** > **DevOps Projects**.

   ![O painel do DevOps Projects](_img/azure-devops-project-github/azuredashboard.png)

1. Selecione **Traga seu próprio código**, depois selecione **Próxima**.

## <a name="configure-access-to-your-github-repo-and-select-a-framework"></a>Configurar o acesso ao seu repositório do GitHub e selecionar uma estrutura

1. Selecione o **GitHub** ou um repositório de código **Git** externo. Para este tutorial, selecione **GitHub**. Talvez seja necessário autenticar-se no GitHub pela primeira vez para permitir que o Azure acesse seu repositório do GitHub.

1. Selecione um **Repositório** e uma **Ramificação** e, em seguida, selecione **Próximo**.

1. Se você estiver usando contêineres do Docker, altere **É aplicativo convertido para Docker** para **SIM**. Para este tutorial, deixe **NÃO** selecionado e, em seguida, selecione **Próximo**. Para obter mais informações sobre como usar os contêineres do Docker, passe o mouse sobre o ícone **i**.

   ![Seleção da estrutura do aplicativo no menu suspenso](_img/azure-devops-project-github/appframework.png)

1. Nos menus suspensos, selecione um **runtime de aplicativo** e uma **estrutura de aplicativo** e, em seguida, selecione **Próximo**. A estrutura do aplicativo determinará o tipo de destino de implantação do serviço do Azure disponível.

1. Selecione um **Serviço do Azure** para implantar o aplicativo e, em seguida, selecione **Próximo**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configurar o Azure DevOps e uma assinatura do Azure

1. Insira um nome em **Nome do projeto**.

1. Crie uma nova organização gratuita na **Organização do Azure DevOps** ou selecione uma existente no menu suspenso.

1. Selecione sua assinatura em **Assinatura do Azure** e insira um nome em **aplicativo Web** ou use o padrão. Selecione uma **Localização** e, em seguida, selecione **Concluído**. Após alguns minutos, a visão geral de implantação do DevOps Projects é exibida no portal do Azure.

1. Selecione **Ir para o recurso** para exibir o painel do DevOps Projects. No canto superior direito, fixe o **Projeto** ao seu painel para permitir acesso rápido. O Azure DevOps Projects configura automaticamente um build de CI e gatilho de liberação. Seu código permanece em seu repositório do GitHub ou em outro repositório externo e um aplicativo de exemplo é configurado em um repositório na **Organização do Azure DevOps**. O Azure DevOps Projects executa o build e implanta o aplicativo no Azure.

   ![Exibição do painel do Azure DevOps Projects](_img/azure-devops-project-github/projectsdashboard.png)

1. O painel mostra o repositório de código, o pipeline de CI/CD e o seu aplicativo no Azure. À direita, em Recursos do Azure, selecione **Procurar** para exibir o aplicativo em execução.

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Confirmar alterações no GitHub e implantá-las automaticamente no Azure

Agora você está pronto para colaborar em seu aplicativo com uma equipe. O processo de CI/CD implanta automaticamente seu trabalho mais recente no site. Cada alteração no repositório do GitHub inicia um build no Azure DevOps e um pipeline de CD executa uma implantação no Azure.

1. No painel do DevOps Projects, selecione **Repositórios**. O repositório do GitHub é aberto em uma nova guia do navegador. Faça uma alteração em seu aplicativo e, em seguida, selecione **Confirmar alterações**.

1. Depois de um tempo, um build inicia no Azure Pipelines. Você pode monitorar o status do build no painel do DevOps Projects. Você também pode monitorá-lo em sua organização do Azure DevOps selecionando a guia **Pipelines de Build** no painel do DevOps Projects.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Examinar o pipeline de CI/CD do Azure Pipelines

O Azure DevOps Projects configura automaticamente um pipeline de CI/CD no Azure Pipelines. Explore e personalize o pipeline conforme necessário. Para se familiarizar com os pipelines de build e de lançamento, faça o seguinte:

1. No painel do DevOps Projects, selecione **Pipelines de Build**.

1. Depois que a página do **Azure Pipelines** for aberta, você verá um histórico dos builds mais recentes e o status de cada build.

   ![Página de builds do Azure Pipelines](_img/azure-devops-project-github/pipelinesbuildpage.png)

1. No canto superior direito da página **Builds**, você poderá selecionar **Editar** para alterar o build atual, **Fila** para adicionar um novo build ou o botão vertical de reticências ( **&#8942;** ) para abrir um menu com mais opções. Selecione **Editar**.

1. O build realiza várias tarefas, como efetuar buscas das fontes no repositório, restaurar dependências e publicar as saídas das implantações. À direita, em **Nome**, altere o nome do pipeline de build para algo mais descritivo. Selecione **Salvar e colocar na fila** e, em seguida, selecione **Salvar**. Insira um comentário e, em seguida, selecione **Salvar** novamente.

   ![Página de builds do Azure DevOps](_img/azure-devops-project-github/buildpage.png)

1. Para ver uma trilha de auditoria das alterações recentes do build, selecione a guia **Histórico**.  O Azure DevOps rastreia as alterações feitas no pipeline de build e permite que você compare as versões.

1. Selecione a guia **Gatilhos**. O Azure DevOps Projects cria automaticamente um gatilho de CI com algumas configurações padrão. Você pode definir gatilhos como **Habilitar a integração contínua** para executar um build cada vez que você confirmar uma alteração de código. Você também pode definir gatilhos para agendar builds a serem executados em horários específicos.

## <a name="clean-up-resources"></a>Limpar recursos

Quando o Serviço de Aplicativo do Azure e os recursos relacionados criados neste tutorial não forem mais necessários, você poderá excluí-los. Use a funcionalidade **Excluir** no painel do DevOps Projects.

## <a name="next-steps"></a>Próximas etapas

Quando você configurou seu processo de CI/CD neste tutorial, um build e um pipeline de lançamento foram criados automaticamente no Azure DevOps Projects. Você pode modificar esses pipelines de build e de lançamento para atender às necessidades de sua equipe.

Para saber mais sobre o pipeline de CI/CD, confira:

> [!div class="nextstepaction"]
> [Definir seu pipeline de CD (implantação contínua) de vários estágios](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

Para saber mais sobre o monitoramento de aplicativos, confira:
  
 > [!div class="nextstepaction"]
 > [O que é o Azure Monitor?](https://docs.microsoft.com/azure/azure-monitor/overview)
