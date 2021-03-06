---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 0ba21fe3789fba03cd4814d19fb103c3a2559c13
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75662929"
---
| Grupos | Limite máximo/padrão | Observação |
| --- | --- | --- |
| Application Gateway do Azure |1\.000 por assinatura | |
| Configurações de IP de front-end |2 |1 pública e 1 privada |
| Portas de front-end |100<sup>1</sup> | |
| Pools de endereços de back-end |100<sup>1</sup> | |
| Servidores de back-end por pool |1\.200 | |
| Ouvintes HTTP |100<sup>1</sup> | |
| Regras de balanceamento de carga HTTP |100<sup>1</sup> | |
| Configurações de HTTP de back-end |100<sup>1</sup> | |
| Instâncias por gateway |32 | |
| Certificado SSL |100<sup>1</sup> |1 por ouvintes HTTP |
| Tamanho máximo do certificado SSL |V1 SKU - 10 KB<br>V2 SKU - 16 KB| |
| Certificados de autenticação |100 | |
| Certificados raiz confiáveis |100 | |
| Tempo limite mínimo da solicitação |1 segundo | |
| Tempo limite máximo da solicitação |24 horas | |
| Número de sites |100<sup>1</sup> |1 por ouvintes HTTP |
| Mapas de URL por ouvinte |1 | |
| Regras com base em caminho máximo por mapa de URL|100||
| Configurações de redirecionamento |100<sup>1</sup>| |
| Conexões WebSocket Simultâneas |Gateways médios 20 mil<br> 50 mil de gateways grandes| |
| Tamanho máximo da URL|32 KB| |
| Tamanho máximo do cabeçalho para HTTP/2 |4| |
| Tamanho máximo de carregamento de arquivo, padrão |2 GB | |
| Tamanho de carregamento de arquivo máximo WAF |gateways de WAF médio v1, 100 MB<br>WAF gateways de grande escala v1, 500 MB<br>v2 WAF, 750 MB| |
| Limite de tamanho de corpo de WAF, sem arquivos|128 KB||
| WAF máximo de regras personalizadas|100||
| Máximo de exclusões de WAF|100||

<sup>1</sup> no caso de SKUs habilitadas para WAF, recomendamos que você limite o número de recursos para 40 para o desempenho ideal.
