---
title: Solução de problemas - Trabalhadores do Runbook Híbrido de Automação do Azure
description: Este artigo fornece informações sobre solução de problemas de trabalhadores do Runbook Híbrido de Automação do Azure
services: automation
ms.service: automation
ms.subservice: ''
author: mgoedtel
ms.author: magoedte
ms.date: 11/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 31d81c6946fc256f5c22b93674469d7b87500173
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480704"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Solucionar problemas de trabalhadores de runbooks híbridos

Este artigo fornece informações sobre como solucionar problemas com os trabalhadores de runbook híbridos.

## <a name="general"></a>Geral

O operador de Runbook híbrido depende de um agente para se comunicar com sua conta de automação para registrar o trabalho, recebe trabalhos de runbook e status de relatórios. Para o Windows, esse agente é o agente Log Analytics para Windows (também conhecido como Microsoft Monitoring Agent (MMA)). Para o Linux, é o agente Log Analytics para Linux.

### <a name="runbook-execution-fails"></a>Cenário: a execução do Runbook falha

#### <a name="issue"></a>Problema

A execução do runbook falha e você recebe o seguinte erro:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Seu runbook foi suspenso logo depois de tentar executá-lo três vezes. Existem condições que podem impedir o runbook de ser concluído. Quando isso acontece, a mensagem de erro relacionada não pode incluir informações adicionais que informam o motivo.

#### <a name="cause"></a>Causa

A seguir estão possíveis causas possíveis:

* Os runbooks não podem autenticar com recursos locais

* O Hybrid Worker está usando um proxy ou firewall

* Os runbooks não podem autenticar com recursos locais

* O computador configurado para executar o recurso Hybrid Runbook Worker não atende aos requisitos mínimos de hardware.

#### <a name="resolution"></a>Resolução

Verifique se o computador tem acesso de saída para *.azure automation.net na porta 443.

Os computadores que executam o Hybrid Runbook Worker devem atender aos requisitos mínimos de hardware antes de serem configurados para hospedar esse recurso. Runbooks e os processos em segundo plano usados podem causar que o sistema se sobrecarregue e provocará atrasos ou tempos limite de trabalho de runbook.

Confirme o computador que executará o recurso Hybrid Runbook Worker atende aos requisitos mínimos de hardware. Se isso acontecer, monitore o uso de CPU e memória para determinar qualquer correlação entre o desempenho de processos do Hybrid Runbook Worker e o Windows. Se houver pressão da CPU e memória, isso pode indicar a necessidade de atualizar os recursos. Você também pode selecionar um recurso de computação diferente que consiga dar suporte aos requisitos mínimos e ajuste a escala quando a demanda da carga de trabalho indicar que um aumento é necessário.

Verifique o log de eventos do **Microsoft SMA** para ver um evento correspondente com descrição *Processo Win32 Encerrado com o código [4294967295]* . A causa desse erro é que você ainda não configurou a autenticação em seus runbooks ou especificou as credenciais Executar como para o grupo do Hybrid Worker. Analise as [Permissões de runbook](../automation-hrw-run-runbooks.md#runbook-permissions) para confirmar que a autenticação dos runbooks está configurada corretamente.

### <a name="no-cert-found"></a>Cenário: nenhum certificado foi encontrado no repositório de certificados no Hybrid Runbook Worker

#### <a name="issue"></a>Problema

Um runbook em execução em um Hybrid Runbook Worker falha com a seguinte mensagem de erro:

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Causa

Esse erro ocorre quando você tenta usar um [Executar como Conta](../manage-runas-account.md) em um runbook que é executado em um Hybrid Runbook Worker em que o certificado de Executar como Conta não está presente. Hybrid Runbook Workers não tem o ativo de certificado localmente, por padrão, o que é necessário para o Executar como Conta funcione corretamente.

#### <a name="resolution"></a>Resolução

Se seu Hybrid Runbook Worker é uma VM do Azure, você pode usar [Identidades gerenciadas para recursos do Azure](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) em vez disso. Esse cenário permite que você se autentique nos recursos do Azure usando a identidade gerenciada da VM do Azure, em vez de Executar como Conta, simplificando a autenticação. Quando o Hybrid Runbook Worker é um computador local, você precisará instalar o certificado de Executar como Conta no computador. Para saber como instalar o certificado, consulte as etapas para executar o runbook [Export-RunAsCertificateToHybridWorker](../automation-hrw-run-runbooks.md#runas-script).

## <a name="linux"></a>Linux

O Hybrid Runbook Worker do Linux depende do [agente de log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md) para se comunicar com sua conta de automação para registrar o trabalho, receber trabalhos de runbook e relatar o status. Se o registro do worker falhar, aqui estão algumas das possíveis causas do erro:

### <a name="oms-agent-not-running"></a>Cenário: o agente análise do log para Linux não está em execução

#### <a name="issue"></a>Problema

O agente de Log Analytics para Linux não está em execução

#### <a name="cause"></a>Causa

Se o agente não estiver em execução, ele impedirá que o Hybrid Runbook Worker Linux se comunique com a automação do Azure. O agente não pode ser executado por vários motivos.

#### <a name="resolution"></a>Resolução

 Verifique se o agente está em execução digitando o seguinte comando: `ps -ef | grep python`. Você deverá ver uma saída semelhante à seguinte, os processos de python com conta de usuário **nxautomation**. Se as soluções de Gerenciamento de Atualizações ou de Automação do Azure não estiverem ativadas, nenhum dos processos a seguir estará em execução.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

A lista a seguir mostra os processos que são iniciados para um Hybrid Runbook Worker do Linux. Eles estão todos localizados no diretório `/var/opt/microsoft/omsagent/state/automationworker/`.


* **oms.conf** - esse valor é o processo do gerenciador do trabalhador. Ele é iniciado diretamente do DSC.

* **worker.conf** -Este processo é o processo do Hybrid Worker Registrado Automaticamente, ele é iniciado pelo gerenciador de trabalho. Esse processo é usado pelo Gerenciamento de Atualizações e é transparente para o usuário. Este processo não está presente se a solução de Gerenciamento de Atualizações não estiver ativada na máquina.

* **diy/worker.conf** - Este processo é o processo do Hybrid Worker DIY. O processo do Hybrid Worker DIY é usado para executar runbooks de usuário no Hybrid Runbook Worker. Ele é diferente apenas do processo do Hybrid Worker Registrado Automaticamente nos detalhes da chave, visto que usa uma configuração diferente. Esse processo não estará presente se a solução de Automação do Azure estiver desabilitada e o DIY Linux Hybrid Worker não estiver registrado.

Se o agente não estiver em execução, execute o seguinte comando para iniciar o serviço: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Cenário: a classe especificada não existe

Se você vir o erro: **A classe especificada não existe..** no `/var/opt/microsoft/omsconfig/omsconfig.log`, o agente do Log Analytics para Linux precisa ser atualizado. Execute o seguinte comando para reinstalar o agente:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

O Windows Hybrid Runbook Worker depende do [agente de log Analytics para Windows](../../azure-monitor/platform/log-analytics-agent.md) se comunicar com sua conta de automação para registrar o trabalho, receber trabalhos de runbook e relatar o status. Se o registro do worker falhar, aqui estão algumas das possíveis causas do erro:

### <a name="mma-not-running"></a>Cenário: o Microsoft Monitoring Agent não está em execução

#### <a name="issue"></a>Problema

O serviço `healthservice` não está sendo executado na máquina Hybrid Runbook Worker.

#### <a name="cause"></a>Causa

Se o serviço do Windows Microsoft Monitoring Agent não estiver em execução, este estado impedirá o Hybrid Runbook Worker de se comunicar com a Automação do Azure.

#### <a name="resolution"></a>Resolução

Verifique se o agente está em execução digitando o seguinte comando no PowerShell: `Get-Service healthservice`. Se o serviço for interrompido, digite o seguinte comando no PowerShell para iniciar o serviço: `Start-Service healthservice`.

### <a name="event-4502"></a> Evento 4502 no log do Operations Manager

#### <a name="issue"></a>Problema

No log de eventos **Application and Services logs Manager** , você vê o evento 4502 e o eventMessage que contém **Microsoft. EnterpriseManagement. HealthService. AzureAutomation. HybridAgent** com a seguinte descrição: *o certificado apresentado pelo serviço \<WSID\>. OMS.opinsights.Azure.com não foi emitido por uma autoridade de certificação usada para os serviços da Microsoft. Entre em contato com seu administrador de rede para ver se eles estão executando um proxy que intercepta a comunicação TLS/SSL.*

#### <a name="cause"></a>Causa

Esse problema poderá ocorrer se seu proxy ou firewall de rede estiver bloqueando a comunicação com o Microsoft Azure. Verifique se o computador tem acesso de saída para *.azure-automation.net na porta 443. 

#### <a name="resolution"></a>Resolução

Os logs são armazenados localmente em cada hybrid worker, em C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Você pode verificar se há algum aviso ou eventos de erro nos logs de eventos **Logs de Aplicativos e Serviços\Microsoft-SMA\Operações** e **Logs de Aplicativos e Serviços\Operations Manager** que indiquem um problema de conectividade ou outro problema que esteja afetando a integração da função de Automação do Azure ou um problema ao em operações normais. Para obter ajuda adicional sobre como solucionar problemas com o agente de Log Analytics, consulte [solucionar problemas com o log Analytics agente do Windows](../../azure-monitor/platform/agent-windows-troubleshoot.md).

[A saída e as mensagens do runbook](../automation-runbook-output-and-messages.md) são enviadas à Automação do Azure do Hybrid Workers assim como os trabalhos do runbook são executados na nuvem. Também é possível habilitar os fluxos Verbose e Progress da mesma forma que você faria para outros runbooks.

### <a name="corrupt-cache"></a> Requisitos do Hybrid Runbook Worker não relatando

#### <a name="issue"></a>Problema

Sua máquina Hybrid Runbook Worker está em execução, mas você não verá dados de pulsação para a máquina no workspace.

A consulta de exemplo a seguir mostra as máquinas em um workspace e sua última pulsação:

```loganalytics
// Last heartbeat of each computer
Heartbeat
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Causa

Esse problema pode ser causado por um cache corrompido no Hybrid Runbook Worker.

#### <a name="resolution"></a>Resolução

Para resolver esse problema, entre no Hybrid Runbook Worker e execute o seguinte script. Esse script interrompe o Microsoft Monitoring Agent, remove seu cache e reinicia o serviço. Essa ação força o Hybrid Runbook Worker baixar novamente sua configuração da automação do Azure.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>Cenário: não é possível adicionar um Hybrid Runbook Worker

#### <a name="issue"></a>Problema

Você recebe a seguinte mensagem de erro ao tentar adicionar um Hybrid Runbook Worker usando o cmdlet `Add-HybridRunbookWorker`.

```error
Machine is already registered
```

#### <a name="cause"></a>Causa

Isso pode ser causado se o computador já está registrado com uma conta de automação diferente ou se você tentar adicionar novamente o Hybrid Runbook Worker após tê-lo removido de um computador.

#### <a name="resolution"></a>Resolução

Para resolver esse problema, remova a seguinte chave do Registro, reinicie `HealthService` e experimente o cmdlet `Add-HybridRunbookWorker` novamente:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>Próximas etapas

Se você não encontrou seu problema ou não conseguiu resolver seu problema, visite um dos seguintes canais para obter mais suporte:

* Obtenha respostas de especialistas do Azure por meio de [Fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Conecte-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente conectando-se à comunidade do Azure para os recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, você pode registrar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **Obter Suporte**.

