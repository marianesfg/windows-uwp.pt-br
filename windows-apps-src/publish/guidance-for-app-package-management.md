---
author: jnHs
Description: Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.
title: Orientação para gerenciamento de pacote de aplicativo
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
---

# Orientação para gerenciamento de pacote de aplicativo


Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.

-   [Versões do sistema operacional e distribuição de pacote](#os-versions-and-package-distribution)
-   [Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Mantendo a compatibilidade de pacote para o Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [Removendo um aplicativo da Loja](#removing-an-app-from-the-store)
-   [Removendo pacotes para uma família de dispositivos com suporte anterior](#removing-packages-for-a-previously-supported-device-family)

## Versões do sistema operacional e distribuição de pacote


Diferentes sistemas operacionais podem executar diferentes tipos de pacotes. Se mais de um dos seus pacotes puder ser executado no dispositivo do cliente, a Windows Store fornecerá a melhor correspondência disponível.

De modo geral, as versões posteriores do sistema operacional podem executar pacotes para versões anteriores do sistema operacional para a mesma família de dispositivos. No entanto, os clientes só obterão esses pacotes se o aplicativo não incluir um pacote para a versão posterior do sistema operacional atual.

Por exemplo, os dispositivos Windows 10 podem executar todas as versões anteriores suportadas do sistema operacional (por família de dispositivos). Os dispositivos de desktop Windows 10 podem executar aplicativos criados para o Windows 8.1 ou Windows 8; os dispositivos móveis Windows 10 podem executar aplicativos criados para Windows Phone 8.1, Windows Phone 8 e até mesmo Windows Phone 7.x.

Os exemplos a seguir ilustram diversos cenários para um aplicativo que inclui pacotes para diferentes versões do sistema operacional. Em alguns casos, restrições específicas dos seus pacotes podem não permitir que eles sejam executados em todas as versões do sistema operacional e nos tipos de dispositivo listados aqui (por exemplo, a arquitetura deve ser apropriada), mas esses exemplos devem ajudar você a compreender quais versões do sistema operacional podem executar seus pacotes específicos.

### Aplicativo de exemplo 1

| Sistema operacional específico do pacote | Sistemas operacionais que receberão esse pacote |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Dispositivos de desktop Windows 10, Windows 8.1      |
| Windows Phone 8.1                   | Dispositivos móveis Windows 10, Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

No exemplo de aplicativo 1, o aplicativo ainda não possui pacotes da UWP (Plataforma Universal do Windows) criados especificamente para dispositivos Windows 10, mas os clientes do Windows 10 ainda podem obter o aplicativo. Esses clientes obterão os melhores pacotes disponíveis, dependendo do tipo de dispositivo.

### Aplicativo de exemplo 2

| Sistema operacional específico do pacote  | Sistemas operacionais que receberão esse pacote |
|--------------------------------------|----------------------------------------------|
| Windows 10 (família de dispositivos universal) | Windows 10 (todas as famílias de dispositivos)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

No exemplo de aplicativo 2, não há nenhum pacote que possa ser executado no Windows 8. Os clientes que estejam executando todas as outras versões do sistema operacional podem obter o aplicativo.

### Aplicativo de exemplo 3

| Sistema operacional específico do pacote | Sistemas operacionais que receberão esse pacote                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (família de dispositivos de desktop)  | Dispositivos de desktop Windows 10                                    |
| Windows Phone 8                     | Dispositivos móveis Windows 10, Windows Phone 8, Windows Phone 8.1 |

No exemplo de aplicativo 3, como não há nenhum pacote UWP destinado à família de dispositivos móveis, os clientes de dispositivos móveis Windows 10 obterão o pacote do Windows Phone 8. Se, mais tarde, esse aplicativo adicionar um pacote direcionado à família de dispositivos móveis (ou à família de dispositivos universal), esses pacotes estarão disponíveis para os clientes em dispositivos móveis Windows 10 em vez do pacote do Windows Phone 8.

Observe também que este exemplo de aplicativo não inclui nenhum pacote que pode ser executado no Windows 7.x.

### Aplicativo de exemplo 4

| Sistema operacional específico do pacote  | Sistemas operacionais que receberão esse pacote |
|--------------------------------------|----------------------------------------------|
| Windows 10 (família de dispositivos universal) | Windows 10 (todas as famílias de dispositivos)             |

No exemplo de aplicativo 4, qualquer dispositivo que execute o Windows 10 pode obter o aplicativo, mas não estará disponível para clientes com uma versão anterior do sistema operacional. Como o pacote UWP é direcionado à família de dispositivos universal, ele estará disponível para dispositivos móveis e de desktop Windows 10.

## Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente


Se você tiver um aplicativo na Loja e quiser atualizar seu aplicativo para Windows 10, crie um novo envio e adicione seus pacotes .appxupload UWP durante a etapa [Pacotes](upload-app-packages.md). Depois que seu aplicativo passar pelo processo de certificação, os clientes que já tinham seu aplicativo antes de atualizar para o Windows 10 poderão baixar o pacote UWP como uma atualização na Loja. O pacote UWP também estará disponível para novas aquisições pelos clientes no Windows 10.

> **Importante**  Depois que um cliente no Windows 10 obtiver seu pacote UWP, você não poderá reverter esse cliente para usar um pacote para qualquer versão anterior do sistema operacional. Não se esqueça de testar exaustivamente seus pacotes UWP no Windows 10 antes de adicioná-los ao seu envio.

Você pode atualizar qualquer um dos seus outros pacotes ao mesmo tempo ou fazer outras alterações no envio (por exemplo, você pode [criar descrições específicas da plataforma](create-platform-specific-descriptions.md) para serem exibidas para clientes em versões anteriores do sistema operacional). Você também pode deixar tudo exatamente igual, se preferir.

> **Observação**  O número de versão de seus pacotes do Windows 10 deve ser maior que os dos pacotes do Windows 8, Windows 8.1 e/ou Windows Phone 8.1 que você estiver publicando (ou pacotes para as versões do sistema operacional que você publicou anteriormente) para o mesmo aplicativo. Para obter mais informações sobre numeração de versão no Windows 10, consulte [Numeração de versão do pacote](package-version-numbering.md).

Depois que o novo envio concluir o processo de certificação, os pacotes UWP estarão disponíveis, juntamente com todos os outros pacotes que você disponibilizou para os clientes que ainda não estão no Windows 10.

Para obter mais informações sobre o empacotamento de aplicativos UWP para a Loja, consulte [Empacotando aplicativos universais do Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

> **Importante**  Tenha em mente que, se você fornecer pacotes destinados à família de dispositivos universal, todo cliente que já tiver seu aplicativo em algum sistema operacional anterior (Windows Phone 8, Windows 8.1 etc.) e atualizar para o Windows 10 será atualizado para o pacote universal do Windows 10.
> 
> Isso acontece mesmo se você excluiu uma família de dispositivos específica na etapa [Preço e disponibilidade](set-app-pricing-and-availability.md#windows-10-device-families) de seu envio, pois a seleção **Famílias de dispositivos** só se aplica a novas aquisições. Se não quiser que cada cliente anterior obtenha o novo pacote do Windows 10, atualize o elemento [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) em seu manifesto appx para incluir somente a família de dispositivos específica à qual você deseja dar suporte.
> 
> Por exemplo, digamos que você deseje que apenas os clientes do Windows 8 e do Windows 8.1 que atualizaram para o Windows 10 obtenham seu aplicativo UWP e queira que os clientes no Windows Phone 8.1 e em versões anteriores mantenham os pacotes disponibilizados anteriormente (para Windows Phone 8 ou Windows Phone 8.1). Para fazer isso, você precisará atualizar o [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) em seu manifesto appx para incluir somente **Windows.Desktop** (para a família de dispositivos de desktop), em vez de deixá-lo como o valor **Windows.Universal** (para a família de dispositivos universal) que o Microsoft Visual Studio inclui no manifesto appx por padrão. Não envie nenhum pacote UWP destinado a famílias de dispositivos universais ou móveis (**Windows.Universal** ou **Windows.Universal**). Dessa forma, seus clientes móveis do Windows 10 não obterão nenhum dos seus pacotes UWP.
> 
> Para saber mais sobre famílias de dispositivos, veja [Guia para aplicativos UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/windows/apps/dn894631).

## Mantendo a compatibilidade de pacote para o Windows Phone 8.1


Determinados requisitos de tipos de pacotes se aplicam durante a atualização de aplicativos que já foram publicados para o Windows Phone 8.1:

-   Depois que um aplicativo tiver um pacote publicado do Windows Phone 8.1, todas as atualizações posteriores também deverão conter um pacote do Windows Phone 8.1.
-   Depois que um aplicativo tiver um XAP publicado do Windows Phone 8.1, as atualizações posteriores deverão ter um XAP do Windows Phone 8.1, appx do Windows Phone 8.1 ou appxbundle do Windows Phone 8.1.
-   Quando um aplicativo tiver um .appx do Windows Phone 8.1 publicado, as atualizações posteriores deverão ter um .appx do Windows Phone 8.1. ou .appxbundle do Windows Phone 8.1. Em outras palavras, um XAP do Windows Phone 8.1 não é permitido. Isso se aplica a um .appxupload que contenha um .appx do Windows Phone 8.1 também.
-   Depois que um aplicativo tiver um .appxbundle do Windows Phone 8.1 publicado, as atualizações posteriores deverão ter um .appxbundle do Windows Phone 8.1. Em outras palavras, um XAP do Windows Phone 8.1 ou .appx do Windows Phone 8.1 não é permitido. Isso se aplica a um .appxupload que contenha um .appxbundle do Windows Phone 8.1 também.

Se essas regras não forem seguidas, haverá erros de carregamento de pacote que impedirão a conclusão do envio.

## Removendo um aplicativo da Loja


Às vezes, você pode desejar cancelar completamente a oferta de um aplicativo aos clientes efetivamente "cancelando sua publicação". Para fazer isso, clique em **Tornar aplicativo indisponível** na página de visão geral do aplicativo. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Loja, e os novos clientes não poderão obtê-lo por meio de qualquer método, incluindo códigos promocionais.

> **Importante**  Isso substituirá todas as configurações de [distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) que você selecionou em seus envios.

Observe que todos os clientes que já têm o aplicativo ainda poderão usá-lo (e ainda poderão receber atualizações se você enviar novos pacotes mais tarde).

Depois de tornar o aplicativo indisponível, você continuará a vê-lo em seu painel. Se optar por oferecer o aplicativo aos clientes novamente, você poderá clicar em **Tornar aplicativo disponível** na página Visão geral do aplicativo. Depois que você confirmar, o aplicativo estará disponível a novos clientes (a menos que esteja restrito pelas configurações no seu último envio) dentro de algumas horas.

> **Observação**  Se você desejar manter seu aplicativo disponível, mas não desejar continuar a oferecê-lo aos clientes em uma determinada versão do sistema operacional, poderá criar um novo envio e remover todos os pacotes para a versão do sistema operacional na qual você deseja impedir novas aquisições. Por exemplo, se antes você tinha pacotes para Windows Phone 8, Windows Phone 8.1 e Windows 10, e agora não deseja continuar oferecendo o aplicativo para novos clientes no Windows Phone 8, remova os pacotes do Windows Phone 8 do envio. Depois que a atualização for publicada, os novos clientes do Windows Phone 8 não poderão adquirir o aplicativo (embora os clientes que já o tenham possam continuar a usá-lo). O aplicativo ainda estará disponível para novos clientes no Windows Phone 8.1 e no Windows 10.

## Removendo pacotes para uma família de dispositivos com suporte anterior


Se você remover todos os pacotes para uma determinada família de dispositivos que contava com o suporte de seu aplicativo anteriormente, deverá confirmar que é essa a sua intenção para poder salvar suas alterações na página **Pacotes**.

Quando você publicar um envio que remove pacotes de uma família de dispositivos que contava anteriormente com o suporte de seu aplicativo, os novos clientes não poderão adquirir o aplicativo nessa família de dispositivos. Você sempre pode publicar outra atualização posteriormente para fornecer pacotes para essa família de dispositivos novamente.

Lembre-se de que mesmo se você remover todos os pacotes que dão suporte a uma determinada família de dispositivos, quaisquer clientes existentes que já tiverem instalado o aplicativo nesse tipo de dispositivo ainda poderão usá-lo e receberão as atualizações que você fornecer mais tarde.

 

 






<!--HONumber=May16_HO2-->


