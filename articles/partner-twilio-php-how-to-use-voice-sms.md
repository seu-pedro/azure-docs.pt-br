---
title: Como usar o Twilio para voz e SMS (PHP) | Microsoft Docs
description: Saiba como fazer uma chamada telefônica e enviar uma mensagem SMS com o serviço de API do Twilio no Azure. Exemplos de código escritos em PHP.
documentationcenter: php
services: ''
author: georgewallace
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: 34057f1962338927a252011dccc56ed6a77bec47
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69636033"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Como usar o Twilio para obter recursos de voz e SMS no PHP
Este guia demonstra como executar tarefas comuns de programação com o serviço de API do Twilio no Azure. Os cenários abrangidos incluem fazer uma chamada telefônica e enviar uma mensagem serviço de mensagem curta (SMS). Para obter mais informações sobre o Twilio e o uso de voz e SMS em seus aplicativos, consulte a seção [Próximas etapas](#NextSteps) .

## <a id="WhatIs"></a>O que é Twilio?
Twilio é alimentar o futuro das comunicações de negócios, que permite aos desenvolvedores incorporar voz, VoIP e mensagens em aplicativos. Eles virtualizam toda a infra-estrutura necessária em um ambiente baseado em nuvem, global, expondo-as por meio da plataforma de comunicações API Twilio. Os aplicativos são simples de criar e dimensionável. Aproveite a flexibilidade com pagamento medida que vá preços e se beneficiar da confiabilidade da nuvem.

**Twilio Voice** permite que seus aplicativos façam e recebam chamadas telefônicas. **SMS da Twilio** permite que seu aplicativo envie e receba mensagens de texto. **Cliente de Twilio** permite que você faça chamadas VoIP de qualquer telefone, tablet ou navegador e oferece suporte a WebRTC.

## <a id="Pricing"></a>Preços e ofertas especiais da Twilio
Os clientes do Azure recebem uma [oferta especial](https://www.twilio.com/azure): US$ 10 de cortesia de crédito da Twilio quando atualizam a sua conta da Twilio. Esse crédito de Twilio pode ser aplicado a qualquer uso de Twilio (equivalente a até 1.000 mensagens SMS de envio ou recebimento de até 1000 minutos de voz entrados, dependendo da localização do seu destino de chamada e mensagem ou número de telefone de crédito de US $10). Resgate esse crédito do Twilio e comece a usar em: [https://ahoy.twilio.com/azure](https://ahoy.twilio.com/azure).

Twilio é um serviço flexível. Não há nenhuma taxa de configuração e você pode fechar sua conta a qualquer momento. Você pode encontrar mais detalhes em [Preços da Twilio][twilio_pricing].

## <a id="Concepts"></a>Conceitos
A API do Twilio é uma API RESTful que fornece os recursos de voz e SMS para aplicativos. As bibliotecas de cliente estão disponíveis em vários idiomas; para obter uma lista, consulte [bibliotecas de API do twilio][twilio_libraries].

Principais aspectos da API do Twilio são Twilio verbos e linguagem de marcação de Twilio (TwiML).

### <a id="Verbs"></a>Verbos da Twilio
A API usa os verbos do Twilio; por exemplo, o verbo **&lt;Say&gt;** instrui o Twilio a fornecer de forma audível uma mensagem em uma chamada.

A seguir está uma lista de verbos do Twilio. Saiba mais sobre os outros verbos e recursos na [Documentação da linguagem de marcação da Twilio](https://www.twilio.com/docs/api/twiml).

* **&lt;Dial&gt;** : Conecta o chamador a outro telefone.
* **&lt;Gather&gt;** : Coleta os dígitos numéricos inseridos no teclado do telefone.
* **&lt;Hangup&gt;** : Encerra uma chamada.
* **&lt;Play&gt;** : Reproduz um arquivo de áudio.
* **&lt;Pause&gt;** : Espera silenciosamente por um número especificado de segundos.
* **&lt;Record&gt;** : Grava a voz do chamador e retorna uma URL de um arquivo que contém a gravação.
* **&lt;Redirect&gt;** : Transfere o controle de uma chamada ou SMS para o TwiML em uma URL diferente.
* **&lt;Reject&gt;** : Rejeita uma chamada recebida para o número do Twilio sem cobrança
* **&lt;Say&gt;** : Faz conversão de texto em fala realizada em uma chamada.
* **&lt;Sms&gt;** : Envia uma mensagem SMS.

### <a id="TwiML"></a>TwiML
TwiML é um conjunto de instruções em XML com base nos verbos do Twilio que informam o Twilio como processar uma chamada ou SMS.

Por exemplo, o seguinte TwiML converteria a mensagem **Olá, mundo** em fala.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Quando o aplicativo chama a API Twilio, um dos parâmetros a API é a URL que retorna a resposta TwiML. Para fins de desenvolvimento, você pode usar URLs fornecidos Twilio para fornecer as respostas de TwiML usadas por seus aplicativos. Você também pode hospedar seus próprio URLs para produzir as respostas TwiML e outra opção é usar o **TwiMLResponse** objeto.

Para obter mais informações sobre verbos twilio, seus atributos e TwiML, consulte [TwiML][twiml]. Para obter informações adicionais sobre a API do twilio, consulte [API do twilio][twilio_api].

## <a id="CreateAccount"></a>Criar uma conta na Twilio
Quando estiver pronto para obter uma conta do twilio, Inscreva-se em [experimentar o twilio][try_twilio]. Você pode começar com uma conta gratuita e atualizá-la depois.

Quando você se inscrever para uma conta de Twilio, você receberá uma ID de conta e um token de autenticação. Eles serão necessários para fazer chamadas de API do Twilio. Para evitar o acesso não autorizado em sua conta, mantenha o token da autenticação seguro. A ID da conta e o token de autenticação são visíveis na [página da conta do twilio][twilio_account], nos campos ROTULAdos SID da **conta** e **token de autenticação**, respectivamente.

## <a id="create_app"></a>Criar um aplicativo PHP
Um aplicativo PHP que usa o serviço do Twilio e está em execução no Azure não é diferente de qualquer outro aplicativo PHP que usa o serviço do Twilio. Embora os serviços Twilios sejam baseados em REST e possam ser chamados do PHP de várias maneiras, este artigo se concentrará em como usar os serviços do twilio com a [biblioteca twilio para php do GitHub][twilio_php]. Para obter mais informações sobre como usar a biblioteca twilio para PHP [https://www.twilio.com/docs/libraries/php][twilio_lib_docs], consulte.

Instruções detalhadas para criar e implantar um aplicativo twilio/PHP no Azure estão disponíveis em [como fazer uma chamada telefônica usando o twilio em um aplicativo PHP no Azure][howto_phonecall_php].

## <a id="configure_app"></a>Configurar seu aplicativo para usar bibliotecas Twilio
Você pode configurar seu aplicativo para usar a biblioteca do Twilio para PHP de duas maneiras:

1. Baixe a biblioteca twilio para php do GitHub ([https://github.com/twilio/twilio-php][twilio_php]) e adicione o diretório de **Serviços** ao seu aplicativo.
   
    -OU-
2. Instalar a biblioteca da Twilio para PHP como um pacote PEAR. Ela pode ser instalada com os seguintes comandos:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Depois de instalar a biblioteca da Twilio para PHP, você pode adicionar uma instrução **require_once** na parte superior dos arquivos PHP para fazer referência à biblioteca:

        require_once 'Services/Twilio.php';

Para obter mais informações, consulte [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Como: Como fazer uma chamada de saída
Abaixo é mostrado como fazer uma chamada externa usando a classe **Services_Twilio**. Esse código também usa um site fornecido pelo Twilio para retornar a resposta TwiML (Linguagem de Marcação do Twilio). Substitua os valores dos números telefônico **De** e **Para** e certifique-se de verificar o número telefônico **De** para sua conta do Twilio antes de executar o código.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "https://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Como mencionado, esse código utiliza um site fornecido pelo Twilio para retornar a resposta de TwiML. Em vez disso, você pode usar seu próprio site para fornecer a resposta TwiML. Para obter mais informações, consulte [Como fornecer respostas TwiML em seu próprio site](#howto_provide_twiml_responses).

* **Observação**: Para solucionar erros de validação de certificado SSL, consulte[http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Como: Enviar uma mensagem SMS
Abaixo é mostrado como enviar uma mensagem SMS usando a classe **Services_Twilio**. O **de** número é fornecido por Twilio para contas de avaliação para enviar mensagens SMS. O número **Para** deve ser verificado para sua conta de Twilio antes de executar o código.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Como: Fornecer respostas TwiML de seu próprio site
Quando o aplicativo inicia uma chamada para a API do Twilio, o Twilio enviará a solicitação a uma URL que deve retornar uma resposta TwiML. O exemplo acima usa a URL [https://twimlets.com/message][twimlet_message_url]fornecida pelo twilio. (Embora o TwiML tenha sido criado para uso do Twilio, você pode exibi-lo em seu navegador. Por exemplo, clique [https://twimlets.com/message][twimlet_message_url] para ver um elemento `<Response>` vazio; como outro exemplo, clique [https://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] para ver um `<Response>` elemento que contém um `<Say>` elemento.)

Em vez de contar com a URL fornecida pela Twilio, você pode criar seu próprio site que retorne respostas HTTP. Você pode criar o site em qualquer linguagem que retorna respostas XML; este tópico pressupõe que você usará PHP para criar o TwiML.

A página PHP seguinte resulta em uma resposta de TwiML que diz **Hello World** na chamada.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Como você pode ver no exemplo acima, a resposta de TwiML é simplesmente um documento XML. A biblioteca da Twilio para PHP contém classes que geram TwiML para você. O exemplo a seguir produz a resposta equivalente conforme mostrado acima, mas usa a classe **Services\_Twilio\_Twiml** na biblioteca da Twilio para PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Para obter mais informações sobre TwiML, consulte [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

Uma vez que a página PHP estiver configurada para fornecer respostas TwiML, use a URL da página PHP como a URL passada para o método `Services_Twilio->account->calls->create`. Por exemplo, se você tiver um aplicativo Web chamado **MyTwiML** implantado em um serviço hospedado do Azure e o nome da página PHP é **mytwiml.php**, a URL poderá ser passada para **Services_Twilio->account->calls->create** conforme mostrado no exemplo a seguir:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Para obter informações adicionais sobre como usar o twilio no Azure com PHP, consulte [como fazer uma chamada telefônica usando o twilio em um aplicativo PHP no Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Como: Usar serviços adicionais do Twilio
Além dos exemplos mostrados aqui, o Twilio oferece APIs baseadas na Web que podem ser usadas para aproveitar a funcionalidade adicional do Twilio do aplicativo Azure. Para obter detalhes completos, consulte a [documentação da API do twilio][twilio_api_documentation].

## <a id="NextSteps"></a>Próximas etapas
Agora que você já conhece os princípios do serviço Twilio, acesse estes links para saber mais:

* [Diretrizes de segurança do twilio][twilio_security_guidelines]
* [Twilio HowTo e código de exemplo][twilio_howtos]
* [Tutoriais de início rápido do twilio][twilio_quickstarts] 
* [Twilio no GitHub][twilio_on_github]
* [Fale com o suporte do twilio][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: https://www.twilio.com/docs/libraries/php
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: https://www.twilio.com/docs/api/errors
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: https://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/api
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
