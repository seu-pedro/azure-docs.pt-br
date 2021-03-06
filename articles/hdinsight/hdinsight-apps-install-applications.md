---
title: Instalar aplicativos de terceiros no HDInsight do Azure
description: Saiba como instalar aplicativos de Apache Hadoop de terceiros no Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: hrasheed
ms.openlocfilehash: 68bbe22a4a6f7697c2bfab291412aae85efc32e9
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70916992"
---
# <a name="install-third-party-apache-hadoop-applications-on-azure-hdinsight"></a>Instalar aplicativos Apache Hadoop de terceiros no Azure HDInsight

Saiba como instalar um aplicativo do [Apache Hadoop](https://hadoop.apache.org/) de terceiros no Azure HDInsight. Para obter instruções sobre como instalar seu próprio aplicativo, confira [Instalar aplicativos personalizados do HDInsight](hdinsight-apps-install-custom-applications.md).

Um aplicativo do HDInsight é um aplicativo que os usuários podem instalar em um cluster do HDInsight. Esses aplicativos podem ser desenvolvidos pela Microsoft, por ISVs (fornecedores independentes de software) ou por conta própria.  

A lista a seguir mostra os aplicativos publicados:

|Aplicativo |Tipo (s) de cluster | Descrição |
|---|---|---|
|[Plataforma AtScale Intelligence](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/atscale.atscale) |Hadoop |O AtScale transforma seu cluster HDInsight em um servidor OLAP de expansão, permitindo que você consulte bilhões de linhas de dados interativamente usando as ferramentas de BI que você já conhece, tem e adora – do Microsoft Excel, Power BI, tableau software para QlikView. |
|[CDAP para HDInsight](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/cask.cdap-for-hdinsight) |HBase |CDAP é a primeira plataforma de integração unificada para Big Data que acelera o tempo de implantação para o Hadoop e permite que ele forneça dados de autoatendimento. Software livre e extensível, o CDAP remove as barreiras para a inovação. Requisitos: 4 nós de região, min D3 v2. |
|[Datameer](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/datameer.datameer) |Hadoop |A plataforma escalonável de autoatendimento do Datameer para preparar, explorar e governar seus dados para análise acelera a transformação de dados de várias fontes complexos em informações valiosas prontas para os negócios, fornecendo insights mais rápidos e inteligentes em escala empresarial. |
|[Dataiku DSS no HDInsight](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/dataiku.dss-on-hdi) |Hadoop, Spark |Dataiku DSS em uma plataforma de ciência de dados corporativos que permite que cientistas de dados e analistas de dados colaborem para projetar e executar novos produtos de dados e serviços com mais eficiência, transformando dados brutos em previsões impactantes. |
|[Aplicativo WANdisco Fusion HDI](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/wandisco.fusion-hdi-app) |Hadoop, Spark, HBase, Storm, Kafka |Manter os dados consistentes em um ambiente distribuído é um grande desafio de operações de dados. O WANdisco Fusion, uma plataforma de software de classe empresarial, resolve esse problema habilitando a consistência de dados não estruturados em qualquer ambiente. |
|[H2O SparklingWater para HDInsight](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/h2o-ai.h2o-sparklingwater) |Spark |A água do H2O sparkling dá suporte aos seguintes algoritmos distribuídos: GLM, Bayes ingênua, floresta aleatória distribuída, máquina de aumento de gradiente, redes neurais profundas, aprendizado profundo, K-means, PCA, modelos de classificação baixa generalizados, detecção de anomalias, autocodificadores. |
|[Striim para integração de dados em tempo real ao HDInsight](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/striim.hdinsightintegration) |Hadoop, HBase, Storm, Spark, Kafka |Striim (pronuncia-se "Stream") é uma plataforma de integração de dados de streaming + inteligência de ponta a ponta, permitindo ingestão contínua, processamento e análise de fluxos de dados diferentes. |
|[Jumbune Enterprise – acelerando o BigData Analytics](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/impetus-infotech-india-pvt-ltd.impetus_jumbune) |Hadoop, Spark |Em um alto nível, o Jumbune auxilia empresas pela, 1. Acelerando o tez, o MapReduce & o hive baseado no mecanismo Spark, o Java, o desempenho da carga de trabalho escalar. 2. Monitoramento proativo do cluster Hadoop, 3. Estabelecendo o gerenciamento de qualidade de dados no sistema de arquivos distribuído. |
|[Kyligence Enterprise](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/kyligence.kyligence) |Hadoop,HBase,Spark |Da plataforma Apache Kylin, o Kyligence Enterprise permite BI em Big Data. Como um mecanismo OLAP corporativo no Hadoop, o Kyligence Enterprise capacita o analista de negócios a arquitetar BI no Hadoop com metodologia de data warehouse e BI padrão do setor. |
|[Explosão de presto para o Azure HDInsight](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/starburst.starburst-presto) |Hadoop |Presto é um mecanismo de consulta SQL distribuído rápido e escalonável. Arquitetado para a separação de armazenamento e computação, o Presto é perfeito para consultar dados no Azure Data Lake Storage, no Armazenamento de Blobs do Azure, em bancos de dados SQL e NoSQL e em outras fontes de dados. |
|[Coletor de dados do StreamSets para nuvem do HDInsight](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/streamsets.streamsets-data-collector-hdinsight) |Hadoop, HBase, Spark, Kafka |O coletor de dados StreamSets é um mecanismo leve e poderoso que transmite dados em tempo real. Use o coletor de dados para rotear e processar dados em seus fluxos de dados. Ele vem com uma licença de avaliação de 30 dias. |
|[Trifator Wrangler Enterprise](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/trifacta.tr01) |Hadoop, Spark, HBase |Trifator o Wrangler Enterprise para HDInsight dá suporte a Wrangling de dados de toda a empresa para qualquer escala de dados. O custo da execução de trifator no Azure é uma combinação dos custos de assinatura de trifator, além dos custos de infraestrutura do Azure para as máquinas virtuais. |
|[Plataforma de dados de Unifi](https://unifisoftware.com/platform/) |Hadoop,HBase,Storm,Spark |A plataforma de dados unificada é um pacote totalmente integrado de ferramentas de dados de autoatendimento projetadas para capacitar o usuário de negócios a lidar com os desafios de dados que impulsionam a receita incremental, reduzem os custos ou a complexidade operacional. |
|[Unraveldata APM](https://azuremarketplace.microsoft.com/marketplace/apps/unravel-data.unravel-app) |Spark |Aplicativo de dados Unravel para o cluster HDInsight Spark. |
|[Eswater o catálogo de dados controlado por ia](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/waterline_data.waterline_data) |Spark |Preponha catálogos, organiza e rege dados usando o ia para marcar automaticamente os dados com os termos comerciais. O catálogo didático de negócios da Waterline é um componente crítico e de sucesso para iniciativas de análise, conformidade e governança de autoatendimento e gerenciamento de TI. |

As instruções fornecidas neste artigo usam o portal do Azure. Também é possível exportar o modelo do Azure Resource Manager do portal ou obter uma cópia do modelo do Resource Manager de fornecedores e usar o Azure PowerShell e a CLI clássica do Azure para implantar o modelo.  Consulte [Criar clusters do Apache Hadoop no HDInsight usando modelos do Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Pré-requisitos
Se você quiser instalar aplicativos do HDInsight em um cluster HDInsight existente, precisará ter um cluster HDInsight. Para criar um, confira [Criar clusters](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). Você também pode instalar aplicativos do HDInsight quando cria um cluster HDInsight.

## <a name="install-applications-to-existing-clusters"></a>Instalar aplicativos em clusters existentes
O procedimento a seguir mostra como instalar aplicativos do HDInsight em um cluster HDInsight existente.

**Instalar um aplicativo do HDInsight**

1. Entre no [Portal do Azure](https://portal.azure.com).
2. No menu à esquerda, navegue até **todos os serviços** > **clusters HDInsight**de**análise** > .
3. Selecione um cluster HDInsight na lista.  Se não tiver um, você deverá criá-lo primeiro.  Confira [Criar clusters](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Na categoria **configurações** , selecione **aplicativos**. Você pode ver uma lista de aplicativos instalados na janela principal. 
   
    ![Menu do portal de aplicativos do HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Selecione **+ Adicionar** no menu. Você pode ver uma lista de aplicativos disponíveis.  Se **+ Add** estiver esmaecido, isso significará que não há nenhum aplicativo para esta versão do cluster HDInsight.
   
    ![Aplicativos disponíveis de aplicativos HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-list1.png)
6. Selecione um dos aplicativos disponíveis e siga as instruções para aceitar os termos legais.

Você pode ver o status da instalação nas notificações do portal (selecione o ícone de sino na parte superior do Portal). Depois que o aplicativo é instalado, ele é exibido na folha Aplicativos Instalados.

## <a name="install-applications-during-cluster-creation"></a>Instalar aplicativos durante a criação do cluster
Você tem a opção de instalar aplicativos do HDInsight ao criar um cluster. Durante o processo, os aplicativos do HDInsight são instalados depois que o cluster é criado e está no estado de execução. Para instalar aplicativos durante a criação do cluster usando o portal do Azure, use a opção **personalizado** em vez da opção **criação rápida** padrão.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Listar os aplicativos do HDInsight instalados e as propriedades
O portal mostra uma lista dos aplicativos do HDInsight instalados para um cluster e as propriedades de cada aplicativo instalado.

**Listar o aplicativo do HDInsight e exibir as propriedades**

1. Entre no [Portal do Azure](https://portal.azure.com).
2. No menu à esquerda, navegue até **todos os serviços** > **clusters HDInsight**de**análise** > .
3. Selecione um cluster HDInsight na lista.
4. Na categoria **configurações** , selecione **aplicativos**. Você pode ver uma lista de aplicativos instalados na janela principal. 
   
    ![Aplicativos instalados de aplicativos do HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Selecione um dos aplicativos instalados para mostrar a propriedade. A propriedades lista:

    |Propriedade | Descrição |
    |---|---|
    |Nome do aplicativo |Nome do aplicativo. |
    |Status |Status do aplicativo. |
    |Página da web |A URL do aplicativo Web que você implantou no nó de borda. A credencial é igual às credenciais de usuário HTTP que você configurou para o cluster. |
    |Ponto de extremidade SSH |Você pode usar o SSH para conectar-se ao nó de borda. As credenciais SSH são iguais às credenciais de usuário SSH que você configurou para o cluster. Para obter informações, consulte [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md). |
    |Descrição | Descrição do aplicativo. |

6. Para excluir um aplicativo, clique nele com o botão direito do mouse e clique em **Excluir** no menu de contexto.

## <a name="connect-to-the-edge-node"></a>Conectar-se ao nó de borda
Você pode se conectar ao nó de borda usando HTTP e SSH. As informações de ponto de extremidade podem ser encontradas no [portal](#list-installed-hdinsight-apps-and-properties). Para obter informações, consulte [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

As credenciais de ponto de extremidade HTTP são as credenciais de usuário HTTP que você configurou para o cluster HDInsight. As credenciais de ponto de extremidade do SSH são as credenciais SSH que você configurou para o cluster HDInsight.

## <a name="troubleshoot"></a>Solução de problemas
Confira [Solucionar problemas de instalação](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Próximas etapas
* [Instalar aplicativos personalizados do HDInsight](hdinsight-apps-install-custom-applications.md): saiba como implantar um aplicativo do HDInsight não publicado no HDInsight.
* [Publicar aplicativos HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar seus aplicativos personalizados do HDInsight no Azure Marketplace.
* [MSDN: Instalar um aplicativo HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicativos do HDInsight.
* [Personalizar clusters HDInsight baseados em Linux usando a Ação de Script](hdinsight-hadoop-customize-cluster-linux.md): saiba como usar a Ação de Script para instalar aplicativos adicionais.
* [Personalizar clusters Apache Hadoop baseados em Linux no HDInsight usando modelos do Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md): saiba como chamar modelos do Resource Manager para criar clusters HDInsight.
* [Usar nós de borda vazios no HDInsight](hdinsight-apps-use-edge-node.md): saiba como usar um nó de borda vazio para acessar o cluster HDInsight, testar e hospedar aplicativos HDInsight.

