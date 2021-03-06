---
title: Perguntas frequentes sobre o Azure HDInsight
description: Perguntas frequentes sobre o HDInsight
keywords: perguntas frequentes, FAQ
author: Ramakoni1
ms.author: ramakoni
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/20/2019
ms.openlocfilehash: b05b83086cc9d8449d9517897f347b6e2685aa95
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720343"
---
# <a name="azure-hdinsight-frequently-asked-questions"></a>Azure HDInsight: perguntas frequentes

Este artigo fornece respostas para algumas das perguntas mais comuns sobre como executar o [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="creating-or-deleting-hdinsight-clusters"></a>Criando ou excluindo clusters HDInsight

### <a name="how-do-i-provision-an-hdinsight-cluster"></a>Como fazer provisionar um cluster HDInsight?

Para examinar os tipos de clusters HDInsight disponíveis e os métodos de provisionamento, consulte [Configurar clusters no HDInsight com Apache Hadoop, Apache Spark, Apache Kafka e muito mais](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).

### <a name="how-do-i-delete-an-existing-hdinsight-cluster"></a>Como fazer excluir um cluster HDInsight existente?

Para saber mais sobre como excluir um cluster quando ele não estiver mais em uso, consulte [excluir um cluster HDInsight](hdinsight-delete-cluster.md).

Deixe pelo menos 30 a 60 minutos entre as operações de criação e exclusão. Caso contrário, a operação poderá falhar com a seguinte mensagem de erro:

``Conflict (HTTP Status Code: 409) error when attempting to delete a cluster immediately after creation of a cluster. If you encounter this error, wait until the newly created cluster is in operational state before attempting to delete it.``

### <a name="how-do-i-select-the-correct-number-of-cores-or-nodes-for-my-workload"></a>Como fazer selecionar o número correto de núcleos ou nós para minha carga de trabalho?

O número apropriado de núcleos e outras opções de configuração dependem de vários fatores.

Para obter mais informações, consulte [planejamento de capacidade para clusters HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning).

### <a name="what-can-i-do-when-cluster-provisioning-fails-because-of-a-capacity-issue"></a>O que posso fazer quando o provisionamento de cluster falhar devido a um problema de capacidade?

Os erros comuns de problema de capacidade e técnicas de mitigação são fornecidos nesta seção.

#### <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Erro: a implantação excederia a cota de ' 800 '

O Azure tem um limite de cota de 800 implantações por grupo de recursos. Cotas diferentes são aplicadas por grupo de recursos, assinatura, conta ou outros escopos. Por exemplo, sua assinatura pode estar configurada para limitar o número de núcleos de uma região. Se você tentar implantar uma máquina virtual que tenha mais núcleos do que a quantidade permitida, receberá uma mensagem de erro indicando que a cota foi excedida.

Para resolver esse problema, exclua as implantações que não são mais necessárias usando o portal do Azure, a CLI ou o PowerShell.

Para obter mais informações, consulte [Solucionar erros de cotas de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

#### <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Erro: o nó máximo excedeu os núcleos disponíveis nesta região

Sua assinatura pode estar configurada para limitar o número de núcleos de uma região. Se você tentar implantar um recurso que tenha mais núcleos do que o valor permitido, receberá uma mensagem de erro indicando que a cota foi excedida.

Para solicitar um aumento de cota, siga essas etapas:

1. Vá para a [portal do Azure](https://portal.azure.com)e selecione **ajuda + suporte**.
   
1. Selecione **Nova solicitação de suporte**.
   
1. Na guia **noções básicas** da página **nova solicitação de suporte** , forneça as seguintes informações:
   
   - **Tipo de problema:** Selecione **limites de serviço e assinatura (cotas)** .
   - **Assinatura:** Selecione a assinatura que você deseja modificar.
   - **Tipo de cota:** Selecione **HDInsight**.

Para obter mais informações, consulte [Criar um tíquete de suporte para aumentar o núcleo](hdinsight-capacity-planning.md#quotas).

### <a name="what-are-the-various-types-of-nodes-in-an-hdinsight-cluster"></a>Quais são os vários tipos de nós em um cluster HDInsight?

Os clusters do Azure HDInsight têm diferentes tipos de máquinas virtuais ou nós. Cada tipo de nó desempenha uma função na operação do sistema.

Para obter mais informações, consulte [tipos de recursos em clusters do Azure HDInsight](hdinsight-virtual-network-architecture.md#resource-types-in-azure-hdinsight-clusters).

## <a name="individual-components"></a>Componentes individuais

### <a name="can-i-install-additional-components-on-my-cluster"></a>Posso instalar componentes adicionais no meu cluster?

Sim. Para instalar componentes adicionais ou personalizar a configuração do cluster, use:

- Scripts durante ou após a criação. Os scripts são invocados por meio da [ação de script](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux), que é uma opção de configuração que pode ser usada no portal do Azure, nos cmdlets do Windows PowerShell do hdinsight ou no SDK do .net do hdinsight. Essa opção de configuração pode ser usada no portal do Azure, nos cmdlets do Windows PowerShell do HDInsight ou no SDK do .NET do HDInsight.

- [Plataforma de aplicativo HDInsight](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) para instalar aplicativos de ecossistema.

Para obter uma lista de componentes com suporte [, consulte Quais são os componentes Apache Hadoop e as versões disponíveis com o HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)

### <a name="can-i-upgrade-the-individual-components-that-are-pre-installed-on-the-cluster"></a>Posso atualizar os componentes individuais que estão pré-instalados no cluster?

Se você atualizar componentes internos ou aplicativos pré-instalados em seu cluster, a configuração resultante não será suportada pela Microsoft. Essas configurações do sistema não foram testadas pela Microsoft. Tente usar uma versão diferente do cluster HDInsight que já pode ter a versão atualizada do componente pré-instalada.

Por exemplo, não há suporte para a atualização do hive como um componente individual. O HDInsight é um serviço gerenciado, e muitos serviços são integrados ao servidor Ambari e testados. Atualizar um Hive por conta própria faz com que os binários indexados de outros componentes sejam alterados e causarão problemas de integração de componentes no cluster.

### <a name="can-spark-and-kafka-run-on-the-same-hdinsight-cluster"></a>O Spark e o Kafka podem ser executados no mesmo cluster HDInsight?

Não, não é possível executar Apache Kafka e Apache Spark no mesmo cluster HDInsight. Crie clusters separados para Kafka e Spark para evitar problemas de contenção de recursos.

### <a name="how-do-i-change-timezone-in-ambari"></a>Como fazer alterar o fuso horário em Ambari?

1. Abra a interface do usuário da Web do amAmbari em `https://CLUSTERNAME.azurehdinsight.net`, em que CLUSTERname é o nome do cluster.
2. No canto superior direito, selecione Admin | Configurações. 

   ![Configurações de Ambari](media/hdinsight-faq/ambari-settings.png)

3. Na janela Configurações do usuário, selecione o fuso horário novo na lista suspensa fuso horário e clique em salvar.

   ![Configurações de usuário do Ambari](media/hdinsight-faq/ambari-user-settings.png)

## <a name="metastore"></a>Metastore

### <a name="how-can-i-migrate-from-the-existing-metastore-to-azure-sql-server"></a>Como posso migrar do metastore existente para o Azure SQL Server? 

Para migrar de SQL Server para o SQL Server do Azure, consulte [tutorial: migrar SQL Server para um banco de dados individual ou banco de dados em pool no banco de dados SQL do Azure offline usando DMS](../dms/tutorial-sql-server-to-azure-sql.md).

### <a name="is-the-hive-metastore-deleted-when-the-cluster-is-deleted"></a>O metastore do Hive excluído quando o cluster é excluído?

Depende do tipo de metastore que o cluster está configurado para usar.

Para um metastore padrão: o metastore padrão faz parte do ciclo de vida do cluster. Quando você exclui um cluster, o metastore e os metadados correspondentes também são excluídos.

Para um metastore personalizado: o ciclo de vida do metastore não está vinculado ao ciclo de vida de um cluster. Portanto, você pode criar e excluir clusters sem perder metadados. Os metadados como os esquemas do hive persistem mesmo depois que você exclui e recria o cluster HDInsight.

Para obter mais informações, consulte [Usar armazenamentos de metadados externos no Azure HDInsight](hdinsight-use-external-metadata-stores.md).

### <a name="does-migrating-a-hive-metastore-also-migrate-the-default-policies-of-the-ranger-database"></a>A migração de um metastore do Hive também migra as políticas padrão do banco de dados do Ranger?

Não, a definição de política está no banco de dados do Ranger, portanto, migrar o banco de dados do Ranger migrará sua política.

### <a name="can-you-migrate-a-hive-metastore-from-an-enterprise-security-package-esp-cluster-to-a-non-esp-cluster-and-vice-versa"></a>É possível migrar um metastore do Hive de um cluster de Enterprise Security Package (ESP) para um cluster não ESP e vice-versa?

Sim, você pode migrar um metastore do Hive de um ESP para um cluster não ESP.

### <a name="how-can-i-estimate-the-size-of-a-hive-metastore-database"></a>Como posso estimar o tamanho de um banco de dados metastore do Hive?

Um metastore do Hive é usado para armazenar os metadados de fontes de dados que são usados pelo servidor do hive. Os requisitos de tamanho dependem parcialmente do número e da complexidade de suas fontes de dados do hive e não podem ser estimados antecipadamente. Conforme descrito em [metastore do hive práticas recomendadas](hdinsight-use-external-metadata-stores.md#hive-metastore-best-practices), você pode começar com uma camada S2, que fornece 50 DTU e 250 GB de armazenamento e, se você vir um afunilamento, poderá escalar verticalmente o banco de dados.

### <a name="do-you-support-any-other-database-other-than-azure-sql-database-as-an-external-metastore"></a>Há suporte para qualquer outro banco de dados que não seja o banco de dados SQL do Azure como um metastore externo?

Não, a Microsoft dá suporte apenas ao banco de dados SQL do Azure como um metastore personalizado externo.

### <a name="can-i-share-a-metastore-across-multiple-clusters"></a>Posso compartilhar um metastore em vários clusters?

Sim, você pode compartilhar o metastore personalizado em vários clusters, desde que eles estejam usando a mesma versão do HDInsight.

## <a name="connectivity-and-virtual-networks"></a>Conectividade e redes virtuais  

### <a name="what-are-the-implications-of-blocking-ports-22-and-23-on-my-network"></a>Quais são as implicações de bloquear as portas 22 e 23 em minha rede?

Se você bloquear as portas 22 e 23, não terá acesso SSH ao cluster. Essas portas não são usadas pelo serviço HDInsight.

Para obter mais informações, consulte um dos seguintes documentos:

- [Controlando o tráfego de rede](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic)

- [Proteger o tráfego de entrada para clusters HDInsight em uma rede virtual com ponto de extremidade privado](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)

- [Endereços IP de gerenciamento do HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)

### <a name="can-i-deploy-an-additional-virtual-machine-within-the-same-subnet-as-an-hdinsight-cluster"></a>Posso implantar uma máquina virtual adicional na mesma sub-rede que um cluster HDInsight?

Sim, você pode implantar uma máquina virtual adicional na mesma sub-rede que um cluster HDInsight. As seguintes configurações são possíveis:

- Nós de borda: você pode adicionar outro nó de borda ao cluster, conforme descrito em [usar nós de borda vazios em clusters de Apache Hadoop no HDInsight](hdinsight-apps-use-edge-node.md).

- Nós autônomos: você pode adicionar uma máquina virtual autônoma à mesma sub-rede e acessar o cluster dessa máquina virtual usando o ponto de extremidade privado `https://<CLUSTERNAME>-int.azurehdinsight.net`. Para obter mais informações, consulte [controlando o tráfego de rede](hdinsight-plan-virtual-network-deployment.md#networktraffic).

### <a name="should-i-store-data-on-the-local-disk-of-an-edge-node"></a>Devo armazenar dados no disco local de um nó de borda?

Não, não é uma boa ideia armazenar dados em um disco local. Se o nó falhar, todos os dados armazenados localmente serão perdidos. É recomendável armazenar dados no Azure Data Lake Storage Gen2 ou no armazenamento de BLOBs do Azure ou montando um compartilhamento de arquivos do Azure para armazenar os dados.


### <a name="can-i-add-an-existing-hdinsight-cluster-to-another-virtual-network"></a>Posso adicionar um cluster HDInsight existente a outra rede virtual?

Não, você não pode. A rede virtual deve ser especificada no momento do provisionamento. Se nenhuma rede virtual for especificada durante o provisionamento, a implantação criará uma rede interna que não pode ser acessada de fora. Para obter mais informações, consulte [Adicionar o HDInsight a uma rede virtual existente](hdinsight-plan-virtual-network-deployment.md#existingvnet).

## <a name="security-and-certificates"></a>Segurança e certificados

### <a name="what-are-the-recommendations-for-malware-protection-on-azure-hdinsight-clusters"></a>Quais são as recomendações para proteção contra malware em clusters do Azure HDInsight?

Para obter informações sobre proteção contra malware, consulte [Microsoft antimalware para serviços de nuvem do Azure e máquinas virtuais](../security/fundamentals/antimalware.md).

### <a name="how-do-i-create-a-keytab-for-an-hdinsight-esp-cluster"></a>Como fazer criar um keytab para um cluster de ESP do HDInsight?

Crie um Kerberos keytab para seu nome de usuário de domínio. Posteriormente, você pode usar esse keytab para autenticar em clusters remotos ingressados no domínio sem inserir uma senha. O nome de domínio está em letras maiúsculas:

```shell
ktutil
ktutil: addent -password -p <username>@<DOMAIN.COM> -k 1 -e RC4-HMAC
Password for <username>@<DOMAIN.COM>: <password>
ktutil: wkt <username>.keytab
ktutil: q
```

### <a name="can-i-use-an-existing-azure-active-directory-tenant-to-create-an-hdinsight-cluster-that-has-the-esp"></a>Posso usar um locatário de Azure Active Directory existente para criar um cluster HDInsight que tem o ESP?

Você deve habilitar Azure Active Directory Domain Services (AD DS do Azure) antes de criar um cluster HDInsight com o ESP. O Hadoop de software livre depende do Kerberos para autenticação (em oposição ao OAuth).

Para unir VMs a um domínio, você deve ter um controlador de domínio. O Azure AD DS é o controlador de domínio gerenciado e é considerado uma extensão de Azure Active Directory que fornece todos os requisitos de Kerberos para criar um cluster Hadoop seguro de uma maneira gerenciada. O HDInsight como um serviço gerenciado integra-se com o Azure AD DS para fornecer segurança de ponta a ponta.

### <a name="can-i-use-a-self-signed-certificate-in-an-aad-ds-secure-ldap-setup-and-provision-an-esp-cluster"></a>Posso usar um certificado autoassinado em uma instalação LDAP segura do AAD-DS e provisionar um cluster ESP?

É recomendável usar um certificado emitido por uma autoridade de certificação, mas também há suporte para o uso de um certificado autoassinado no ESP. Para obter mais informações, consulte:

- [Habilitar Azure Active Directory Domain Services](domain-joined/apache-domain-joined-configure-using-azure-adds.md#enable-azure-ad-ds)

- [Tutorial: configurar o LDAP seguro para um domínio gerenciado Azure Active Directory Domain Services](../active-directory-domain-services/tutorial-configure-ldaps.md)

### <a name="how-can-i-pull-login-activity-shown-in-ranger"></a>Como posso efetuar pull da atividade de logon mostrada no Ranger?

Para requisitos de auditoria, a Microsoft recomenda Habilitar logs de Azure Monitor conforme descrito em [usar logs de Azure monitor para monitorar clusters HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial).

### <a name="can-i-disable-clamscan-on-my-cluster"></a>Posso desabilitar o clamscan em meu cluster?

Clamscan é o software antivírus executado no cluster HDInsight e é usado pela azsecd (segurança do Azure) para proteger seus clusters contra ataques de vírus. A Microsoft recomenda enfaticamente que os usuários evitem fazer alterações na configuração padrão do clamscan.

Esse processo não interfere nem demora nenhum ciclo para outros processos. Ele sempre resultará em outro processo. Os picos de CPU de clamscan devem ser vistos somente quando o sistema estiver ocioso.  

Em cenários nos quais você deve controlar a agenda, você pode usar as seguintes etapas:

1. Desabilite a execução automática usando o seguinte comando:
   
   `/usr/local/vbin/azsecd config -s clamav -d Disabled`
   
1. Adicione um trabalho cron que execute o seguinte comando como raiz:
   
   `/usr/local/bin/azsecd manual -s clamav`

Para obter mais informações sobre como configurar e executar um trabalho cron, consulte [como fazer configurar um trabalho cron](https://askubuntu.com/questions/2368/how-do-i-set-up-a-cron-job)?

### <a name="why-is-llap-available-on-spark-esp-clusters"></a>Por que o LLAP está disponível em clusters de ESP do Spark?
Em clusters do Spark do ESP, o LLAP é habilitado por motivos de segurança (ou seja, o Apache Ranger), não pelo desempenho. Você deve usar VMs de nó maior para acomodar o uso de recursos de LLAP (por exemplo, D13V2 mínimo). 

### <a name="how-can-i-add-additional-aad-groups-after-creating-an-esp-cluster"></a>Como posso adicionar outros grupos do AAD depois de criar um cluster ESP?
Há duas maneiras de conseguir isso: 1-você pode recriar o cluster e adicionar o grupo adicional no momento da criação do cluster. Se você estiver usando a sincronização com escopo no AAD-DS, verifique se o grupo B está incluído na sincronização com escopo.
2-Adicione o grupo como um subgrupo aninhado do grupo anterior que foi usado para criar o cluster ESP. Por exemplo, se você tiver criado um cluster ESP com `A`de grupo, poderá mais tarde em Adicionar grupo `B` como um subgrupo aninhado de `A` e depois de aproximadamente uma hora ele será sincronizado e estará disponível no cluster automaticamente. 

## <a name="storage"></a>Armazenamento

### <a name="can-i-add-an-azure-data-lake-storage-gen2-to-an-existing-hdinsight-cluster-as-an-additional-storage-account"></a>Posso adicionar um Azure Data Lake Storage Gen2 a um cluster HDInsight existente como uma conta de armazenamento adicional?

Não, atualmente não é possível adicionar uma conta de armazenamento Azure Data Lake Storage Gen2 a um cluster que tenha armazenamento de BLOBs como seu armazenamento primário. Para obter mais informações, consulte [comparar opções de armazenamento](hdinsight-hadoop-compare-storage-options.md).

### <a name="how-can-i-find-the-currently-linked-service-principal-for-a-data-lake-storage-account"></a>Como posso encontrar a entidade de serviço vinculada atualmente para uma conta de armazenamento Data Lake?

Você pode encontrar suas configurações em **Data Lake Storage Gen1 acesso** em suas propriedades de cluster no portal do Azure. Para obter mais informações, consulte [verificar a configuração do cluster](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#verify-cluster-set-up).
 
### <a name="how-can-i-calculate-the-usage-of-storage-accounts-and-blob-containers-for-my-hdinsight-clusters"></a>Como calcular o uso de contas de armazenamento e contêineres de BLOB para meus clusters HDInsight?

Realize um dos seguintes procedimentos:

- [Usar o PowerShell](../storage/scripts/storage-blobs-container-calculate-size-powershell.md)

- Localize o tamanho do */User/Hive/. Lixeira/* pasta no cluster HDInsight, usando a seguinte linha de comando:
  
  `hdfs dfs -du -h /user/hive/.Trash/`

### <a name="how-can-i-set-up-auditing-for-my-blob-storage-account"></a>Como posso configurar a auditoria para minha conta de armazenamento de BLOBs?

Para auditar contas de armazenamento de BLOBs, configure o monitoramento usando o procedimento em [monitorar uma conta de armazenamento no portal do Azure](../storage/common/storage-monitor-storage-account.md). Um log HDFS-Audit fornece apenas informações de auditoria somente para o sistema de arquivos do HDFS local (hdfs://mycluster).  Ele não inclui operações que são feitas no armazenamento remoto.

### <a name="how-can-i-transfer-files-between-a-blob-container-and-an-hdinsight-head-node"></a>Como posso transferir arquivos entre um contêiner de BLOB e um nó principal do HDInsight?

Execute um script semelhante ao seguinte script de Shell em seu nó de cabeçalho:

```shell
for i in cat filenames.txt
do
   hadoop fs -get $i <local destination>
done
```
 
> [!NOTE]
> O arquivo file *names. txt* terá o caminho absoluto dos arquivos nos contêineres de BLOB.
 
### <a name="are-there-any-ranger-plugins-for-storage"></a>Há algum plug-in do Ranger para armazenamento?

No momento, não existe nenhum plug-in do Ranger para armazenamento de BLOBs e Azure Data Lake Storage Gen1 ou Gen2. Para clusters ESP, você deve usar Azure Data Lake Storage, porque você pode, pelo menos, definir as permissões refinadas manualmente no nível do sistema de arquivos usando as ferramentas do HDFS. Além disso, ao usar Azure Data Lake Storage, os clusters ESP farão parte do controle de acesso do sistema de arquivos usando Azure Active Directory no nível do cluster. 

Você pode atribuir políticas de acesso a dados aos grupos de segurança dos seus usuários usando o Gerenciador de Armazenamento do Azure. Para obter mais informações, consulte:

- [Como fazer definir permissões para que os usuários do Azure AD consultem dados em Data Lake Storage Gen2 usando o hive ou outros serviços?](hdinsight-hadoop-use-data-lake-storage-gen2.md#how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services)
- [Definir permissões no nível do arquivo e do diretório usando o Gerenciador de Armazenamento do Azure com o Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

### <a name="can-i-increase-hdfs-storage-on-a-cluster-without-increasing-the-disk-size-of-worker-nodes"></a>Posso aumentar o armazenamento do HDFS em um cluster sem aumentar o tamanho do disco dos nós de trabalho?

Não, você não pode aumentar o tamanho do disco de nenhum nó de trabalho, portanto, a única maneira de aumentar o tamanho do disco é descartar o cluster e recriá-lo com VMs de trabalho maiores. Não use o HDFS para armazenar nenhum dos seus dados do HDInsight, pois os dados serão excluídos se você excluir o cluster. Em vez disso, armazene seus dados no Azure. Escalar verticalmente o cluster também pode adicionar capacidade adicional ao cluster HDInsight.

## <a name="edge-nodes"></a>Nós de borda

### <a name="can-i-add-an-edge-node-after-the-cluster-has-been-created"></a>Posso adicionar um nó de borda após a criação do cluster?

Cluster HDInsight ou para um novo cluster ao criar o cluster. Para obter mais informações, confira o artigo [Usar nós de borda vazios em clusters Apache Hadoop no HDInsight](hdinsight-apps-use-edge-node.md).

### <a name="how-can-i-connect-to-an-edge-node"></a>Como posso me conectar a um nó de borda?

Depois de criar um nó de borda, você pode se conectar a ele usando SSH na porta 22. Você pode encontrar o nome do nó de borda no portal do cluster. Os nomes geralmente terminam com *-Ed*.

### <a name="why-are-persisted-scripts-not-running-automatically-on-newly-created-edge-nodes"></a>Por que os scripts persistentes não são executados automaticamente em nós de borda recém-criados?

Você usa scripts persistentes para personalizar novos nós de trabalho adicionados ao cluster por meio de operações de dimensionamento. Os scripts persistentes não se aplicam a nós de borda.

## <a name="rest-api"></a>API REST

### <a name="what-are-the-rest-api-calls-to-pull-a-tez-query-view-from-the-cluster"></a>Quais são as chamadas à API REST para efetuar pull de uma exibição de consulta tez do cluster?

Você pode usar os seguintes pontos de extremidade REST para efetuar pull das informações necessárias no formato JSON. Use cabeçalhos de autenticação básica para fazer as solicitações.

- Tez Visualização da Consulta: *https:\//\<nome do cluster >. azurehdinsight. net/WS/v1/Timeline/HIVE_QUERY_ID/*
- Tez DAG exibição: *https:\//\<nome do cluster >. azurehdinsight. net/WS/v1/Timeline/TEZ_DAG_ID/*

### <a name="how-do-i-retrieve-the-configuration-details-from-hdi-cluster-by-using-an-azure-active-directory-user"></a>Como fazer recuperar os detalhes de configuração do cluster HDI usando um usuário Azure Active Directory?

Para negociar tokens de autenticação apropriados com o usuário do AAD, percorra o gateway usando o seguinte formato:

* https://`<cluster dnsname>`. azurehdinsight.net/api/v1/clusters/testclusterdem/stack_versions/1/repository_versions/1 

### <a name="how-do-i-use-ambari-restful-api-to-monitor-yarn-performance"></a>Como fazer usar a API RESTful Ambari para monitorar o desempenho do YARN?

Se você chamar o comando de ondulação na mesma rede virtual ou em uma rede virtual emparelhada, o comando será:

```curl
curl -u <cluster login username> -sS -G
http://<headnodehost>:8080/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
```
 
Se você chamar o comando de fora da rede virtual ou de uma rede virtual não emparelhada, o formato do comando será:

- Para um cluster não ESP:
  
  ```curl
  curl -u <cluster login username> -sS -G 
  https://<ClusterName>.azurehdinsight.net/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
  ```

- Para um cluster ESP:
  
  ```curl
  curl -u <cluster login username>-sS -G 
  https://<ClusterName>.azurehdinsight.net/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
  ```

> [!NOTE]
> A ondulação solicitará uma senha. Você deve inserir uma senha válida para o nome de usuário de logon do cluster.

## <a name="billing"></a>Cobrança

### <a name="how-much-does-it-cost-to-deploy-an-hdinsight-cluster"></a>Quanto custa para implantar um cluster HDInsight?

Para obter mais informações sobre preços e perguntas frequentes relacionadas à cobrança, consulte a página de [preços do Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) .

### <a name="when-does-hdinsight-billing-start--stop"></a>Quando o início da cobrança do HDInsight & parar?

A cobrança do cluster HDInsight começa quando um cluster é criado e para quando o cluster é excluído. A cobrança é classificada em pro por minuto.

### <a name="how-do-i-cancel-my-subscription"></a>Como fazer cancelar minha assinatura?

Para obter informações sobre como cancelar sua assinatura, consulte [cancelar sua assinatura do Azure](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription).

### <a name="for-pay-as-you-go-subscriptions-what-happens-after-i-cancel-my-subscription"></a>Para assinaturas pagas conforme o uso, o que acontece depois de cancelar minha assinatura?

Para obter informações sobre sua assinatura após sua cancelamento, consulte [o que acontece depois de cancelar minha assinatura?](/azure/billing/billing-how-to-cancel-azure-subscription)

## <a name="hive"></a>Hive

### <a name="why-does-the-hive-version-appear-as-121000-instead-of-21-in-the-ambari-ui-even-though-i-am-running-an-hdinsight-36-cluster"></a>Por que a versão do hive aparece como 1.2.1000 em vez de 2,1 na interface do usuário do Ambari embora eu esteja executando um cluster HDInsight 3,6?

Embora apenas 1,2 apareça na interface do usuário do amAmbari, o HDInsight 3,6 contém o hive 1,2 e o hive 2,1.

## <a name="other-faq"></a>Outras perguntas frequentes

### <a name="what-does-hdinsight-offer-in-terms-of-real-time-stream-processing-capabilities"></a>O que o HDInsight oferece em termos de recursos de processamento de fluxo em tempo real?

Para obter informações sobre os recursos de integração do processamento de fluxo no Azure HDInsight, consulte [escolhendo uma tecnologia de processamento de fluxo no Azure](/azure/architecture/data-guide/technology-choices/stream-processing).

### <a name="is-there-a-way-to-dynamically-terminate-the-head-node-of-the-cluster-when-the-cluster-is-idle-for-a-specific-period"></a>Há uma maneira de encerrar dinamicamente o nó principal do cluster quando o cluster está ocioso por um período específico?

Você não pode fazer isso com clusters HDInsight. Você pode usar Azure Data Factory para esses cenários.

### <a name="what-compliance-offerings-does-hdinsight-offer"></a>Quais ofertas de conformidade o HDInsight oferece?

Para obter informações de conformidade, consulte a [central de confiabilidade da Microsoft](https://www.microsoft.com/trust-center) e a [visão geral da conformidade do Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).
