---
title: Conceitos de eventos ao vivo e saídas ao vivo nos serviços de mídia do Azure v3
titleSuffix: Azure Media Services
description: Este tópico fornece uma visão geral de eventos ao vivo e saídas ao vivo nos serviços de mídia do Azure v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 09/30/2019
ms.author: juliako
ms.openlocfilehash: c1b72f2a84f8cafa1767639cae64fb420b0a997c
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76546037"
---
# <a name="live-events-and-live-outputs-in-media-services"></a>Eventos ao vivo e saídas ao vivo nos serviços de mídia

Os serviços de mídia do Azure permitem que você forneça eventos ao vivo para seus clientes na nuvem do Azure. Para configurar seus eventos de transmissão ao vivo nos serviços de mídia v3, você precisa entender os conceitos discutidos neste artigo.

> [!TIP]
> Para clientes que estão migrando das APIs do Media Services v2, a entidade de **evento ao vivo** substitui o **canal** em v2 e a **saída ao vivo** substitui o **programa**.

## <a name="live-events"></a>Eventos ao vivo

[Eventos ao Vivo](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis pela ingestão e pelo processamento dos feeds de vídeo ao vivo. Quando você cria um evento ao vivo, é criado um ponto de extremidade de entrada primário e secundário que você pode usar para enviar um sinal ao vivo de um codificador remoto. O codificador remoto dinâmico envia o feed de contribuição para esse ponto de extremidade de entrada usando o protocolo de entrada [RTMP](https://www.adobe.com/devnet/rtmp.html) ou [Smooth streaming](https://msdn.microsoft.com/library/ff469518.aspx) (fragmentado-MP4). Para o protocolo de ingestão RTMP, o conteúdo pode ser enviado em claro (`rtmp://`) ou criptografado com segurança na conexão (`rtmps://`). Para o protocolo de ingestão Smooth Streaming, os esquemas de URL compatíveis são `http://` ou `https://`.  

## <a name="live-event-types"></a>Tipos de eventos ao vivo

Um [evento ao vivo](https://docs.microsoft.com/rest/api/media/liveevents) pode ser um dos dois tipos: codificação de passagem ou ativa. Os tipos são definidos durante a criação usando [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType. None**: um codificador ao vivo local envia um fluxo de taxas de bits múltiplas. O fluxo ingerido passa pelo evento ao vivo sem nenhum processamento adicional. Também chamado de modo de passagem.
* **LiveEventEncodingType. Standard**: um codificador ao vivo local envia um fluxo de taxa de bits única para o evento ao vivo e os serviços de mídia criam fluxos de taxas de bits múltiplas. Se o feed de contribuição for de 720p ou de resolução superior, a predefinição de **default720p** codificará um conjunto de 6 pares de resolução/taxa de bits.
* **LiveEventEncodingType. Premium1080p**: um codificador ao vivo local envia um fluxo de taxa de bits única para o evento ao vivo e os serviços de mídia criam fluxos de taxa de bits múltiplas. A predefinição de default1080p especifica o conjunto de saída de pares de resolução/taxa de bits.

### <a name="pass-through"></a>Passagem

![diagrama de exemplo de passagem ao vivo com serviços de mídia](./media/live-streaming/pass-through.svg)

Ao usar o **Evento ao vivo** de passagem, você depende de seu codificador dinâmico local para gerar um fluxo de vídeo com várias taxas de bits e enviá-lo como o feed de contribuição para o Evento ao vivo (usando o protocolo RTMP ou MP4 fragmentado). A seguir, o Evento ao vivo executa os fluxos de vídeo de entrada sem qualquer processamento adicional. Esse evento ao vivo de passagem é otimizado para eventos ao vivo de execução longa ou transmissão ao vivo linear 24x365. Ao criar esse tipo de Evento ao vivo, especifique Nenhum (LiveEventEncodingType.None).

Você pode enviar a contribuição em resoluções de até 4K e em uma taxa de quadros de 60 quadros / segundo, com codecs de vídeo H.264/AVC ou H.265/HEVC e AAC (AAC-LC, HE-AACv1 ou HE-AACv2) codec de áudio. Para obter mais informações, consulte [comparação de tipos de eventos ao vivo](live-event-types-comparison.md).

> [!NOTE]
> Usar um método de passagem é a maneira mais econômica de realizar a transmissão ao vivo quando você está fazendo vários eventos por um longo período de tempo e já investiu em codificadores locais. Veja os detalhes de [preços](https://azure.microsoft.com/pricing/details/media-services/) .
>

Veja um exemplo de código do .NET no [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Codificação ativa  

![diagrama de exemplo de codificação ativa com os serviços de mídia](./media/live-streaming/live-encoding.svg)

Ao usar a codificação ativa com os serviços de mídia, você configura seu codificador ao vivo local para enviar um vídeo de taxa de bits única como o feed de contribuição para o evento ao vivo (usando o protocolo RTMP ou MP4 fragmentado). Em seguida, você configura um evento ao vivo para que ele codifique que o fluxo de taxa de bits única de entrada para um [fluxo de vídeo de várias taxas de bits](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)e disponibiliza a saída para a entrega para reproduzir dispositivos por meio de protocolos como MPEG-Dash, HLS e Smooth streaming.

Ao usar a codificação ativa, você pode enviar o feed de contribuição somente em resoluções até a resolução de uma taxa de quadros de 30 quadros/segundo, com H. 264/AVC Video Codec e o codec de áudio AAC (AAC-LC, HE-AACv1 ou HE-AACv2). Observe que os eventos ao vivo de passagem podem dar suporte a resoluções de até 4K em 60 quadros/segundo. Para obter mais informações, consulte [comparação de tipos de eventos ao vivo](live-event-types-comparison.md).

As resoluções e taxas de bits contidas na saída do codificador ao vivo são determinadas pela predefinição. Se estiver usando um codificador ativo **padrão** (LiveEventEncodingType. Standard), a predefinição *default720p* especificará um conjunto de seis pares de taxa de resolução/bit, indo de 720p a 3,5 Mbps para 192P a 200 Kbps. Caso contrário, se estiver usando um codificador **Premium1080p** Live (LiveEventEncodingType. Premium1080p), a predefinição de *default1080p* especificará um conjunto de seis pares de taxa de resolução/bit, indo de 1080p a 3,5 Mbps para 180p a 200 Kbps. Para obter informações, confira [Predefinições do sistema](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Se você precisar personalizar a predefinição de codificação ativa, abra um tíquete de suporte por meio de portal do Azure. Especifique a tabela desejada de resolução e taxas de bits. Verifique se há apenas uma camada em 720p (se solicitar uma predefinição para um codificador ao vivo padrão) ou em 1080p (se solicitar uma predefinição para um codificador ao vivo Premium1080p) e 6 camadas no máximo.

## <a name="creating-live-events"></a>Criando eventos ao vivo

### <a name="options"></a>Opções

Ao criar um Evento ao vivo, você pode especificar as seguintes opções:

* O protocolo de streaming para o Evento ao vivo (atualmente, os protocolos RTMP e Smooth Streaming são compatíveis).<br/>Não é possível alterar a opção de protocolo enquanto o Evento ao Vivo ou suas Saídas ao Vivo associadas estiverem em execução. Se você precisar de protocolos diferentes, crie um evento ao vivo separado para cada protocolo de streaming.  
* Ao criar o evento, é possível especificar sua inicialização automática. <br/>Quando a inicialização automática é definida como verdadeira, o evento em tempo real será iniciado após a criação. A cobrança começa assim que o Evento ao vivo começa a ser transmitido. É necessário chamar explicitamente o recurso Parar no Evento ao vivo para parar a cobrança adicional. Como alternativa, você pode iniciar o evento quando estiver pronto para iniciar o streaming.

    Para saber mais, confira [Estados e cobrança do Evento ao vivo](live-event-states-billing.md).

* Restrições de IP sobre a ingestão e versão prévia. É possível definir os endereços IP que têm permissão para ingerir um vídeo neste Evento ao vivo. Os endereços IP permitidos podem ser especificados como um endereço IP único (por exemplo, '10.0.0.1'), um intervalo IP usando um endereço IP e uma máscara de sub-rede CIDR (por exemplo, '10.0.0.1/22) ou um intervalo IP usando um endereço IP e uma máscara de sub-rede com notação decimal com ponto (por exemplo, '10.0.0.1(255.255.252.0)').<br/>Se nenhum endereço IP for especificado e não houver definição de regra, nenhum endereço IP será permitido. Para permitir qualquer endereço IP, crie uma regra e defina 0.0.0.0/0.<br/>Os endereços IP devem estar em um dos seguintes formatos: endereço IpV4 com quatro números ou intervalo de endereços CIDR.

    Se você quiser habilitar determinados IPs em seus próprios firewalls ou se quiser restringir as entradas aos seus eventos ao vivo para os endereços IP do Azure, baixe um arquivo JSON dos [intervalos de endereços IP do datacenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653). Para obter detalhes sobre esse arquivo, selecione a seção **detalhes** na página.
    
* Ao criar o evento, você pode optar por ativar as transcrições dinâmicas. <br/> Por padrão, a transcrição ao vivo está desabilitada. Você não pode alterar essa propriedade enquanto o evento ao vivo ou suas saídas dinâmicas associadas estiverem em execução. 

### <a name="naming-rules"></a>Regras de nomenclatura

* O nome de evento ativo máximo é de 32 caracteres.
* O nome deve seguir este padrão de [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) : `^[a-zA-Z0-9]+(-*[a-zA-Z0-9])*$`.

Consulte também as [convenções de nomenclatura de pontos de extremidade de streaming](streaming-endpoint-concept.md#naming-convention).

> [!TIP]
> Para garantir a exclusividade do seu nome de evento ao vivo, você pode gerar um GUID e, em seguida, remover todos os hifens e chaves (se houver). A cadeia de caracteres será exclusiva em todos os eventos ao vivo e seu comprimento será garantido como 32.

## <a name="live-event-ingest-urls"></a>URLs de ingestão de Evento ao vivo

Depois que o evento ao vivo for criado, você poderá obter URLs de ingestão que você fornecerá ao codificador local em tempo real. O codificador dinâmico usa essas URLs para gerar a entrada de um fluxo ao vivo. Para saber mais, confira [Codificadores dinâmicos locais recomendados](recommended-on-premises-live-encoders.md).

Você pode usar URLs intuitivas ou não intuitivas.

> [!NOTE] 
> Para que uma URL de ingestão seja preditiva, defina o modo de "personalização".

* URL não intuitiva

    A URL não intuitivo é o modo padrão no Media Services V3. Você possivelmente obterá o Evento ao vivo rapidamente, mas a URL de ingestão é conhecida apenas ao iniciar o evento ao vivo. A URL será alterada se você parar/iniciar o Evento ao vivo. <br/>Não intuitivo é útil em cenários quando um usuário final deseja transmitir usando um aplicativo em que o aplicativo deseja obter um evento ao vivo e ter uma URL de ingestão dinâmica não é um problema.

    Se um aplicativo cliente não precisar gerar previamente uma URL de ingestão antes da criação do evento ao vivo, permita que os serviços de mídia gerem automaticamente o token de acesso para o evento ao vivo.

* URL intuitiva

    O modo intuitivo é preferido por difusores de mídia grandes que usam codificadores de difusão de hardware e não desejam reconfigurar seus codificadores quando eles iniciam o evento ao vivo. Eles querem uma URL de ingestão preditiva, que não é alterada ao longo do tempo.

    Para especificar esse modo, defina `vanityUrl` como `true` no momento da criação (o padrão é `false`). Você também precisa passar seu próprio token de acesso (`LiveEventInput.accessToken`) no momento da criação. Especifique o valor do token para evitar um token aleatório na URL. O token de acesso deve ser uma cadeia de caracteres GUID válida (com ou sem os hifens). Depois que o modo for definido, ele não poderá ser atualizado.

    O token de acesso precisa ser exclusivo em seu data center. Se seu aplicativo precisar usar uma URL intuitivo, é recomendável sempre criar uma nova instância de GUID para seu token de acesso (em vez de reutilizar qualquer GUID existente).

    Use as APIs a seguir para habilitar a URL intuitivo e definir o token de acesso para um GUID válido (por exemplo, `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`).  

    |Idioma|Habilitar URL intuitivo|Definir token de acesso|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--vanity-url](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--access-token](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Regras de nomenclatura de URL de ingestão dinâmica

* A cadeia de caracteres *aleatória* abaixo é um número hexadecimal de 128 bits composto de 32 caracteres de “0” a “9” e “a” a “f”.
* *seu token de acesso*: a cadeia de caracteres GUID válida que você define ao usar o modo intuitivo. Por exemplo, `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *nome do fluxo*: indica o nome do fluxo para uma conexão específica. O valor do nome do fluxo geralmente é adicionado pelo codificador ao vivo que você usa. Você pode configurar o codificador ao vivo para usar qualquer nome para descrever a conexão, por exemplo: "video1_audio1", "video2_audio1", "fluxo".

#### <a name="non-vanity-url"></a>URL não intuitiva

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>URL intuitiva

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>URL de visualização do Evento ao vivo

Depois que o evento ao vivo começar a receber o feed de contribuição, você poderá usar seu ponto de extremidade de visualização para visualizar e validar que você está recebendo a transmissão ao vivo antes da publicação adicional. Depois de verificar se o fluxo de visualização é bom, você pode usar o evento ao vivo para disponibilizar o Live Stream para entrega por meio de um ou mais pontos de extremidade de streaming (pré-criados). Para fazer isso, crie uma nova [saída ao vivo](https://docs.microsoft.com/rest/api/media/liveoutputs) no evento ao vivo.

> [!IMPORTANT]
> Certifique-se de que o vídeo está fluindo para a URL de visualização antes de continuar!

## <a name="live-event-long-running-operations"></a>Operações de longa execução de evento ao vivo

Para obter detalhes, consulte [operações de execução longa](media-services-apis-overview.md#long-running-operations).

## <a name="live-outputs"></a>Saídas ao Vivo

Uma vez que o fluxo esteja fluindo para o Evento ao vivo, é possível começar o evento de transmissão criando um [Ativo](https://docs.microsoft.com/rest/api/media/assets), uma [Saída ao vivo](https://docs.microsoft.com/rest/api/media/liveoutputs) e um [Localizador de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators). A Saída ao vivo arquiva o fluxo e o disponibiliza para usuários por meio do [Ponto de extremidade de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

Para obter informações detalhadas sobre as saídas ao vivo, consulte [usando um DVR de nuvem](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Fazer perguntas, comentar, obter atualizações

Confira o artigo [comunidade dos Serviços de Mídia do Azure](media-services-community.md) para ver diferentes maneiras de fazer perguntas, comentários e obter atualizações sobre os serviços de mídia.

## <a name="next-steps"></a>Próximos passos

[Tutorial de live streaming](stream-live-tutorial-with-api.md)
