---
title: Exemplos de código para a plataforma Microsoft Identity | Microsoft Docs
description: Fornece um índice de exemplos de código disponíveis da plataforma de identidade da Microsoft (ponto de extremidade v 2.0), organizado por cenário.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: ddffd75d960fe981382d880340d4bf61464cce3c
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702531"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Exemplos de código da plataforma Microsoft Identity (ponto de extremidade v 2.0)

Você pode usar a plataforma de identidade da Microsoft para:

- Adicionar autenticação e autorização para aplicativos Web e APIs da Web.
- Solicitar um token de acesso para acessar uma API da Web protegida.

Este artigo descreve brevemente e fornece links para exemplos do ponto de extremidade da plataforma Microsoft Identity. Esses exemplos mostram como isso é feito e também fornecem trechos de código que você pode usar em seus aplicativos. Na página de exemplo de código, você encontrará tópicos detalhados do Leiame que ajudam com requisitos, instalação e configuração. Os comentários dentro do código ajudam você a entender as seções críticas.

> [!NOTE]
> Se você estiver interessado em exemplos do v 1.0, consulte [exemplos de código do Azure AD (ponto de extremidade v 1.0)](sample-v1-code.md).

Para entender o cenário básico para cada tipo de exemplo, consulte [tipos de aplicativo para o ponto de extremidade da plataforma Microsoft Identity](v2-app-types.md).

Você também pode contribuir com os exemplos no GitHub. Para saber como, consulte [Exemplos e documentação do Microsoft Azure Active Directory](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Aplicativos de página única

Estes exemplos mostram como escrever um aplicativo de página única protegido com a plataforma de identidade da Microsoft. Esses exemplos usam um dos tipos de MSAL. js.

| Plataforma | Description | Link |
| -------- | --------------------- | -------- |
| ![esta imagem mostra o logotipo do JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (MSAL. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Chama o Microsoft Graph |[javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![esta imagem mostra o logotipo do JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (MSAL. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Chamadas B2C |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![esta imagem mostra o logotipo do JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (MSAL. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Chama sua própria API Web |[javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |
| ![esta imagem mostra o logotipo angular JS](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL AngularJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)| Chama o Microsoft Graph  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/MsalAngularjsDemoApp)
| ![esta imagem mostra o logotipo angular](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| Chama o Microsoft Graph  | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/MSALAngularDemoApp) |

## <a name="web-applications"></a>Aplicativos Web

Os exemplos a seguir ilustram os aplicativos Web que iniciam sessão de usuários. Alguns exemplos também demonstram o aplicativo chamando o Microsoft Graph ou sua própria API Web com a identidade do usuário.

| Plataforma | Conecta apenas usuários | Conecta usuários e chama o Microsoft Graph |
| -------- | ------------------- | --------------------------------- |
| ![Esta imagem mostra o logotipo ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2,2 | [Tutorial de usuários de entrada do ASP.NET Core WebApp](https://aka.ms/aspnetcore-webapp-sign-in) | O mesmo exemplo no [aplicativo Web ASP.NET Core chama Microsoft Graph](https://aka.ms/aspnetcore-webapp-call-msgraph) fase |
| ![Esta imagem mostra o logotipo do ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [Início rápido do ASP.NET](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Esta imagem mostra o logotipo do Java](media/sample-v2-code/logo_java.png)  |                   | [MS-Identity-Java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp) |
| ![Esta imagem mostra o logotipo do Python](media/sample-v2-code/logo_python.png)  |                   | [MS-Identity-Python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp) |
| ![Esta imagem mostra o logotipo do node. js](media/sample-v2-code/logo_nodejs.png)  |                   | [Início rápido do Node.js](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs) |
| ![Esta imagem mostra o logotipo do Ruby](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Aplicativos cliente públicos de desktop e móveis

Os exemplos a seguir mostram aplicativos cliente públicos (aplicativos móveis ou de desktop) que acessam a API de Microsoft Graph ou sua própria API Web no nome de um usuário. Todos esses aplicativos cliente usam a MSAL (biblioteca de autenticação da Microsoft).

| Aplicativo cliente | Plataforma | Fluxo/Concessão | Chama o Microsoft Graph | Chama uma API Web do ASP.NET Core 2,0 |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Desktop (WPF)      | ![Esta imagem mostra o .NET/C# logotipo](media/sample-v2-code/logo_NET.png) | [interativo](msal-authentication-flows.md#interactive)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo .NETC# /(Desktop)](media/sample-v2-code/logo_NET.png) | [Autenticação Integrada do Windows](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo .NETC# /(Desktop)](media/sample-v2-code/logo_NETcore.png) | [Nome de usuário/senha](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo do Java](media/sample-v2-code/logo_java.png) | [Nome de usuário/senha](msal-authentication-flows.md#usernamepassword) |[MS-Identity-Java-Desktop](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo do Python](media/sample-v2-code/logo_python.png) | [Nome de usuário/senha](msal-authentication-flows.md#usernamepassword) |[MS-Identity-Python-desktop](https://github.com/Azure-Samples/ms-identity-python-desktop) |  |
| Móveis (Android, iOS, UWP)   | ![Esta imagem mostra o logotipo doC# .net/(Xamarin)](media/sample-v2-code/logo_xamarin.png) | [interativo](msal-authentication-flows.md#interactive) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobile (iOS)       | ![Esta imagem mostra o iOS/Objective-C ou Swift](media/sample-v2-code/logo_iOS.png) | [interativo](msal-authentication-flows.md#interactive) |[Ios-Swift-ObjC-Native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Área de trabalho (macOS)       | macOS | [interativo](msal-authentication-flows.md#interactive) |[macOS-Swift-ObjC-Native-v2](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) |  |
| Móvel (Android-Java)   | ![Esta imagem mostra o logotipo do Android](media/sample-v2-code/logo_Android.png) | [interativo](msal-authentication-flows.md#interactive) |  [Android-Java](https://github.com/Azure-Samples/ms-identity-android-java) |  |
| Móvel (Android-Kotlin)   | ![Esta imagem mostra o logotipo do Android](media/sample-v2-code/logo_Android.png) | [interativo](msal-authentication-flows.md#interactive) |  [Android-Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin) |  |

## <a name="daemon-applications"></a>Aplicativos de daemon

Os exemplos a seguir mostram um aplicativo que acessa a API do Microsoft Graph com sua própria identidade (sem usuários).

| Aplicativo cliente | Plataforma | Fluxo/Concessão | Chama o Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Console | ![Esta imagem mostra o logotipo do .NET Core](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [Credenciais do cliente](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Aplicativo Web | ![Esta imagem mostra o logotipo do ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [Credenciais do cliente](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |
| Console | ![Esta imagem mostra o logotipo do Java](media/sample-v2-code/logo_java.png) | [Credenciais do cliente](msal-authentication-flows.md#client-credentials) | [MS-Identity-Java-daemon](https://github.com/Azure-Samples/ms-identity-java-daemon) |
| Console | ![Esta imagem mostra o logotipo do Python](media/sample-v2-code/logo_python.png) | [Credenciais do cliente](msal-authentication-flows.md#client-credentials) | [MS-Identity-Python-daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) |

## <a name="headless-applications"></a>Aplicativos sem periféricos

O exemplo a seguir mostra um aplicativo cliente público em execução em um dispositivo sem um navegador da Web. O aplicativo pode ser uma ferramenta de linha de comando, um aplicativo em execução no Linux ou Mac, ou um aplicativo de IoT. O exemplo apresenta um aplicativo que acessa a API Microsoft Graph, no nome de um usuário que entra de forma interativa em outro dispositivo (como um telefone celular). Este aplicativo cliente usa a biblioteca de autenticação da Microsoft (MSAL).

| Aplicativo cliente | Plataforma | Fluxo/Concessão | Chama o Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo .NETC# /(Desktop)](media/sample-v2-code/logo_NETcore.png) | [Fluxo de código do dispositivo](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo do Java](media/sample-v2-code/logo_java.png) | [Fluxo de código do dispositivo](msal-authentication-flows.md#device-code) |[MS-Identity-Java-devicecodeflow](https://github.com/Azure-Samples/ms-identity-java-devicecodeflow) |
| Área de trabalho (Console)   | ![Esta imagem mostra o logotipo do Python](media/sample-v2-code/logo_python.png) | [Fluxo de código do dispositivo](msal-authentication-flows.md#device-code) |[MS-Identity-Python-devicecodeflow](https://github.com/Azure-Samples/ms-identity-python-devicecodeflow) |

## <a name="web-apis"></a>APIs da Web

Os exemplos a seguir mostram como proteger uma API Web com o ponto de extremidade da plataforma de identidade da Microsoft e como chamar uma API downstream da API da Web.

| Plataforma | Amostra |
| -------- | ------------------- |
| ![Esta imagem mostra o logotipo ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2,2 | API Web do ASP.NET Core (serviço) de [dotnet-Native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![Esta imagem mostra o logotipo do ASP.NET](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | API Web (serviço) de [MS-Identity-ASPNET-webAPI-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |
| ![Esta imagem mostra o logotipo do Java](media/sample-v2-code/logo_java.png) | API Web (serviço) de [MS-Identity-Java-webAPI](https://github.com/Azure-Samples/ms-identity-java-webapi) |

## <a name="azure-functions-as-web-apis"></a>Azure Functions como APIs Web

Os exemplos a seguir mostram como proteger uma função do Azure usando HttpTrigger e expondo uma API Web com o ponto de extremidade da plataforma de identidade da Microsoft e como chamar uma API downstream da API da Web.

| Plataforma | Amostra |
| -------- | ------------------- |
| ![Esta imagem mostra o logotipo ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2,2 | ASP.NET Core a função do Azure da API Web (serviço) do [dotnet-Native-aspnetcore-v2](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions)  |
| ![Esta imagem mostra o logotipo do node. js](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | API Web (serviço) de [NodeJS e Passport-Azure-ad](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) |
| ![Esta imagem mostra o logotipo do Python](media/sample-v2-code/logo_python.png)</p>Python | API da Web (serviço) do [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions) |
| ![Esta imagem mostra o logotipo do node. js](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | API da Web (serviço) de [NodeJS e Passport-Azure-ad usando em nome de](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions) |

## <a name="other-microsoft-graph-samples"></a>Outros exemplos do Microsoft Graph

Para saber mais sobre [exemplos](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) e tutoriais que demonstram os diferentes padrões de uso para a API do Microsoft Graph, incluindo autenticação com o Microsoft Azure AD, consulte [Exemplos e tutoriais da Comunidade do Microsoft Graph](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Consulte também

- [Guia do desenvolvedor de Azure Active Directory (v 1.0)](v1-overview.md)
- [Conceitos e referência da API do Graph do Microsoft Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Biblioteca auxiliar da API do Graph do Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
