---
title: incluir arquivo
description: incluir arquivo
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ab68fc6533be5e3241de2e49652251fea5fe2f7d
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780641"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operacionais e drivers com suporte

### <a name="nvidia-tesla-cuda-drivers"></a>Drivers NVIDIA Tesla (CUDA)

Drivers NVIDIA Tesla (CUDA) para NC, NCv2, NCv3, ND e VMs da série NDV2 (opcional para a série NV) têm suporte apenas em sistemas operacionais listados na tabela a seguir. Os links de download do driver Tesla são atuais no momento da publicação. Para os drivers mais recentes, visite o site da [NVIDIA](https://www.nvidia.com/).

> [!TIP]
> Como alternativa à instalação manual do driver CUDA em uma VM do Windows Server, você pode implantar uma imagem da [Máquina Virtual de Ciência de Dados](../articles/machine-learning/data-science-virtual-machine/overview.md) do Azure. As edições DSVM do Windows Server 2016 instalam previamente os drivers NVIDIA CUDA, a Biblioteca de Rede Neural Profunda CUDA e outras ferramentas.


| SISTEMA OPERACIONAL | Driver |
| -------- |------------- |
| Windows Server 2016 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Drivers NVIDIA GRID

A Microsoft redistribui os instaladores de driver de grade NVIDIA para VMs de série NVv3 e NV usadas como estações de trabalho virtuais ou para aplicativos virtuais. Instale somente esses drivers de grade nas VMs da série NV do Azure, somente nos sistemas operacionais listados na tabela a seguir. Esses drivers incluem o licenciamento de Software de GPU Virtual de GRID no Azure. Você não precisa configurar um servidor de licença de software de vGPU NVIDIA.

Observe que a extensão NVIDIA sempre instalará o driver mais recente. Fornecemos links para a versão anterior aqui para os clientes que têm dependência em uma versão mais antiga.

Para o Windows Server 2019, o Windows Server 2016 e o Windows 10 (até o Build 1909):
- [Grade 10,0 (441,66)](https://go.microsoft.com/fwlink/?linkid=874181) (. exe)
- [Grade 9,0 (431, 2)](https://download.microsoft.com/download/8/C/C/8CC88D54-EB07-44D3-8FA9-B797B173ED04/431.02_grid_win10_server2016_server2019_64bit_international.exe) (. exe) 

Para o Windows Server 2012 R2, Windows Server 2008 R2, Windows 8 e Windows 7: 
- [Grade 10,0 (441,66)](https://go.microsoft.com/fwlink/?linkid=874184) (. exe)
- [Grade 9,1 (431,79)](https://download.microsoft.com/download/8/6/e/86ef2daa-b31e-43ad-90f2-bd795384b71e/431.79_grid_win7_win8_server2008R2_server2012R2_64bit_international.exe) (. exe)  
