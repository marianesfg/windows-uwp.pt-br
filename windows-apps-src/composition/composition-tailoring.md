---
title: Personalização de composição
description: Use as APIs de composição para personalizar sua interface do usuário, otimizar para desempenho e acomodar as configurações de usuários e as características do dispositivo.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 95a7355241f9ba4cc7b4bb743b78ac09169d65d9
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213673"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Personalizando efeitos & experiências usando a interface do usuário do Windows

A interface do usuário do Windows fornece muitos efeitos bonitos, animações e meios de diferenciação. No entanto, atender às expectativas do usuário quanto ao desempenho e à personalização ainda é uma parte necessária da criação de aplicativos bem-sucedidos. O Plataforma Universal do Windows dá suporte a uma família grande e diversificada de dispositivos, que têm recursos e funcionalidades diferentes. Para fornecer uma experiência inclusiva para todos os seus usuários, você precisa garantir que seus aplicativos sejam dimensionados entre dispositivos e respeitar as preferências do usuário. A personalização da interface do usuário pode fornecer uma maneira eficiente de aproveitar os recursos de um dispositivo e garantir uma experiência de usuário agradável e inclusiva.

A personalização da interface do usuário é uma categoria abrangente que abrange o trabalho para o desempenho, a bela interface do usuário, com relação às seguintes áreas:

- Respeitar e adaptar-se às configurações do usuário para efeitos
- Acomodar configurações de usuário para animações
- Otimizando a interface do usuário para os recursos de hardware fornecidos

Aqui, abordaremos como adaptar seus efeitos e animações com a camada Visual nas áreas acima, mas há muitos outros meios para personalizar seu aplicativo a fim de garantir uma ótima experiência do usuário final. Os documentos de orientação estão disponíveis sobre como [personalizar sua interface do usuário](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) para vários dispositivos e [criar uma interface do usuário responsiva](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Configurações de efeitos de usuário

Os usuários podem personalizar sua experiência com o Windows por vários motivos, quais aplicativos devem respeitar e se adaptar ao. Os usuários finais de uma área podem controlar a alteração dos tipos de efeitos que eles veem em seu sistema.

### <a name="transparency-effects-settings"></a>Configurações de efeitos de transparência

Uma dessas configurações de efeito que os usuários podem personalizar é ativar/desativar efeitos de transparência. Isso pode ser encontrado no aplicativo configurações em personalização > cores, ou por meio de configurações aplicativo > facilidade de acesso > exibição.

![Opção de transparência nas configurações](images/tailoring-transparency-setting.png)

Quando ativado, qualquer efeito que use transparência será exibido como esperado. Isso se aplica a acrílico, HostBackdropBrush ou qualquer grafo de efeito personalizado que não seja totalmente opaco.

Quando desativado, o material de acrílico retornará automaticamente a uma cor sólida porque o pincel acrílico do XAML ouviu esse evento por padrão. Aqui, vemos que o aplicativo de calculadora retornando adequadamente a uma cor sólida quando os efeitos de transparência não estão habilitados:

![calculadora com acrílico](images/tailoring-acrylic.png)
![calculadora com acrílico respondendo às configurações de transparência](images/tailoring-acrylic-fallback.png)

No entanto, para quaisquer efeitos personalizados, o aplicativo precisa responder à propriedade [UISettings. AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) ou ao evento [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) e alternar o grafo de efeito/efeito para usar um efeito que não tenha transparência. Um exemplo disso é o seguinte:

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>Configurações de animação

Da mesma forma, os aplicativos devem escutar e responder à propriedade [UISettings. AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) para garantir que as animações sejam ativadas ou desativadas com base nas configurações do usuário em Configurações > facilidade de acesso > exibição.

![Opção de animações nas configurações](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Aproveitando a API de recursos

Aproveitando as APIs do [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) , você pode detectar quais recursos de composição estão disponíveis e o desempenho em determinado hardware e adaptar o design para garantir que os usuários finais obtenham uma experiência de alto desempenho e bonita em qualquer dispositivo. As APIs fornecem um meio para verificar os recursos do sistema de hardware a fim de implementar o dimensionamento de efeito normal em uma variedade de fatores forma. Isso facilita a personalização adequada do aplicativo para criar uma experiência de usuário final bonita e direta.

Essa API fornece métodos e um ouvinte de eventos que pode ser usado para fazer decisões de dimensionamento de efeito para a interface do usuário do aplicativo. O recurso detecta como o sistema pode lidar com operações complexas de composição e renderização e, em seguida, retorna as informações em um modelo fácil de consumir para os desenvolvedores utilizarem.

### <a name="using-composition-capabilities"></a>Usando funcionalidades de composição

A funcionalidade CompositionCapabilities já está sendo utilizada para recursos como o material do acrílico, em que o material volta a um efeito mais funcional, dependendo do cenário e do hardware.

A API pode ser adicionada ao código existente em algumas etapas fáceis.

1. Adquira o objeto Capabilities no construtor do seu aplicativo.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registre um ouvinte de eventos de recursos alterados para seu aplicativo.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Adicione conteúdo ao método de retorno de chamada de evento para lidar com vários níveis de recursos. Isso pode ou não ser semelhante à próxima etapa abaixo.
1. Ao usar efeitos, verifique primeiro o objeto Capabilities. Considere o uso de verificações condicionais ou de instruções de controle de alternância, dependendo de como você deseja adaptar os efeitos.

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

O código de exemplo completo pode ser encontrado no [repositório do GitHub da interface do usuário do Windows](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Efeitos rápidos versus lentos

Com base nos comentários dos métodos [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) e [AREEFFECTSFAST](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) fornecidos na API CompositionCapabilities, o aplicativo pode decidir trocar os efeitos caros ou não suportados por outros efeitos de sua escolha que são otimizados para o dispositivo. Alguns efeitos são conhecidos pelo uso consistente de mais recursos do que outros e devem ser usados com moderação, e outros efeitos podem ser usados mais livremente. Para todos os efeitos, no entanto, o cuidado deve ser usado ao encadear e animar, pois alguns cenários ou combinações podem alterar as características de desempenho do grafo de efeito. Abaixo estão algumas regras gerais de desempenho para efeitos individuais:

- Efeitos que são conhecidos por ter alto impacto no desempenho são os seguintes: Desfoque Gaussiano, máscara de sombra, BackDropBrush, HostBackDropBrush e Visual de camada. Eles não são recomendados para dispositivos de low-end [(nível de recurso 9.1-9,3)](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)e devem ser usados criteriosamente em dispositivos de alta linha.
- Efeitos com impacto de desempenho médio incluem matriz de cores, determinado efeito de mistura BlendModes (luminosidade, cor, saturação e matiz), SpotLight, SceneLightingEffect e (dependendo do cenário) BorderEffect. Esses efeitos podem funcionar com determinados cenários em dispositivos de low-end, mas devem ser usados ao encadear e animar. Recomenda-se restringir o uso para dois ou menos e animando somente em transições.
- Todos os outros efeitos têm baixo impacto no desempenho e funcionam em todos os cenários razoáveis ao animar e encadear.

## <a name="related-articles"></a>{1&gt;{2&gt;Artigos relacionados&lt;2}&lt;1}

- [Técnicas de design responsivas da UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Personalização de dispositivos UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
