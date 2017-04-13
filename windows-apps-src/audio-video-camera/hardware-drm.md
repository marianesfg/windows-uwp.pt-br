---
author: eliotcowley
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: "Este tópico fornece uma visão geral de como adicionar DRM (gerenciamento de direitos digitais) baseado em hardware PlayReady ao seu aplicativo UWP (Plataforma Universal do Windows)."
title: DRM de Hardware
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ba561b340796a175815a37f5c3d8d4c125ef35c4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="hardware-drm"></a>DRM de Hardware

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico fornece uma visão geral de como adicionar DRM (gerenciamento de direitos digitais) baseado em hardware PlayReady ao seu aplicativo UWP (Plataforma Universal do Windows).

> [!NOTE] 
> O DRM do PlayReady baseado em hardware é compatível com uma grande quantidade de dispositivos, incluindo dispositivos com Windows e sem Windows, como televisores, telefones e tablets. Para um dispositivo Windows dar suporte ao DRM do PlayReady de Hardware, ele deve estar executando o Windows 10 e ter uma configuração de hardware com suporte.

Cada vez mais, os provedores de conteúdo estão migrando para proteções baseadas em hardware para conceder permissão para reproduzir conteúdo completo de alto valor em aplicativos. O suporte robusto a uma implementação de hardware do núcleo criptográfico foi adicionado ao PlayReady para atender a essa necessidade. Esse suporte permite a reprodução segura de conteúdo de alta definição (1080p) e ultra-alta definição (UHD) em várias plataformas de dispositivos. O material de chave (incluindo chaves privadas, chaves de conteúdo e qualquer outro material de chave usado para derivar ou desbloquear essas chaves) e amostras de vídeo compactadas e não compactadas descriptografadas são protegidos ao aproveitar a segurança do hardware.

## <a name="windows-tee-implementation"></a>Implementação do ambiente de execução confiável do Windows

Este tópico fornece uma breve visão geral de como o Windows 10 implementa o ambiente de execução confiável (TEE).

Os detalhes da implementação do ambiente de execução confiável do Windows está fora do escopo deste documento. No entanto, uma breve discussão sobre a diferença entre o kit de porta padrão do ambiente de execução confiável e da porta do Windows será benéfica. O Windows implementa a camada de proxy do OEM e transfere as chamadas de funções PRITEE serializadas para um driver de modo de usuário no subsistema do Windows Media Foundation. Isso eventualmente é roteado para o driver do ambiente de execução confiável (Trusted Execution Environment) do Windows ou para o driver de gráficos do OEM. Os detalhes de qualquer uma dessas abordagens está fora do escopo deste documento. O diagrama a seguir mostra a interação de componentes geral para a porta do Windows. Se você quiser desenvolver uma implementação de TEE do PlayReady do Windows, contate <WMLA@Microsoft.com>.

![diagrama de componente de tee do Windows](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>Considerações sobre como usar o DRM de hardware

Este tópico fornece uma breve lista de itens que devem ser considerados ao desenvolver aplicativos projetados para usar DRM de hardware. Conforme explicado em [DRM do PlayReady](playready-client-sdk.md#output-protection), com o HWDRM do PlayReady para Windows 10, todas as proteções de saída são impostas dentro da implementação do ambiente de execução confiável do Windows, que tem algumas consequências sobre os comportamentos de proteção de saída:

-   **Suporte para o nível de proteção de saída (OPL) para vídeo digital descompactado 270:** o HWDRM do PlayReady para Windows 10 não dá suporte para baixa resolução e irá impor que esse HDCP seja ativado. Recomendamos que o conteúdo de alta definição para o HWDRM tenha um OPL superior a 270, embora não seja necessário. Além disso, recomendamos que você defina restrição de tipo HDCP na licença (HDCP versão 2.2 no Windows 10).
-   **Ao contrário do DRM de software (SWDRM), proteções de saída são impostas em todos os monitores com base no monitor de menor capacidade.** Por exemplo, se o usuário tiver dois monitores conectados, em que um dos monitores oferece suporte à uma HDCP e o outro não, haverá falha na reprodução se a licença exigir uma HDCP, mesmo se o conteúdo só estiver sendo renderizado no monitor que oferece suporte à HDCP. No DRM de software, o conteúdo é reproduzido contanto que esteja sendo renderizado somente no monitor que oferece suporte à HDCP.
-   **Não há garantia de que o HWDRM seja usado pelo cliente e que seja seguro, a menos que as seguintes condições sejam atendidas** pelas licenças e chaves de conteúdo:
    -   A licença usada para a chave de conteúdo de vídeo deve ter uma propriedade de nível mínimo de segurança de 3000.
    -   O áudio deve ser criptografado em uma chave de conteúdo diferente daquela do vídeo, e a licença usada para áudio deve ter uma propriedade de nível mínimo de segurança de 2000. Como alternativa, o áudio pode não ser criptografado.
    
Além disso, você deve levar os seguintes itens em consideração ao usar o HWDRM:

-   Não há suporte para PMP (Processo de Mídia Protegida)
-   Não há suporte para o Windows Media Video (também conhecido como VC-1) (consulte [Substituir DRM de hardware](#override-hardware-drm)).
-   Não há suporte para várias GPUs (unidades de processamento gráfico) para licenças persistentes.

Para lidar com licenças persistentes em computadores com várias GPUs, considere o seguinte cenário:

1.  Um cliente compra um novo computador com uma placa gráfica integrada.
2.  O cliente usa um aplicativo que adquire licenças persistentes enquanto usa o DRM de hardware.
3.  A licença persistente agora está associada às chaves de hardware da placa gráfica.
4.  O cliente instala uma nova placa gráfica.
5.  Todas as licenças no HDS (armazenamento de dados hash) são associadas à placa de vídeo integrada, mas o cliente agora deseja reproduzir conteúdo protegido usando a placa gráfica instalada recentemente.

Para impedir falha na reprodução porque as licenças não podem ser descriptografadas pelo hardware, o PlayReady usa um HDS separado para cada placa gráfica que ele encontrar. Isso fará com que o PlayReady tente a aquisição de licença para uma parte do conteúdo do qual o PlayReady normalmente teria uma licença (ou seja, no caso de DRM de software ou em qualquer caso sem uma alteração de hardware, o PlayReady não precisa readquirir uma licença). Portanto, se o aplicativo adquirir uma licença persistente enquanto usa o DRM de hardware, é necessário que seu aplicativo consiga lidar com o caso em que a licença é efetivamente "perdida" se o usuário final instalar (ou desinstalar) uma placa gráfica. Como isso não é um cenário comum, você pode optar por manipular as chamadas de suporte quando o conteúdo não for mais reproduzido após uma alteração de hardware em vez de descobrir como lidar com uma alteração de hardware no código de servidor/cliente.

## <a name="override-hardware-drm"></a>Substituir DRM de hardware

Esta seção descreve como substituir DRM de hardware (HWDRM) se o conteúdo a ser reproduzido não oferecer suporte a ele.

Por padrão, o DRM de hardware é usado se o sistema oferecer suporte a ele. No entanto, não há suporte para parte do conteúdo no DRM de hardware. Um exemplo disso é o conteúdo Cocktail. Outro exemplo é qualquer conteúdo que usa um codec de vídeo que não seja H.264 e HEVC. Mais um exemplo é o conteúdo HEVC, pois alguns DRMs de hardware oferecerão suporte a HEVC, enquanto outros não. Portanto, se você deseja reproduzir uma parte do conteúdo e o DRM de hardware não oferecer suporte a ele no sistema em questão, você poderá recusar o DRM de hardware.

O exemplo a seguir mostra como recusar o DRM de hardware. Você só precisa fazer isso para alternar. Além disso, verifique se não há nenhum objeto PlayReady na memória; caso contrário, o comportamento será indefinido.

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

Para voltar para o DRM de hardware, defina o valor de **SoftwareOverride** como **0**.

Para cada reprodução de mídia, você precisa definir o **MediaProtectionManager** como:

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

A melhor maneira de saber se você está em DRM de hardware ou software é verificar em C:\\Users\\&lt;nome do usuário&gt;\\AppData\\Local\\Packages\\&lt;nome do aplicativo&gt;\\LocalState\\PlayReady\\\*

-   Se houver um arquivo mspr.hds, você estará em DRM de software.
-   Se houver outro arquivo \*.hds, você estará em DRM de hardware.
-   Você também pode excluir a pasta PlayReady inteira e repetir o teste.

## <a name="detect-the-type-of-hardware-drm"></a>Detectar o tipo de DRM de hardware

Esta seção descreve como detectar qual tipo de DRM de hardware tem suporte no sistema.

Você pode usar o método [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) para determinar se o sistema oferece suporte a um recurso DRM de hardware específico. Por exemplo:

```cpp
boolean PlayReadyStatics->CheckSupportedHardware(PlayReadyHardwareDRMFeatures enum);
```

A enumeração [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) contém a lista válida de valores de recursos de DRM de hardware que pode ser consultada. Para determinar se há suporte para DRM de hardware, use o membro **HardwareDRM** na consulta. Para determinar se o hardware oferece suporte ao codec HEVC (Codificação de Vídeo de Alta Eficiência)/H.265, use o membro **HEVC** na consulta.

Você também pode usar a propriedade [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) para obter o nível de segurança do certificado do cliente e determinar se há suporte para o DRM de hardware. A menos que o nível de segurança do certificado retornado seja maior ou igual a 3000, o cliente não é individualizado ou provisionado (nesse caso, a propriedade retorna 0) ou o DRM de hardware não está em uso (nesse caso, essa propriedade retorna um valor menor que 3000).

## <a name="see-also"></a>Consulte também
- [DRM do PlayReady](playready-client-sdk.md)
