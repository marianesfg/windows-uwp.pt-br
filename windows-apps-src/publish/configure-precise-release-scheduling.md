---
Description: Você pode definir a data e a hora exatas em que o aplicativo será disponibilizado na Loja, o que lhe conferirá maior flexibilidade e a capacidade de personalizar datas para diferentes mercados.
title: Configurar o agendamento preciso do lançamento
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, agenda, data de lançamento, datas, lançamento
ms.localizationpriority: medium
ms.openlocfilehash: b674b2569a40a4f7a504bc6b7bc55ac932b83d01
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234807"
---
# <a name="configure-precise-release-scheduling"></a>Configurar o agendamento preciso do lançamento

A seção **Agendamento** na página [Preço e disponibilidade](set-app-pricing-and-availability.md) permite que você defina a data e a hora exatas em que o aplicativo deve ser disponibilizado na Loja, conferindo a você maior flexibilidade e a capacidade de personalizar datas para diferentes mercados.

> [!NOTE]
> Embora este tópico se refira aos aplicativos, o agendamento de versão para envios de complemento usa o mesmo processo.

Além disso, você pode optar por definir uma data em que o produto não estará mais disponível na Loja. Observe que isso significa que o produto não pode mais ser encontrado na Loja por meio de pesquisa ou navegação, mas qualquer cliente com um link direto pode ver a listagem da Loja do produto. Ele pode baixá-lo somente se ele já tiver o produto ou um [código promocional](generate-promotional-codes.md) e estiver usando um dispositivo Windows 10.

Por padrão (a menos que você tenha selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](choose-visibility-options.md#discoverability)), o aplicativo estará disponível para os clientes assim que ele for aprovado na certificação e concluir o processo de publicação. Para escolher outras datas, selecione **Mostrar opções** para expandir essa seção.

Observe que você não conseguirá configurar as datas na seção **Agendamento** se tiver selecionado uma das opções **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](choose-visibility-options.md#discoverability), porque o aplicativo não será lançado para os clientes; por isso, não há uma data de lançamento a ser configurada.

> [!IMPORTANT]
> As datas especificadas na seção Agendamento só se aplicam aos clientes no Windows 10.
>
>Se o aplicativo publicado anteriormente oferecer suporte a versões anteriores do sistema operacional, qualquer data de **aquisição de parada** que você selecionar não se aplicará a esses clientes; Eles ainda poderão adquirir o aplicativo (a menos que você envie uma atualização com uma nova seleção na seção de [visibilidade](choose-visibility-options.md#discoverability) ou se você selecionar tornar o **aplicativo indisponível** na página **visão geral do aplicativo** ).

## <a name="base-schedule"></a>Agendamento base

As seleções feitas para o agendamento base se aplicarão a todos os mercados em que o aplicativo está disponível, a menos que você adicione posteriormente datas para mercados específicos (ou grupos de mercados), selecionando [Personalizar para mercados específicos](#customize-the-schedule-for-specific-markets).

Você verá duas opções aqui: **Lançamento** e **Parar aquisição**.

## <a name="release"></a>Versão

Na lista suspensa **Lançamento**, você pode definir quando deseja que o aplicativo seja disponibilizado na Loja. Isso significa que o aplicativo pode ser descoberto na Loja por meio de pesquisa ou navegação, e que os clientes podem ver a listagem da Loja e adquirir o aplicativo.

>[!NOTE]
> Depois que o aplicativo for publicado e disponibilizado na Loja, você não conseguirá selecionar uma data de **Lançamento** (a não ser que o aplicativo já tenha sido lançado).

Estas são as opções que você pode configurar para o agendamento do **Lançamento** de um produto:
- **assim que possível**: o produto será lançado assim que ele for certificado e publicado. Essa é a opção padrão.
- **em**: o produto será lançado na data e hora que você selecionar. Você tem duas opções adicionais:
   - **UTC**: a hora que você selecionar será UTC (Horário Coordenado Universal), para que o aplicativo seja lançado ao mesmo tempo em todos os lugares.
   - **Local**: a hora que você selecionar será o valor usado em cada fuso horário associado a um mercado. (Observe que, no caso de mercados que incluem mais de um fuso horário, apenas um fuso horário desse mercado será usado. Para o Estados Unidos, o fuso horário do leste é usado. Uma lista abrangente de fusos horários é mostrada mais abaixo nesta página.
- **não agendado**: o aplicativo não estará disponível na Loja. Se você escolher essa opção, poderá disponibilizar o aplicativo na Loja mais tarde, criando um novo envio e escolhendo uma das outras opções.

## <a name="stop-acquisition"></a>Parar aquisição

Na lista suspensa **Parar aquisição**, você pode definir uma data e hora em que os novos clientes não possam mais adquirir o aplicativo na Loja ou descobri-lo na listagem. Isso pode ser útil se você quiser controlar com precisão quando um aplicativo não será oferecido mais para novos clientes; por exemplo, quando você estiver coordenando a disponibilidade de mais de um aplicativo.

Por padrão, a opção **Parar aquisição** é definida como nunca. Para alterar essa configuração, selecione **em** na lista suspensa e especifique uma data e hora, conforme descrito acima. Na data e hora que você selecionar, os clientes não poderão mais adquirir o aplicativo.

É importante entender que essa opção tem o mesmo impacto que selecionar **tornar este aplicativo detectável, mas não disponível** na seção de [visibilidade](choose-visibility-options.md#discoverability) e escolher **parar a aquisição: qualquer cliente com um link direto pode ver a listagem de armazenamento do produto, mas eles só poderão baixá-lo se possuírem o produto antes ou tiver um código promocional e estiver usando um dispositivo Windows 10.** Para parar completamente de oferecer um aplicativo para novos clientes, clique em **Make app unavailable** na página de visão geral do aplicativo. Para obter mais informações, consulte [Como remover um aplicativo da Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Se você selecionar uma data para **Parar aquisição** e depois decidir disponibilizar o aplicativo novamente, poderá criar um novo envio e alterar **Parar aquisição** para **Nunca**. O aplicativo ficará disponível novamente depois que o envio atualizado for publicado.

## <a name="customize-the-schedule-for-specific-markets"></a>Personalizar o agendamento para mercados específicos

Por padrão, as opções selecionadas acima serão aplicadas a todos os mercados em que o aplicativo for oferecido. Para personalizar o preço para mercados específicos, clique em **Personalizar para mercados específicos**. A janela pop-up **Seleção de mercado** será exibida, listando todos os mercados em que você optou por disponibilizar o app. Se você tiver excluído algum mercado na seção [Mercados](define-pricing-and-market-selection.md), esses mercados não serão mostrados aqui.

Para adicionar a agenda de um mercado, selecione-a e clique em **Salvar**. Depois, você verá as mesmas opções **Lançamento** e **Parar aquisição** descritas acima, mas as seleções feitas serão aplicadas somente a esse mercado.

Para adicionar um agendamento que se aplicará a vários mercados, crie um *grupo de mercados*. Para fazer isso, selecione os mercados que você deseja incluir e insira um nome para o grupo. (Esse nome é somente para referência e não ficará visível para todos os clientes.) Por exemplo, se você quiser criar um grupo de mercado para a América do Norte, selecione **Canadá**, **México** e **Estados Unidos**, e nomeie-o como **América do Norte** ou outro nome de sua preferência. Quando você terminar de criar o grupo de mercados, clique em **Salvar**. Depois, você verá as mesmas opções **Lançamento** e **Parar aquisição** descritas acima, mas as seleções feitas serão aplicadas somente a esse grupo de mercados.

Para adicionar uma agenda personalizada para um mercado adicional ou um grupo de mercados adicional, basta clicar novamente em **Personalizar para mercados específicos** e repetir essas etapas. Para alterar os mercados incluídos em um grupo de mercados, selecione seu nome. Para remover o agendamento personalizado de um grupo de mercados (ou um mercado individual), clique em **Remover**.

> [!NOTE]
> Um mercado não pode pertencer a mais de um dos grupos de mercado usados na seção **Agendamento**.

## <a name="global-time-zones"></a>Fusos horários globais

Abaixo está uma tabela que mostra quais fusos horários específicos são usados em cada mercado, portanto, quando seu envio usa a hora local (por exemplo, lançamento às 9h local), você pode descobrir em que hora ele será lançado em cada mercado, especialmente útil com mercados com mais de um fuso horário, como o Canadá.

| Market | Fuso horário |
|--------|-----------|
| Afeganistão  |  (UTC + 04:30) Cabul |
| Albânia  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Argélia  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Samoa Americana  |  (UTC + 13:00) Samoa |
| Andorra  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Angola  |  (UTC + 01:00) Centro-oeste da África |
| Anguilla  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Antártida  |  (UTC + 12:00) Auckland, Wellington |
| Antígua e Barbuda  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Argentina  |  (UTC-03:00) Cidade de Buenos Aires |
| Armênia  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Aruba  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Austrália  |  (UTC + 10:00) Camberra, Melbourne, Sydney |
| Áustria  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Azerbaijão  |  (UTC + 04:00) Baku |
| Bahamas  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Bahrein  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Bangladesh  |  (UTC + 06:00) Dacca |
| Barbados  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Belarus  |  (UTC + 03:00) Minsk |
| Bélgica  |  (UTC + 01:00) Bruxelas, Copenhague, Madri, Paris |
| Belize  |  (UTC-06:00) Hora central (EUA & Canadá) |
| Benin  |  (UTC + 01:00) Centro-oeste da África |
| Bermudas  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Butão  |  (UTC + 06:00) Dacca |
| República Bolivariana da Venezuela  |  (UTC-04:00) Caracas |
| Bolívia  |  (UTC-04:00) Georgetown, la paz, Manaus, San Juan |
| Bonaire, Santo Eustáquio e Saba  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Bósnia e Herzegovina  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Botsuana  |  (UTC + 01:00) Centro-oeste da África |
| Ilha Bouvet  |  (UTC + 00:00) Monróvia, Reykjavík |
| Brasil  |  (UTC-03:00) Brasília |
| Território Britânico do Oceano Índico  |  (UTC + 06:00) Dacca |
| Ilhas Virgens Britânicas  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Brunei  |  (UTC + 08:00) Irkutsk |
| Bulgária  |  (UTC + 02:00) Chisinau |
| Burquina Faso  |  (UTC + 00:00) Monróvia, Reykjavík |
| Burundi  |  (UTC + 02:00) Harare, Pretória |
| CÃ ́te d' Ivoire  |  (UTC + 00:00) Monróvia, Reykjavík |
| Camboja  |  (UTC + 07:00) Bancoc, Hanói, Jacarta |
| Camarões  |  (UTC + 01:00) Centro-oeste da África |
| Canada  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Cabo Verde  |  (UTC-01:00) Ilha de cabo verde |
| Ilhas Cayman  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| República Centro-Africana  |  (UTC + 01:00) Centro-oeste da África |
| Chade  |  (UTC + 01:00) Centro-oeste da África |
| Chile  |  (UTC-04:00) Santiago |
| China  |  (UTC + 08:00) Pequim, Chonquim, Hong Kong, Urumqi |
| Ilha Christmas  |  (UTC + 07:00) Krasnoyarsk |
| Ilhas Cocos (Keeling)  |  (UTC + 06:30) Yangon (Rangoon) |
| Colômbia  |  (UTC-05:00) Bogotá, Lima, Quito, Rio Branco |
| Ilhas Comores  |  (UTC + 03:00) Nairóbi |
| Congo  |  (UTC + 01:00) Centro-oeste da África |
| Congo (República Democrática)  |  (UTC + 01:00) Centro-oeste da África |
| Ilhas Cook  |  (UTC-10:00) Havaí |
| Costa Rica  |  (UTC-06:00) Hora central (EUA & Canadá) |
| Croácia  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| CuraÃ § ao  |  (UTC-04:00) Cuiabá |
| Chipre  |  (UTC + 02:00) Chisinau |
| Tchéquia  |  (UTC + 01:00) Belgrado, Bratislava, Budapeste, Liubliana, Praga |
| Dinamarca  |  (UTC + 01:00) Bruxelas, Copenhague, Madri, Paris |
| Djibuti  |  (UTC + 03:00) Nairóbi |
| Dominica  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| República Dominicana  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Equador  |  (UTC-05:00) Bogotá, Lima, Quito, Rio Branco |
| Egito  |  (UTC + 02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Hora central (EUA & Canadá) |
| Guiné Equatorial  |  (UTC + 01:00) Centro-oeste da África |
| Eritreia  |  (UTC + 03:00) Nairóbi |
| Estônia  |  (UTC + 02:00) Chisinau |
| Etiópia  |  (UTC + 03:00) Nairóbi |
| Ilhas Malvinas (Falkland)  |  (UTC-04:00) Santiago |
| Ilhas Faroe  |  (UTC + 00:00) Dublin, Edimburgo, Lisboa, Londres |
| Fiji  |  (UTC + 12:00) Ilhas |
| Finlândia  |  (UTC + 02:00) Helsinque, Kiev, Riga, Sófia, Tallinn, Vilnius |
| França  |  (UTC + 01:00) Bruxelas, Copenhague, Madri, Paris |
| Guiana Francesa  |  (UTC-03:00) Caiena, fortaleza |
| Polinésia Francesa  |  (UTC-10:00) Havaí |
| Terras Austrais e Antárticas Francesas  |  (UTC + 05:00) Ashgabat, Tashkent |
| Gabão  |  (UTC + 01:00) Centro-oeste da África |
| Gâmbia, A  |  (UTC + 00:00) Monróvia, Reykjavík |
| Geórgia  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Alemanha  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Gana  |  (UTC + 00:00) Monróvia, Reykjavík |
| Gibraltar  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Grécia  |  (UTC + 02:00) Atenas, Bucareste |
| Groenlândia  |  (UTC + 00:00) Monróvia, Reykjavík |
| Granada  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Guadalupe  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Guam  |  (UTC + 10:00) Guam, porta Moresby |
| Guatemala  |  (UTC-06:00) Hora central (EUA & Canadá) |
| Guernsey  |  (UTC + 00:00) Monróvia, Reykjavík |
| Guiné  |  (UTC + 00:00) Monróvia, Reykjavík |
| Guiné-Bissau  |  (UTC + 00:00) Monróvia, Reykjavík |
| Guiana  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Haiti  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Ilhas Heard e McDonald  |  (UTC-05:00) Bogotá, Lima, Quito, Rio Branco |
| Santa Sé (Cidade do Vaticano)  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Honduras  |  (UTC-06:00) Hora central (EUA & Canadá) |
| RAE de Hong Kong  |  (UTC + 08:00) Pequim, Chonquim, Hong Kong, Urumqi |
| Hungria  |  (UTC + 01:00) Belgrado, Bratislava, Budapeste, Liubliana, Praga |
| Islândia  |  (UTC + 00:00) Monróvia, Reykjavík |
| Índia  |  (UTC + 05:30) Chennai, Kolkata, Mumbai, Nova Délhi |
| Indonésia  |  (UTC + 07:00) Bancoc, Hanói, Jacarta |
| Iraque  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Irlanda  |  (UTC + 00:00) Dublin, Edimburgo, Lisboa, Londres |
| Israel  |  (UTC + 02:00) Jerusalém |
| Itália  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Jamaica  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Japão  |  (UTC + 09:00) Osaka, Sapporo, Tóquio |
| Jersey  |  (UTC + 00:00) Monróvia, Reykjavík |
| Jordânia  |  (UTC + 02:00) Chisinau |
| Cazaquistão  |  (UTC + 05:00) Ashgabat, Tashkent |
| Quênia  |  (UTC + 03:00) Nairóbi |
| Kiribati  |  (UTC + 14:00) Ilha Kiritimati |
| Coreia do Sul  |  (UTC + 09:00) Seul |
| Kuwait  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Quirguistão  |  (UTC + 06:00) Astana |
| Laos  |  (UTC + 07:00) Bancoc, Hanói, Jacarta |
| Letônia  |  (UTC + 02:00) Chisinau |
| Líbano  |  (UTC + 02:00) Chisinau |
| Lesoto  |  (UTC + 02:00) Harare, Pretória |
| Libéria  |  (UTC + 00:00) Monróvia, Reykjavík |
| Líbia  |  (UTC + 02:00) Chisinau |
| Liechtenstein  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Lituânia  |  (UTC + 02:00) Chisinau |
| Luxemburgo  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| RAE de Macau  |  (UTC + 08:00) Pequim, Chonquim, Hong Kong, Urumqi |
| Madagascar  |  (UTC + 03:00) Nairóbi |
| Malaui  |  (UTC + 02:00) Harare, Pretória |
| Malásia  |  (UTC + 08:00) Kuala Lumpur, Cingapura |
| Maldivas  |  (UTC + 05:00) Ashgabat, Tashkent |
| Máli  |  (UTC + 00:00) Monróvia, Reykjavík |
| Malta  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Homem, ilha de  |  (UTC + 00:00) Dublin, Edimburgo, Lisboa, Londres |
| Ilhas Marshall  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-antigo |
| Martinica  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Mauritânia  |  (UTC + 00:00) Monróvia, Reykjavík |
| Maurício  |  (UTC + 04:00) Porta Louis |
| Mayotte  |  (UTC + 03:00) Nairóbi |
| México  |  (UTC-06:00) Guadalajara, cidade do México, Monterrey |
| Micronésia  |  (UTC + 10:00) Guam, porta Moresby |
| Moldova  |  (UTC + 02:00) Chisinau |
| Mônaco  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Mongólia  |  (UTC + 07:00) Krasnoyarsk |
| Montenegro  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Montserrat  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Marrocos  |  (UTC + 01:00) Casablanca |
| Moçambique  |  (UTC + 02:00) Harare, Pretória |
| Myanmar  |  (UTC + 06:30) Yangon (Rangoon) |
| Namíbia  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Nauru  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-antigo |
| Nepal  |  (UTC + 05:45) Katmandu |
| Países Baixos  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Nova Caledônia  |  (UTC + 11:00) Ilhas Salomão, Nova Caledônia |
| Nova Zelândia  |  (UTC + 12:00) Auckland, Wellington |
| Nicarágua  |  (UTC-06:00) Hora central (EUA & Canadá) |
| Níger  |  (UTC + 01:00) Centro-oeste da África |
| Nigéria  |  (UTC + 01:00) Centro-oeste da África |
| Niue  |  (UTC + 13:00) Samoa |
| Ilha Norfolk  |  (UTC + 11:00) Ilhas Salomão, Nova Caledônia |
| Macedônia do Norte  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Ilhas Marianas do Norte  |  (UTC + 10:00) Guam, porta Moresby |
| Noruega  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Omã  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Paquistão  |  (UTC + 05:00) Islamabad, Carachi |
| Palau  |  (UTC + 09:00) Osaka, Sapporo, Tóquio |
| Autoridade Palestina  |  (UTC + 02:00) Chisinau |
| Panamá  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Papua Nova-Guiné  |  (UTC + 10:00) Oficial Vladivostok |
| Paraguai  |  (UTC-04:00) Assunção |
| Peru  |  (UTC-05:00) Bogotá, Lima, Quito, Rio Branco |
| Filipinas  |  (UTC + 08:00) Kuala Lumpur, Cingapura |
| Ilhas Pitcairn  |  (UTC-08:00) Hora do Pacífico (EUA & Canadá) |
| Polônia  |  (UTC + 01:00) Belgrado, Bratislava, Budapeste, Liubliana, Praga |
| Portugal  |  (UTC + 00:00) Dublin, Edimburgo, Lisboa, Londres |
| Catar  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Reunião  |  (UTC + 04:00) Porta Louis |
| Romênia  |  (UTC + 02:00) Chisinau |
| ROW  |  (UTC-07:00) Hora das montanhas (EUA & Canadá) |
| Rússia  |  (UTC + 03:00) Moscou, São Petersburgo |
| Ruanda  |  (UTC + 02:00) Harare, Pretória |
| SÃ £ o TomÃ © e PrÃncipe  |  (UTC + 00:00) Monróvia, Reykjavík |
| São BarthÃ © Lemy  |  (UTC + 04:00) Ierevan |
| Santa Helena, Ascensão e Tristão da Cunha  |  (UTC + 00:00) Dublin, Edimburgo, Lisboa, Londres |
| São Cristóvão e Névis  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Santa Lúcia  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Saint Martin (parte francesa)  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| São Pedro e Miquelon  |  (UTC-02:00) Meados do Atlântico-antigo |
| São Vicente e Granadinas  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Samoa  |  (UTC + 13:00) Samoa |
| San Marino  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Arábia Saudita  |  (UTC + 03:00) Kuwait, Riad |
| Senegal  |  (UTC + 00:00) Monróvia, Reykjavík |
| Sérvia  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Seicheles  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Serra Leoa  |  (UTC + 00:00) Monróvia, Reykjavík |
| Singapura  |  (UTC + 08:00) Kuala Lumpur, Cingapura |
| Sint Maarten (parte holandesa)  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Eslováquia  |  (UTC + 01:00) Belgrado, Bratislava, Budapeste, Liubliana, Praga |
| Eslovênia  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Ilhas Salomão  |  (UTC + 11:00) Ilhas Salomão, Nova Caledônia |
| Somália  |  (UTC + 03:00) Nairóbi |
| África do Sul  |  (UTC + 02:00) Harare, Pretória |
| Geórgia do Sul e Ilhas Sandwich do Sul  |  (UTC-02:00) Meados do Atlântico-antigo |
| Espanha  |  (UTC + 01:00) Bruxelas, Copenhague, Madri, Paris |
| Sri Lanka  |  (UTC + 05:30) Chennai, Kolkata, Mumbai, Nova Délhi |
| Suriname  |  (UTC-03:00) Caiena, fortaleza |
| Svalbard e Jan Mayen  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Suazilândia  |  (UTC + 02:00) Harare, Pretória |
| Suécia  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Suíça  |  (UTC + 01:00) Amsterdã, Berlim, Berna, Roma, Estocolmo, Viena |
| Taiwan  |  (UTC + 08:00) Horário |
| Tadjiquistão  |  (UTC + 05:00) Ashgabat, Tashkent |
| Tanzânia  |  (UTC + 03:00) Nairóbi |
| Tailândia  |  (UTC + 07:00) Bancoc, Hanói, Jacarta |
| Timor-Leste  |  (UTC + 09:00) Seul |
| Togo  |  (UTC + 00:00) Monróvia, Reykjavík |
| Tokelau  |  (UTC + 13:00) Nuku ' alofa |
| Tonga  |  (UTC + 13:00) Nuku ' alofa |
| Trinidad e Tobago  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Tunísia  |  (UTC + 01:00) Sarajevo, Skopje, Varsóvia, Zagreb |
| Turquia  |  (UTC + 03:00) Istambul |
| Turcomenistão  |  (UTC + 05:00) Ashgabat, Tashkent |
| Ilhas Turcas e Caicos  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Tuvalu  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-antigo |
| Territórios Insulares dos EUA  |  (UTC + 13:00) Samoa |
| Ilhas Virgens dos Estados Unidos  |  (UTC-04:00) Hora do Atlântico (Canadá) |
| Uganda  |  (UTC + 03:00) Nairóbi |
| Ucrânia  |  (UTC + 02:00) Chisinau |
| Emirados Árabes Unidos  |  (UTC + 04:00) Abu Dhabi, Mascate |
| United Kingdom  |  (UTC + 00:00) Dublin, Edimburgo, Lisboa, Londres |
| Estados Unidos  |  (UTC-05:00) Hora do leste (EUA & Canadá) |
| Uruguai  |  (UTC-03:00) Brasília |
| Uzbequistão  |  (UTC + 05:00) Ashgabat, Tashkent |
| Vanuatu  |  (UTC + 11:00) Ilhas Salomão, Nova Caledônia |
| Vietnã  |  (UTC + 07:00) Bancoc, Hanói, Jacarta |
| Wallis e Futuna  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-antigo |
| Iêmen  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Zâmbia  |  (UTC + 02:00) Harare, Pretória |
| Zimbábue  |  (UTC + 02:00) Harare, Pretória |
