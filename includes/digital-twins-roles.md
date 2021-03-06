---
title: incluir arquivo
description: incluir arquivo
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: 864d693f2919922b5035f963103b2ce98600a51c
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044959"
---
A tabela a seguir descreve as funções que estão disponíveis no Gêmeos Digitais do Azure:

| **Função** | **Descrição** | **Identificador** |
| --- | --- | --- |
| Administrador de Espaço | Permissão *CREATE*, *READ*, *UPDATE* e *DELETE* para o espaço especificado e todos os nós abaixo. Permissão global. | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Administrador de usuários| Permissão *CREATE*, *READ*, *UPDATE* e *DELETE* para usuários e objetos relacionados ao usuário. Permissão *READ* para espaços. | dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Administrador de Dispositivo | Permissão *CREATE*, *READ*, *UPDATE* e *DELETE* para dispositivos e objetos relacionados aos dispositivos. Permissão *READ* para espaços. | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Administrador de Chave | Permissão *CREATE*, *READ*, *UPDATE* e *DELETE* para chaves de acesso. Permissão *READ* para espaços. | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Administrador de Token |  Permissão *READ* e *UPDATE* para chaves de acesso. Permissão *READ* para espaços. | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Usuário |  Permissão *READ* para espaços, sensores e usuários, incluindo seus respectivos objetos relacionados. | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Especialista de Suporte |  Permissão *READ* para tudo, exceto chaves de acesso. | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Instalador do Dispositivo | Permissão *READ* e *UPDATE* para dispositivos e sensores, incluindo seus respectivos objetos relacionados. Permissão *READ* para espaços. | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Dispositivos de gateway | Permissão *CREATE* para sensores. Permissão *READ* para dispositivos e sensores, incluindo seus respectivos objetos relacionados. | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |