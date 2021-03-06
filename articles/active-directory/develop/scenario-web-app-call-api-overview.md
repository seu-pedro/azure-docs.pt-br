---
title: Compilar um aplicativo Web que chama APIs da Web-plataforma de identidade da Microsoft | Azure
description: Saiba como criar um aplicativo Web que chama APIs da Web (visão geral)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9e123195205bb0eb88f0edd4e2dff2e0da9d84ce
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701647"
---
# <a name="scenario-web-app-that-calls-web-apis"></a>Cenário: aplicativo Web que chama APIs da Web

Saiba como criar um aplicativo Web que conecta usuários na plataforma de identidade da Microsoft e, em seguida, chama APIs da Web em nome do usuário conectado.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

Este cenário pressupõe que você tenha passado pelo seguinte cenário:

> [!div class="nextstepaction"]
> [Aplicativo Web que faz logon de usuários](scenario-web-app-sign-user-overview.md)

## <a name="overview"></a>Visão Geral

Você adiciona autenticação ao seu aplicativo Web, que, portanto, pode conectar usuários e chamar uma API da Web em nome do usuário conectado.

![Aplicativo Web que chama as APIs Web](./media/scenario-webapp/web-app.svg)

Aplicativos Web que chamam APIs da Web:

- são aplicativos cliente confidenciais.
- é por isso que eles registraram um segredo (senha de aplicativo ou certificado) com o Azure AD. Esse segredo é passado durante a chamada para o Azure AD para obter um token

## <a name="specifics"></a>Especificações

> [!NOTE]
> Adicionar a entrada a um aplicativo Web não usa as bibliotecas MSAL, pois isso é sobre a proteção do aplicativo Web. A proteção de bibliotecas é obtida por bibliotecas denominadas middleware. Esse era o objeto do cenário anterior [que os usuários entram em um aplicativo Web](scenario-web-app-sign-user-overview.md)
>
> Ao chamar APIs Web de um aplicativo Web, você precisará obter tokens de acesso para essas APIs da Web. Você pode usar bibliotecas MSAL para adquirir esses tokens.

A experiência de ponta a ponta dos desenvolvedores para esse cenário tem, portanto, aspectos específicos como:

- Durante o [registro do aplicativo](scenario-web-app-call-api-app-registration.md), você precisará fornecer um, ou vários (se você implantar seu aplicativo em vários locais), URIs de resposta, segredos ou certificados precisam ser compartilhados com o Azure AD.
- A [configuração do aplicativo](scenario-web-app-call-api-app-configuration.md) precisa fornecer credenciais de cliente como compartilhadas com o Azure ad durante o registro do aplicativo

## <a name="next-steps"></a>Próximos passos

> [!div class="nextstepaction"]
> [Registro do Aplicativo](scenario-web-app-call-api-app-registration.md)
