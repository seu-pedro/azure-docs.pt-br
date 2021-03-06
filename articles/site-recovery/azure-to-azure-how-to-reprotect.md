---
title: Proteger novamente as VMs do Azure para a região primária com Azure Site Recovery | Microsoft Docs
description: Descreve como proteger novamente as VMs do Azure após o failover, a região secundária para a primária, usando Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 955e1b84f897a5eb877033e0a58b8d661f143a14
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73954182"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Proteger novamente VMs do Azure que sofreram failover para a região primária


Quando você [faz failover](site-recovery-failover.md) de VMs do Azure de uma região para outra usando o [Azure Site Recovery](site-recovery-overview.md), as VMs inicializam na região secundária, em um estado desprotegido. Se você fizer failback das VMs de volta para a região primária, faça o seguinte:

- Proteja novamente as VMs na região secundária, para que elas iniciem a replicação para a região primária.
- Depois que a nova proteção é concluída e as VMs estão replicando, você pode fazer failover delas da região secundária para a primária.

## <a name="prerequisites"></a>pré-requisitos
1. O failover de VM da região primária para a secundária deve ser confirmado.
2. O local de destino primário deve estar disponível e você deve ser capaz de acessar ou criar recursos nessa região.

## <a name="reprotect-a-vm"></a>Proteger novamente uma VM

1. Em **Cofre** > **Itens replicados**, clique com o botão direito do mouse na VM em que o failover foi executado e, em seguida, selecione **Proteger Novamente**. A direção de nova proteção deve mostrar da secundária para a primária.

   ![Proteger novamente](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Examine o grupo de recursos, a rede, o armazenamento e os conjuntos de disponibilidade. Em seguida, clique em **OK**. Se houver algum recurso marcado como novo, ele será criado como parte do processo de nova proteção.
3. O trabalho de nova proteção propaga os dados mais recentes para o site de destino. Depois que isso termina, ocorre a replicação delta. Em seguida, você pode fazer failover de volta para o site primário. Você pode selecionar a conta de armazenamento ou a rede que você deseja usar ao proteger novamente, usando a opção personalizar.

   ![Opção de personalização](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Personalizar as configurações de nova proteção

Você pode personalizar as propriedades a seguir da VM de destino ao proteger novamente.

![Personalizar](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Propriedade |Observações  |
|---------|---------|
|Grupo de recursos de destino     | Modifique o grupo de recursos de destino no qual a VM é criada. Como parte da nova proteção, a VM de destino é excluída. Você pode escolher um novo grupo de recursos sob o qual criar a VM após o failover.        |
|Rede virtual de destino     | A rede de destino não pode ser alterada durante o trabalho de nova proteção. Para alterar a rede, refaça o mapeamento de rede.         |
|Armazenamento de destino (a VM secundária não usa discos gerenciados)     | Você pode alterar a conta de armazenamento que a VM usa após o failover.         |
|Discos gerenciados de réplica (a VM secundária usa discos gerenciados)    | O Site Recovery cria discos gerenciados de réplica na região primária para espelhar discos gerenciados da VM secundária.         |
|Armazenamento de cache     | Você pode especificar uma conta de armazenamento de cache a ser usada durante a replicação. Por padrão, uma nova conta de armazenamento de cache será criada se ela não existir.         |
|Conjunto de disponibilidade     |Se a VM na região secundária for parte de um conjunto de disponibilidade, você poderá escolher um conjunto de disponibilidade para a VM de destino na região primária. Por padrão, o Site Recovery tenta localizar o conjunto de disponibilidade existente na região primária e usá-lo. Durante a personalização, você pode especificar um novo conjunto de disponibilidade.         |


### <a name="what-happens-during-reprotection"></a>O que acontece ao proteger novamente?

Por padrão, acontece o seguinte:

1. Uma conta de armazenamento de cache é criada na região onde a VM com failover está em execução.
2. Se a conta de armazenamento de destino (a conta de armazenamento original na região primária) não existir, uma nova será criada. O nome da conta de armazenamento atribuído é o nome da conta de armazenamento usada pela VM secundária, com o sufixo "asr".
3. Se a sua VM usar discos gerenciados, os discos gerenciados de réplica serão criados na região primária para armazenar os dados replicados dos discos da VM secundária.
4. Se o conjunto de disponibilidade de destino não existir, um novo será criado como parte do trabalho de nova proteção, se necessário. Se você personalizar as configurações de nova proteção, o conjunto selecionado será usado.

Quando você dispara um trabalho de nova proteção e a VM de destino existe, ocorre o seguinte:

1. A VM do lado de destino é desligada quando ela está em execução.
2. Se a VM estiver usando discos gerenciados, uma cópia dos discos originais serão criados com o sufixo '-ASRReplica'. Os discos originais são excluídos. As cópias '-ASRReplica' são usadas para replicação.
3. Se a VM estiver usando discos não gerenciados, os discos de dados da VM de destino são desanexados e usados para replicação. Uma cópia do disco do sistema operacional é criada e anexada na VM. O disco do sistema operacional original é desanexado e usado para replicação.
4. Somente alterações entre o disco de origem e o disco de destino são sincronizadas. Os diferenciais são computados comparando ambos os discos e, em seguida, transferidos. Para localizar a verificação de tempo estimada abaixo.
5. Após a conclusão da sincronização, a replicação delta começa e cria um ponto de recuperação de acordo com a política de replicação.

Quando você dispara um trabalho de nova proteção e a VM de destino e o disco não existem, ocorre o seguinte:
1. Se a VM estiver usando discos gerenciados, réplicas dos discos originais serão criadas com o sufixo '-ASRReplica'. As cópias '-ASRReplica' são usadas para replicação.
2. Se a VM estiver usando discos não gerenciados, réplicas dos discos originais serão criadas na conta de armazenamento de destino.
3. Todos discos são copiados da região com failover para a nova região de destino.
4. Após a conclusão da sincronização, a replicação delta começa e cria um ponto de recuperação de acordo com a política de replicação.

#### <a name="estimated-time--to-do-the-reprotection"></a>Tempo estimado para a nova proteção 

Na maioria dos casos, Azure Site Recovery não replica os dados completos para a região de origem. Abaixo estão as condições que determinam a quantidade de dados que serão replicados:

1.  Se os dados da VM de origem forem excluídos, corrompidos ou inacessíveis devido a algum motivo, como alteração/exclusão do grupo de recursos, durante a nova proteção, o IR ocorrerá, pois não há dados disponíveis na região de origem a ser usada.
2.  Se os dados da VM de origem estiverem acessíveis, somente os diferenciais serão computados comparando os discos e, em seguida, transferidos. Verifique a tabela abaixo para obter o tempo estimado 

|\* * Situação de exemplo * * | \* * Tempo necessário para proteger novamente o * * |
|--- | --- |
|A região de origem tem uma VM com um disco padrão de 1 TB<br/>-Somente 127 GB de dados são usados e o restante do disco está vazio<br/>-O tipo de disco é padrão com taxa de transferência de MiB/S de 60<br/>-Sem alteração de dados após o failover| Tempo aproximado de 45 minutos – 1,5 horas<br/> -Durante a nova proteção Site Recovery preencherá a soma de verificação de dados inteiros que terão 127 GB/45 MBs ~ 45 minutos<br/>-Um tempo de sobrecarga é necessário para que Site Recovery fazer escala automática com 20-30 minutos<br/>-Sem encargos de egresso |
|A região de origem tem uma VM com um disco padrão de 1 TB<br/>-Somente 127 GB de dados são usados e o restante do disco está vazio<br/>-O tipo de disco é padrão com taxa de transferência de MiB/S de 60<br/>-45 GB de alteração de dados após o failover| Tempo aproximado de 1 hora – 2 horas<br/>-Durante a nova proteção Site Recovery preencherá a soma de verificação de dados inteiros que terão 127 GB/45 MBs ~ 45 minutos<br/>-Tempo de transferência para aplicar alterações de 45 GB com 45 GB/45 MBps ~ 17 minutos<br/>-Os encargos de egresso seriam apenas para dados de 45 GB e não para a soma de verificação|
 



## <a name="next-steps"></a>Próximas etapas

Depois que a VM estiver protegida, você poderá iniciar um failover. O failover desliga a VM na região secundária e cria e inicializa a VM na região primária, com um pequeno tempo de inatividade. Recomendamos que você escolha um horário adequadamente e que execute um failover de teste, mas iniciando um failover completo para o site primário. [Saiba mais](site-recovery-failover.md) sobre failover.
