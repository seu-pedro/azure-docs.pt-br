---
title: Gerenciar recursos locais remotos usando funções do PowerShell
description: Saiba como configurar Conexões Híbridas na retransmissão do Azure para conectar um aplicativo de funções do PowerShell a recursos locais, que podem ser usados para gerenciar remotamente o recurso local.
author: eamono
ms.topic: conceptual
ms.date: 9/5/2019
ms.author: eamono
ms.openlocfilehash: 36fc4c873dccfe9fa814bddccd829ed04207f095
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226929"
---
# <a name="managing-hybrid-environments-with-powershell-in-azure-functions-and-app-service-hybrid-connections"></a>Gerenciando ambientes híbridos com o PowerShell no Azure Functions e no serviço de aplicativo Conexões Híbridas

O recurso Conexões Híbridas do serviço de Azure App permite o acesso a recursos em outras redes. Você pode saber mais sobre esse recurso na documentação do [conexões híbridas](../app-service/app-service-hybrid-connections.md) . Este artigo descreve como usar essa funcionalidade para executar funções do PowerShell direcionadas a um servidor local. Esse servidor pode então ser usado para gerenciar todos os recursos no ambiente local por meio de uma função Azure PowerShell.


## <a name="configure-an-on-premises-server-for-powershell-remoting"></a>Configurar um servidor local para comunicação remota do PowerShell

O script a seguir habilita a comunicação remota do PowerShell e cria uma nova regra de firewall e um ouvinte HTTPS do WinRM. Para fins de teste, é usado um certificado autoassinado. Em um ambiente de produção, recomendamos que você use um certificado assinado.

```powershell
# For configuration of WinRM, see
# https://docs.microsoft.com/windows/win32/winrm/installation-and-configuration-for-windows-remote-management.

# Enable PowerShell remoting.
Enable-PSRemoting -Force

# Create firewall rule for WinRM. The default HTTPS port is 5986.
New-NetFirewallRule -Name "WinRM HTTPS" `
                    -DisplayName "WinRM HTTPS" `
                    -Enabled True `
                    -Profile "Any" `
                    -Action "Allow" `
                    -Direction "Inbound" `
                    -LocalPort 5986 `
                    -Protocol "TCP"

# Create new self-signed-certificate to be used by WinRM.
$Thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME  -CertStoreLocation Cert:\LocalMachine\My).Thumbprint

# Create WinRM HTTPS listener.
$Cmd = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:COMPUTERNAME ""; CertificateThumbprint=""$Thumbprint""}"
cmd.exe /C $Cmd
```

## <a name="create-a-powershell-function-app-in-the-portal"></a>Criar um aplicativo de funções do PowerShell no portal

O recurso de Conexões Híbridas do serviço de aplicativo está disponível apenas nos planos de preços básico, padrão e isolado. Ao criar o aplicativo de funções com o PowerShell, crie ou selecione um desses planos.

1. Na [portal do Azure](https://portal.azure.com), selecione **+ criar um recurso** no menu à esquerda e, em seguida, selecione **aplicativo de funções**.

1. Para o **plano de hospedagem**, selecione plano do serviço de **aplicativo**e, em seguida, selecione **plano/local do serviço de aplicativo**.

1. Selecione **criar novo**, digite um nome de **plano do serviço de aplicativo** , escolha um **local** em uma [região](https://azure.microsoft.com/regions/) perto de você ou perto de outros serviços que suas funções acessam e selecione **tipo de preço**.

1. Escolha o plano padrão S1 e, em seguida, selecione **aplicar**.

1. Selecione **OK** para criar o plano e, em seguida, defina as configurações de **aplicativo de funções** restantes conforme especificado na tabela imediatamente após a captura de tela a seguir:

    ![Aplicativo de funções do PowerShell Core](./media/functions-hybrid-powershell/create-function-powershell-app.png)  

    | Configuração      | Valor sugerido  | DESCRIÇÃO                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome do aplicativo** | Nome globalmente exclusivo | Nome que identifica seu novo aplicativo de funções. Os caracteres válidos são `a-z`, `0-9` e `-`.  | 
    | **Assinatura** | Sua assinatura | A assinatura na qual este novo aplicativo de funções será criado. |
    | **Grupo de recursos** |  myResourceGroup | Nome do novo grupo de recursos no qual criar o seu aplicativo de funções. Você também pode usar o valor sugerido. |
    | **SO** | Sistema operacional preferencial | Selecione Windows. |
    | **Pilha de runtime** | Linguagem preferencial | Escolha PowerShell Core. |
    | **Armazenamento** |  Nome globalmente exclusivo |  Crie uma conta de armazenamento usada pelo seu aplicativo de funções. Os nomes de conta de armazenamento devem ter de 3 a 24 caracteres de comprimento e podem conter apenas números e letras minúsculas. Você também pode usar uma conta existente.
    | **Application Insights** | Padrão | Cria um recurso do Application Insights do mesmo *nome do aplicativo* na região com suporte mais próxima. Ao expandir essa configuração, você pode alterar o **nome do novo recurso** ou escolher um **local** diferente em uma região de [Geografia do Azure](https://azure.microsoft.com/global-infrastructure/geographies/) onde você deseja armazenar seus dados. |

1. Depois que as configurações forem validadas, selecione **criar**.

1. Selecione o ícone de **notificação** no canto superior direito do portal e aguarde a mensagem "implantação bem-sucedida".

1. Selecione **Ir para recursos** para exibir o novo aplicativo de funções. Você também pode selecionar **Fixar no painel**. A fixação torna mais fácil retornar a esse recurso de aplicativo de função no seu painel.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>Criar uma conexão híbrida para o aplicativo de funções

As conexões híbridas são configuradas na seção rede do aplicativo de funções:

1. Selecione a guia **recursos da plataforma** no aplicativo de funções e, em seguida, selecione **rede**. 
   Visão geral do aplicativo ![para rede de plataforma](./media/functions-hybrid-powershell/app-overview-platform-networking.png)  
1. Selecione **configurar seus pontos de extremidade de conexões híbridas**.
   ![Rede](./media/functions-hybrid-powershell/select-network-feature.png)  
1. Selecione **Adicionar conexão híbrida**.
   ![conexão híbrida](./media/functions-hybrid-powershell/hybrid-connection-overview.png)  
1. Insira informações sobre a conexão híbrida conforme mostrado logo após a captura de tela a seguir. Você tem a opção de fazer com que a configuração de **host do ponto de extremidade** corresponda ao nome do host do servidor local para facilitar a memorização do servidor mais tarde, quando você estiver executando comandos remotos. A porta corresponde à porta padrão do serviço de gerenciamento remoto do Windows que foi definida no servidor anteriormente.
  ![Adicionar conexão híbrida](./media/functions-hybrid-powershell/add-hybrid-connection.png)  

    **Nome da conexão híbrida**: ContosoHybridOnPremisesServer
    
    **Host do ponto de extremidade**: finance1
    
    **Porta do ponto de extremidade**: 5986
    
    **Namespace do ServiceBus**: criar novo
    
    **Local**: escolha um local disponível
    
    **Nome**: contosopowershellhybrid

5. Selecione **OK** para criar a conexão híbrida.

## <a name="download-and-install-the-hybrid-connection"></a>Baixar e instalar a conexão híbrida

1. Selecione **baixar Gerenciador de conexões** para salvar o arquivo. msi localmente no seu computador.
![baixar o instalador](./media/functions-hybrid-powershell/download-hybrid-connection-installer.png)  
1. Copie o arquivo. msi do seu computador local para o servidor no local.
1. Execute o instalador do Gerenciador de Conexões Híbridas para instalar o serviço no servidor local.
![instalar a conexão híbrida](./media/functions-hybrid-powershell/hybrid-installation.png)  
1. No portal, abra a conexão híbrida e copie a cadeia de conexão do gateway para a área de transferência.
![copiar cadeia de conexão híbrida](./media/functions-hybrid-powershell/copy-hybrid-connection.png)  
1. Abra a interface do usuário do Gerenciador de Conexões Híbridas no servidor local.
![abrir a interface do usuário de conexão híbrida](./media/functions-hybrid-powershell/hybrid-connection-ui.png)  
1. Selecione o botão **inserir manualmente** e cole a cadeia de conexão da área de transferência.
![colar](./media/functions-hybrid-powershell/enter-manual-connection.png) de conexão  
1. Reinicie o Gerenciador de Conexões Híbridas do PowerShell se ele não for exibido como conectado.
    ```powershell
    Restart-Service HybridConnectionManager
    ```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Criar uma configuração de aplicativo para a senha de uma conta de administrador

1. Selecione a guia **recursos da plataforma** no aplicativo de funções.
1. Em **configurações gerais**, selecione **configuração**.
![selecionar a configuração de plataforma](./media/functions-hybrid-powershell/select-configuration.png)  
1. Expanda **nova configuração de aplicativo** para criar uma nova configuração para a senha.
1. Nomeie a configuração _ContosoUserPassword_e insira a senha.
1. Selecione **OK** e, em seguida, salvar para armazenar a senha no aplicativo de funções.
![adicionar configuração de aplicativo para a senha](./media/functions-hybrid-powershell/add-appsetting-password.png)  

## <a name="create-a-function-http-trigger-to-test"></a>Criar um gatilho http de função para teste

1. Crie uma nova função de gatilho HTTP do aplicativo de funções.
![criar um novo gatilho HTTP](./media/functions-hybrid-powershell/create-http-trigger-function.png)  
1. Substitua o código do PowerShell do modelo pelo código a seguir:

    ```powershell
    # Input bindings are passed in via param block.
    param($Request, $TriggerMetadata)
    
    # Write to the Azure Functions log stream.
    Write-Output "PowerShell HTTP trigger function processed a request."
    
    # Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
    $UserName = "ContosoUser"
    $securedPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
    $Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)
    
    # This is the name of the hybrid connection Endpoint.
    $HybridEndpoint = "finance1"
    
    $Script = {
        Param(
            [Parameter(Mandatory=$True)]
            [String] $Service
        )
        Get-Service $Service
    }
    
    Write-Output "Scenario 1: Running command via Invoke-Command"
    Invoke-Command -ComputerName $HybridEndpoint `
                   -Credential $Credential `
                   -Port 5986 `
                   -UseSSL `
                   -ScriptBlock $Script `
                   -ArgumentList "*" `
                   -SessionOption (New-PSSessionOption -SkipCACheck)
    ```

3. Selecione **salvar** e **executar** para testar a função.
![testar o aplicativo de funções](./media/functions-hybrid-powershell/test-function-hybrid.png)  

## <a name="managing-other-systems-on-premises"></a>Gerenciando outros sistemas locais

Você pode usar o servidor local conectado para se conectar a outros servidores e sistemas de gerenciamento no ambiente local. Isso permite que você gerencie suas operações de datacenter do Azure usando suas funções do PowerShell. O script a seguir registra uma sessão de configuração do PowerShell que é executada sob as credenciais fornecidas. Essas credenciais devem ser para um administrador nos servidores remotos. Você pode usar essa configuração para acessar outros pontos de extremidade no servidor local ou Datacenter.

```powershell
# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
$UserName = "ContosoUser"
$SecuredPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
$Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)

# This is the name of the hybrid connection Endpoint.
$HybridEndpoint = "finance1"

# The remote server that will be connected to run remote PowerShell commands on
$RemoteServer = "finance2".

Write-Output "Use hybrid connection server as a jump box to connect to a remote machine"

# We are registering an endpoint that runs under credentials ($Credential) that has access to the remote server.
$SessionName = "HybridSession"
$ScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $SessionName)

    if (-not (Get-PSSessionConfiguration -Name $SessionName -ErrorAction SilentlyContinue))
    {
        Register-PSSessionConfiguration -Name $SessionName -RunAsCredential $Using:Credential
    }
}

Write-Output "Registering session on hybrid connection jumpbox"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $ScriptCommand `
               -ArgumentList $SessionName `
               -SessionOption (New-PSSessionOption -SkipCACheck)

# Script to run on the jump box to run against the second machine.
$RemoteScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $ComputerName)
        # Write out the hostname of the hybrid connection server.
        hostname
        # Write out the hostname of the remote server.
        Invoke-Command -ComputerName $ComputerName -Credential $Using:Credential -ScriptBlock {hostname} `
                        -UseSSL -Port 5986 -SessionOption (New-PSSessionOption -SkipCACheck)
}

Write-Output "Running command against remote machine via jumpbox by connecting to the PowerShell configuration session"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $RemoteScriptCommand `
               -ArgumentList $RemoteServer `
               -SessionOption (New-PSSessionOption -SkipCACheck) `
               -ConfigurationName $SessionName
```

Substitua as seguintes variáveis neste script pelos valores aplicáveis do seu ambiente:
* $HybridEndpoint
* $RemoteServer

Nos dois cenários anteriores, você pode conectar e gerenciar seus ambientes locais usando o PowerShell em Azure Functions e Conexões Híbridas. Incentivamos você a aprender mais sobre o [conexões híbridas](../app-service/app-service-hybrid-connections.md) e o [PowerShell no functions](./functions-reference-powershell.md).

Você também pode usar [redes virtuais](./functions-create-vnet.md) do Azure para se conectar ao seu ambiente local por meio de Azure functions.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"] 
> [Saiba mais sobre como trabalhar com funções do PowerShell](functions-reference-powershell.md)
