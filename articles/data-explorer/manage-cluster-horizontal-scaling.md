---
title: Gerenciar o dimensionamento horizontal do cluster (scale out) no Azure Data Explorer para acomodar a demanda em alteração
description: Este artigo descreve as etapas para escalar horizontalmente e reduzir em um cluster Data Explorer do Azure com base na demanda em alteração.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: 52a9c0a13723361bbc93362cdd9e2c73ef0372f2
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74942232"
---
# <a name="manage-cluster-horizontal-scaling-scale-out-in-azure-data-explorer-to-accommodate-changing-demand"></a>Gerenciar o dimensionamento horizontal do cluster (scale out) no Azure Data Explorer para acomodar a demanda em alteração

Dimensionar um cluster apropriadamente é essencial para o desempenho do Azure Data Explorer. Um tamanho de cluster estático pode levar a subutilização ou superutilização, e nenhuma dessas condições é a ideal. Como a demanda em um cluster não pode ser prevista com precisão absoluta, é melhor *dimensionar* um cluster, adicionar e remover recursos de capacidade e CPU com alteração de demanda. 

Há dois fluxos de trabalho para dimensionar um cluster de Data Explorer do Azure: 
* Dimensionamento horizontal, também chamado de expansão e saída.
* [Dimensionamento vertical](manage-cluster-vertical-scaling.md), também chamado de expansão e redução.
Este artigo explica o fluxo de trabalho de dimensionamento horizontal.

## <a name="configure-horizontal-scaling"></a>Configurar o dimensionamento horizontal

Usando o dimensionamento horizontal, você pode dimensionar a contagem de instâncias automaticamente, com base em regras e agendamentos predefinidos. Para especificar as configurações de dimensionamento automático para o cluster:

1. Na portal do Azure, vá para o recurso de cluster de Data Explorer do Azure. Em **configurações**, selecione **escalar horizontalmente**. 

2. Na janela **escalar horizontalmente** , selecione o método de dimensionamento automático desejado: **escala manual**, **dimensionamento automático otimizado**ou **dimensionamento automático personalizado**.

### <a name="manual-scale"></a>Dimensionamento manual

A escala manual é a configuração padrão durante a criação do cluster. O cluster tem uma capacidade estática que não é alterada automaticamente. Você seleciona a capacidade estática usando a barra de **contagem de instâncias** . O dimensionamento do cluster permanece nessa configuração até que você faça outra alteração.

   ![Método de dimensionamento manual](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale-preview"></a>Dimensionamento automático otimizado (versão prévia)

Dimensionamento automático otimizado é o método de dimensionamento automático recomendado. Esse método otimiza o desempenho e os custos do cluster. Se o cluster se aproximar de um estado de subutilização, ele será dimensionado no. Essa ação reduz os custos, mas mantém o nível de desempenho. Se o cluster se aproximar de um estado de excesso de utilização, ele será escalado horizontalmente para manter o desempenho ideal. Para configurar o dimensionamento automático otimizado:

1. Selecione **dimensionamento automático otimizado**. 

1. Selecione uma contagem de instâncias mínima e uma contagem máxima de instâncias. O dimensionamento automático do cluster varia entre esses dois números, com base na carga.

1. Clique em **Salvar**.

   ![Método de dimensionamento automático otimizado](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

O dimensionamento automático otimizado começa a funcionar. Suas ações agora estão visíveis no log de atividades do Azure do cluster.

#### <a name="logic-of-optimized-autoscale"></a>Lógica de dimensionamento automático otimizado 

**Escalar horizontalmente**

Quando o cluster se aproxima de um estado de utilização excessiva, escale horizontalmente para manter o desempenho ideal. A expansão ocorrerá quando:
* O número de instâncias de cluster está abaixo do número máximo de instâncias definidas pelo usuário.
* A utilização do cache é alta por mais de uma hora.

> [!NOTE]
> A lógica de expansão não considera atualmente a utilização de ingestão e as métricas de CPU. Se essas métricas forem importantes para seu caso de uso, use o [dimensionamento automático personalizado](#custom-autoscale).

**Reduzir horizontalmente**

Quando o cluster se aproximar de um estado de subutilização, dimensione-o para reduzir os custos, mas manter o desempenho. Várias métricas são usadas para verificar se é seguro dimensionar no cluster. As regras a seguir são avaliadas diariamente por 7 dias antes da realização de scale in:
* O número de instâncias é acima de 2 e acima do número mínimo de instâncias definidas.
* Para garantir que não haja sobrecarga de recursos, as métricas a seguir devem ser verificadas antes de a redução de escala ser executada: 
    * A utilização do cache não é alta
    * A CPU está abaixo da média 
    * A utilização da ingestão está abaixo da média 
    * A utilização de ingestão de streaming (se a ingestão de streaming for usada) não é alta
    * Os eventos Keep Alive estão acima de um mínimo definido, processados corretamente e no momento.
    * Sem limitação de consulta 
    * O número de consultas com falha está abaixo do mínimo definido.

> [!NOTE]
> A escala na lógica atualmente requer uma avaliação de 7 dias antes da implementação da escala otimizada no. Essa avaliação ocorre uma vez a cada 24 horas. Se for necessária uma alteração rápida, use a [escala manual](#manual-scale).

### <a name="custom-autoscale"></a>Dimensionamento automático personalizado

Usando o dimensionamento automático personalizado, você pode dimensionar seu cluster dinamicamente com base nas métricas que você especificar. O gráfico a seguir mostra o fluxo e as etapas para configurar o dimensionamento automático personalizado. Para obter mais detalhes, siga o elemento gráfico.

1. Na caixa **nome da configuração de dimensionamento automático** , insira um nome, como *escala horizontal: utilização de cache*. 

   ![Regra de escala](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. Para o **modo de escala**, selecione **escala com base em uma métrica**. Esse modo fornece dimensionamento dinâmico. Você também pode selecionar **Dimensionar para uma contagem de instâncias específica**.

3. Selecione **+ Adicionar uma regra**.

4. Na seção **regra de dimensionamento** à direita, insira valores para cada configuração.

    **Critérios**

    | Configuração | Descrição e valor |
    | --- | --- |
    | **Agregação de tempo** | Selecione um critério de agregação, como **Média**. |
    | **Nome da métrica** | Selecione a métrica na qual a operação de escala se baseará, como **Utilização de cache**. |
    | **Estatística de intervalo de agregação** | Escolha entre **Média**, **Mínima**, **Máxima** e **Soma**. |
    | **Operador** | Escolha a opção apropriada, como **Maior que ou igual a**. |
    | **Limite** | Escolha um valor apropriado. Por exemplo, para a utilização de cache, 80% é um bom ponto de partida. |
    | **Duração (em minutos)** | Escolha um período apropriado para o sistema retroceder ao calcular métricas. Comece com o padrão de dez minutos. |
    |  |  |

    **Ação**

    | Configuração | Descrição e valor |
    | --- | --- |
    | **Operação** | Escolha a opção apropriada para reduzir ou escalar horizontalmente. |
    | **Contagem de instâncias** | Escolha o número de nós ou instâncias que você deseja adicionar ou remover quando uma condição de métrica for atendida. |
    | **Tempo de resfriamento (minutos)** | Escolha um intervalo de tempo apropriado de espera entre as operações de escala. Comece com o padrão de cinco minutos. |
    |  |  |

5. Selecione **Adicionar**.

6. Na seção **limites da instância** à esquerda, insira valores para cada configuração.

    | Configuração | Descrição e valor |
    | --- | --- |
    | **Mínimo** | É o número de instâncias para baixo do qual o cluster não será reduzido, independentemente da utilização. |
    | **Máximo** | É o número de instâncias para acima do qual o cluster não será aumentado, independentemente da utilização. |
    | **Padrão** | O número padrão de instâncias. Essa configuração será usada se houver problemas na leitura das métricas de recurso. |
    |  |  |

7. Clique em **Salvar**.

Agora você configurou o dimensionamento horizontal para o cluster de Data Explorer do Azure. Adicione outra regra para o dimensionamento vertical. Se você precisar de assistência com problemas de dimensionamento de cluster, [abra uma solicitação de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) no portal do Azure.

## <a name="next-steps"></a>Próximos passos

* [Monitorar o desempenho, a integridade e o uso do Data Explorer do Azure com métricas](using-metrics.md)
* [Gerencie o dimensionamento vertical do cluster](manage-cluster-vertical-scaling.md) para o dimensionamento apropriado de um cluster.
