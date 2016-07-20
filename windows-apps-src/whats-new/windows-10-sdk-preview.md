---
author: QuinnRadich
title: Novidades no Windows 10
description: "A Compilação do Windows 10 Anniversary SDK Preview e as novas ferramentas de desenvolvedor fornecem as ferramentas, os recursos e as experiências da nova Plataforma Universal do Windows."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 9623e10b15dbc5f9480d1cd9bd740fa8e914b3c6

---

# Novidades no Windows

A compilação 14295 do Windows 10 Anniversary SDK Preview e as atualizações das ferramentas de desenvolvedor do Windows continuarão a fornecer as ferramentas, os recursos e as experiências da Plataforma Universal do Windows. 
              [Instale as ferramentas e o SDK](https://developer.microsoft.com/windows/downloads#_blank) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) ou descobrir como pode usar seu [código de aplicativo existente no Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

## Compilação 12295 do Windows 10 Anniversary SDK Preview

Recurso | Descrição
 :---- | :----
Rede | Agora você pode fornecer sua própria validação personalizada de certificados SSL/TLS de servidor assinando o evento [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank). Você também pode desativar completamente a leitura de respostas HTTP do cache, especificando o valor de enumeração [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) em uma solicitação HTTP. Agora é possível limpar as credenciais de autenticação para habilitar um cenário de "logoff" chamando o método [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank).
Extensões | Uma novidade no Microsoft Edge é a capacidade de usar extensões. Com as extensões, os usuários são capazes de ampliar os recursos do Microsoft Edge, fornecendo funcionalidades de nicho que são importante para determinados públicos-alvo. Confira a [documentação de extensões](https://developer.microsoft.com/microsoft-edge/platform/documentation/extensions/#_blank) para obter mais informações.
APIs Bluetooth | Aplicativos agora são capazes de acessar os serviços RFCOMM em periféricos Bluetooth remotos via [Windows.Devices.Bluetooth e Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank) sem a necessidade de primeiramente emparelhar-se com o periférico. Novos métodos permitem que aplicativos pesquisem e acessem serviços RFCOMM em dispositivos não emparelhados.
APIs de chat | Com a nova classe [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank), você pode sincronizar as mensagens de texto de e para a nuvem.
[Mapeamento do conceito de aplicativos do Windows para desenvolvedores do Android e iOS](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | Se você for um desenvolvedor com habilidades e/ou código para Android ou iOS e quiser mudar para o Windows 10 e para a UWP (Plataforma Universal do Windows), este método tem tudo o que você precisa para mapear recursos de plataforma, e seu conhecimento, entre as três plataformas.
[EDP (proteção de dados empresariais)](https://msdn.microsoft.com/windows/uwp/enterprise/edp-hub?branch=build2016#_blank) | EDP é um conjunto de recursos em desktops, notebooks, tablets e telefones para o Gerenciamento de Dispositivos Móveis (MDM). O EDP dá às empresas maior controle sobre como seus dados (arquivos empresariais e blobs de dados) são tratados nos dispositivos que a empresa gerencia.
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | O novo namespace AppExtensions permite que seu aplicativo da Windows Store hospede o conteúdo fornecido por outros aplicativos da Windows Store. Você pode descobrir, enumerar e acessar conteúdo somente leitura desses aplicativos.
Windows IoT | O Windows 10 IoT Core permite que você crie aplicativos IoT na familiaridade do Windows e agora está disponível na Raspberry Pi 3, a mais nova placa Raspberry Pi.
APIs de mídia | As novas APIs MediaBreak no namespace Windows.Media.Playback permitem que você agende e gerencie pausas de mídia com facilidade durante a reprodução de mídia usando MediaSource e MediaPlaybackItem. As novas APIs AudioGraph no namespace Windows.Media.Audio adicionam processamento de áudio espacial que permite atribuir os emissores e ouvintes posicionados em 3D a nós do gráfico de áudio.
APIs de mapas | O [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank) foi aprimorado para permitir que os desenvolvedores obtenham uma região visível que esteja próxima da câmera, excluindo regiões que estão distantes e perto do horizonte em uma exibição profundamente densa. A classe [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank) foi estendida, permitindo que os desenvolvedores otimizem o tráfego de rede na geocodificação reversa por meio de especificação de uma precisão desejada. Agora os desenvolvedores podem aproveitar o download de mapas offline usando o método [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) e especificando a latitude e longitude. Para saber mais, consulte [Iniciar o aplicativo Mapas do Windows](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank).



<!--HONumber=Jul16_HO2-->


