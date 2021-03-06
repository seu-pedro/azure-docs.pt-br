---
title: Introdução à área de trabalho do Windows na plataforma de identidade da Microsoft | Microsoft Docs
description: Como um aplicativo .NET da área de trabalho do Windows (XAML) pode obter um token de acesso e chamar uma API protegida pela plataforma de identidade da Microsoft.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: c29a06496bb1303849250f049e4e7444a5a5ddf3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423350"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Chamar a API do Microsoft Graph em um aplicativo da Área de Trabalho do Windows

Este guia demonstra como um aplicativo .NET da Área de Trabalho do Windows (XAML) nativo usa um token de acesso para chamar a API do Microsoft Graph. O aplicativo também pode acessar outras APIs que exigem tokens de acesso de uma plataforma de identidade da Microsoft para o ponto de extremidade v2.0 do desenvolvedor. Essa plataforma era chamada de Azure AD.

Depois de concluir o guia, seu aplicativo poderá chamar uma API protegida que usa contas pessoais (incluindo o outlook.com, live.com e outros). O aplicativo também usará contas corporativas e de estudante de qualquer empresa ou organização que usa o Azure Active Directory.  

> [!NOTE]
> O guia exige o Visual Studio 2015 Atualização 3, Visual Studio 2017 ou o Visual Studio 2019. Não tem nenhuma dessas versões? [Baixe gratuitamente o Visual Studio 2019](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Como o aplicativo de exemplo gerado por este guia funciona

![Mostra como funciona o aplicativo de exemplo gerado por este tutorial](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

O aplicativo de exemplo que você cria com este guia habilita um aplicativo da Área de Trabalho do Windows que consulta a API do Microsoft Graph ou uma API Web que aceita tokens de um ponto de extremidade da plataforma de identidade da Microsoft. Para esse cenário, você adiciona um token às solicitações HTTP por meio do cabeçalho de Autorização. A MSAL (Biblioteca de Autenticação da Microsoft) lida com a aquisição e a renovação de tokens.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Manipulando a aquisição de token para acessar APIs Web protegidas

Após a autenticação do usuário, o aplicativo de exemplo receberá um token que pode ser usado para consultar a API do Microsoft Graph ou uma API Web protegida pela plataforma de identidade da Microsoft para desenvolvedores.

APIs, como o Microsoft Graph, exigem um token para permitir o acesso a recursos específicos. Por exemplo, um token é necessário para ler o perfil ou acessar o calendário de um usuário, ou enviar um email. O aplicativo pode solicitar um token de acesso usando a MSAL para acessar esses recursos especificando escopos de API. Esse token de acesso é então adicionado ao cabeçalho de Autorização HTTP de cada chamada feita no recurso protegido.

A MSAL gerencia o armazenamento em cache e a atualização de tokens de acesso para você, de forma que o aplicativo não precise fazer isso.

## <a name="nuget-packages"></a>Pacotes NuGet

Este guia usa os seguintes pacotes NuGet:

|Biblioteca|DESCRIÇÃO|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de Autenticação da Microsoft (MSAL.NET)|

## <a name="set-up-your-project"></a>Configurar o seu projeto

Nesta seção, você cria um novo projeto para demonstrar como integrar um aplicativo .NET da Área de Trabalho do Windows (XAML) com a opção *Entrar com uma Conta da Microsoft*, de forma que o aplicativo possa consultar APIs Web que exigem um token.

O aplicativo que você criar com este guia exibe um botão que é usado para chamar um gráfico, uma área para mostrar os resultados na tela e um botão de saída.

> [!NOTE]
> Prefere baixar este projeto do Visual Studio de exemplo? [Baixe um projeto](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip) e vá para a [Etapa de configuração](#register-your-application) para configurar o exemplo de código antes de executá-lo.
>

Para criar seu aplicativo, faça o seguinte:

1. No Visual Studio, selecione **Arquivo** > **Novo** > **Projeto**.
2. Em **Modelos**, selecione **Visual C#** .
3. Selecione **aplicativo WPF (.NET Framework)** , dependendo da versão do Visual Studio que você estiver usando.

## <a name="add-msal-to-your-project"></a>Adicionar MSAL ao seu projeto

1. No Visual Studio, selecione **Ferramentas** > **Gerenciador de Pacotes do NuGet**> **Console do Gerenciador de Pacotes**.
2. Na janela do Console do Gerenciador de Pacotes, cole o seguinte comando do Azure PowerShell:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Esse comando instala a Biblioteca de Autenticação da Microsoft. A MSAL lida com a aquisição, o armazenamento em cache e a atualização de tokens de usuário usados para acessar APIs protegidas pelo Azure Active Directory v2.0
    >

## <a name="register-your-application"></a>Registre seu aplicativo

Você pode registrar seu aplicativo de duas maneiras.

### <a name="option-1-express-mode"></a>Opção 1: Modo Expresso

Registre rapidamente seu aplicativo fazendo o seguinte:
1. Vá para o [portal do Azure – Registro de Aplicativo](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs).
1. Insira um nome para seu aplicativo e selecione **Registrar**.
1. Siga as instruções para baixar e configurar automaticamente o novo aplicativo com apenas um clique.

### <a name="option-2-advanced-mode"></a>Opção 2: Modo Avançado

Para registrar seu aplicativo e adicionar suas informações de registro de aplicativo à sua solução, faça o seguinte:
1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. Se sua conta fornecer acesso a mais de um locatário, selecione sua conta no canto superior direito e defina sua sessão do portal para o locatário desejado do Azure AD.
1. Navegue até a página [Registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) da plataforma de identidade da Microsoft para desenvolvedores.
1. Selecione **Novo registro**.
   - Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo, por exemplo, `Win-App-calling-MsGraph`.
   - Na seção **Tipos de conta com suporte**, selecione **Contas em qualquer diretório organizacional e contas pessoais da Microsoft (por exemplo, Skype, Xbox, Outlook.com)** .
   - Selecione **Registrar** para criar o aplicativo.
1. Na lista de páginas para o aplicativo, selecione **Autenticação**.
   1. Na seção **URIs de Redirecionamento** na lista de URIs de Redirecionamento:
   1. Na coluna **TIPO**, selecione **Cliente público/nativo (dispositivo móvel e desktop)** .
   1. Insira `https://login.microsoftonline.com/common/oauth2/nativeclient` na coluna **URI DE REDIRECIONAMENTO**
1. Selecione **Registrar**.
1. Vá para o Visual Studio, abra o arquivo *App.xaml.cs* e, depois, substitua `Enter_the_Application_Id_here` no snippet de código abaixo pela ID do aplicativo que você acabou de registrar e copiar.

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```

## <a name="add-the-code-to-initialize-msal"></a>Adicionar o código para inicializar a MSAL

Nesta etapa, você cria uma classe para lidar com a interação com a MSAL, por exemplo, com a manipulação de tokens.

1. Abra o arquivo *App.xaml.cs* e adicione a referência para a MSAL à classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. Atualize a classe app para o seguinte:

    ```csharp
    public partial class App : Application
    {
        static App()
        {
            _clientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
        }

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - the content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use `organizations`
        //   - for any Work or School accounts, or Microsoft personal account, use `common`
        //   - for Microsoft Personal account, use consumers
        private static string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";

        private static string Tenant = "common";

        private static IPublicClientApplication _clientApp ;

        public static IPublicClientApplication PublicClientApp { get { return _clientApp; } }
    }
    ```

## <a name="create-the-application-ui"></a>Criar a interface do usuário do aplicativo

Esta seção mostra como um aplicativo pode consultar um servidor de back-end protegido como o Microsoft Graph. 

Um arquivo *MainWindow.xaml* deve ser criado automaticamente como parte de seu modelo de projeto. Abra esse arquivo e, em seguida, substitua o nó *\<Grade>* de seu aplicativo pelo código a seguir:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Usar a MSAL para obter um token para a API do Microsoft Graph

Nesta seção, você usará a MSAL para obter um token da API do Microsoft Graph.

1. No arquivo *MainWindow.xaml.cs*, adicione a referência da MSAL à classe:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Substitua o código de classe `MainWindow` pelo seguinte:

    ```csharp
    public partial class MainWindow : Window
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };


        public MainWindow()
        {
            InitializeComponent();
        }

      /// <summary>
        /// Call AcquireToken - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            var app = App.PublicClientApp;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;

            var accounts = await app.GetAccountsAsync();
            var firstAccount = accounts.FirstOrDefault();

            try
            {
                authResult = await app.AcquireTokenSilent(scopes, firstAccount)
                    .ExecuteAsync();
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilent.
                // This indicates you need to call AcquireTokenInteractive to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

                try
                {
                    authResult = await app.AcquireTokenInteractive(scopes)
                        .WithAccount(accounts.FirstOrDefault())
                        .WithPrompt(Prompt.SelectAccount)
                        .ExecuteAsync();
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }

            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Mais informações

#### <a name="get-a-user-token-interactively"></a>Obter um token de usuário interativamente

A chamada ao método `AcquireTokenInteractive` resulta em uma janela que solicita a conexão do usuário. Os aplicativos geralmente exigem que os usuários façam logon interativamente na primeira vez que precisarem acessar um recurso protegido. Eles também podem precisar entrar quando uma operação silenciosa para adquirir um token falhar (por exemplo, quando a senha do usuário tiver expirado).

#### <a name="get-a-user-token-silently"></a>Obter um token de usuário no modo silencioso

O método `AcquireTokenSilent` manipula as aquisições e renovações de tokens sem nenhuma interação do usuário. Depois que `AcquireTokenInteractive` for executado pela primeira vez, `AcquireTokenSilent` será o método comum usado para obter tokens que acessam recursos protegidos nas próximas chamadas – já que as chamadas para solicitar ou renovar tokens são feitas no modo silencioso.

Por fim, o método `AcquireTokenSilent` falhará. Os motivos da falha podem ser que o usuário se desconectou ou alterou a senha em outro dispositivo. Quando a MSAL detecta que o problema pode ser resolvido com a solicitação de uma ação interativa, ela dispara uma exceção `MsalUiRequiredException`. O aplicativo pode tratar essa exceção de duas maneiras:

* Ele pode fazer uma chamada para `AcquireTokenInteractive` imediatamente. Essa chamada resulta na solicitação de entrada do usuário. Esse padrão geralmente é usado nos aplicativos online em que não há nenhum conteúdo offline disponível para o usuário. O exemplo gerado por essa configuração interativa segue esse padrão, que você pode ver em ação na primeira vez que executar o exemplo. 

* Como nenhum usuário usou o aplicativo, `PublicClientApp.Users.FirstOrDefault()` conterá um valor nulo e uma exceção `MsalUiRequiredException` será lançada. 

* Em seguida, o código no exemplo trata a exceção chamando `AcquireTokenInteractive`, o que resulta na solicitação de entrada do usuário.

* Ele também pode apresentar uma indicação visual aos usuários informando que uma entrada interativa é necessária, para que eles possam selecionar a hora certa de entrar. O aplicativo também pode tentar novamente `AcquireTokenSilent` mais tarde. Esse padrão é usado com frequência quando os usuários podem usar outras funcionalidades do aplicativo sem interrupções, por exemplo, quando o conteúdo offline está disponível no aplicativo. Nesse caso, os usuários podem decidir quando desejam entrar para acessar o recurso protegido ou para atualizar as informações desatualizadas. Como alternativa, o aplicativo pode optar por repetir `AcquireTokenSilent` quando a rede é restaurada depois de estar temporariamente indisponível.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Chamar a API do Microsoft Graph usando o token obtido recentemente

Adicione o seguinte método a `MainWindow.xaml.cs`. O método é usado para fazer uma solicitação `GET` na API do Graph usando um cabeçalho de autorização:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Mais informações sobre como fazer uma chamada REST em uma API protegida

Neste exemplo de aplicativo, use o método `GetHttpContentWithToken` para fazer uma solicitação HTTP `GET` em um recurso protegido que exige um token e, em seguida, retornar o conteúdo para o chamador. Esse método adiciona o token adquirido no cabeçalho de Autorização HTTP. Para este exemplo, o recurso é o ponto de extremidade *me* da API do Microsoft Graph – que exibe as informações de perfil do usuário.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Adicionar um método para desconectar um usuário

Para desconectar um usuário, adicione o seguinte método ao arquivo `MainWindow.xaml.cs`:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private async void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    var accounts = await App.PublicClientApp.GetAccountsAsync(); 

    if (accounts.Any())
    {
        try
        {
            await App.PublicClientApp.RemoveAsync(accounts.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Mais informações sobre como desconectar o usuário

O método `SignOutButton_Click` acima remove os usuários do cache de usuário da MSAL, o que diz para a MSAL esquecer o usuário atual, de modo que uma solicitação futura de aquisição de um token seja bem sucedida apenas se for feita para ser interativa.

Embora o aplicativo neste exemplo ofereça suporte a um único usuário, a MSAL oferece suporte a cenários em que várias contas podem estar conectadas ao mesmo tempo. Um exemplo é um aplicativo de email onde um usuário tem várias contas.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Exibir informações básicas de token

Para exibir informações básicas sobre o token, adicione o seguinte método ao arquivo *MainWindow.xaml.cs*:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Username: {authResult.Account.Username}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Mais informações

Além do token de acesso que é usado para chamar a API do Microsoft Graph, após o usuário se autenticar, a MSAL também obterá um token de ID. Esse token contém um pequeno subconjunto de informações pertinentes aos usuários. O método `DisplayBasicTokenInfo` exibe as informações básicas contidas no token. Por exemplo, ele mostra o nome de exibição do usuário e a ID, bem como a data de expiração do token e a cadeia de caracteres que representa o token de acesso em si. Pressione o botão *Chamar API do Microsoft Graph* várias vezes e veja que o mesmo token foi reutilizado em solicitações posteriores. Veja também a data de expiração que está sendo estendida quando a MSAL decide a hora de renovar o token.
<!--end-collapse-->

[!INCLUDE [5. Test and Validate](../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]

Ajude-nos a melhorar a plataforma de identidade da Microsoft. Deixe sua opinião respondendo a uma breve pesquisa de duas perguntas.

> [!div class="nextstepaction"]
> [Pesquisa da plataforma de identidade da Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
