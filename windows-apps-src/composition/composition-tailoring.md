---
author: daneuber
title: Adaptar de composição
description: Use as APIs de composição para adaptar sua interface do usuário, otimizar o desempenho e acomodar características de dispositivo e as configurações do usuário.
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66384c4df3195ae0fff35ae5dd7e1b1983204068
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4392213"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Adaptar efeitos e experiências usando a interface do usuário do Windows

Interface do usuário do Windows fornece muitos belos efeitos, animações e meios para diferenciação. No entanto, as expectativas do usuário para desempenho e personalização da reunião ainda é uma parte fundamental da criação de aplicativos com êxito. A plataforma universal do Windows dá suporte a uma grande e diversificada família de dispositivos, que possuem diferentes recursos e funcionalidades. Para proporcionar uma experiência inclusiva para todos os seus usuários, você precisará garantir que sua escala de aplicativos em dispositivos e respeitar as preferências do usuário. Adaptar de interface do usuário pode fornecer uma maneira eficiente para aproveitar os recursos do dispositivo e garantir uma experiência de usuário agradável e inclusivo.

Adaptar de interface do usuário é uma categoria ampla que abrange o trabalho de alto desempenho, interface do usuário bonitas com relação às seguintes áreas:

- Respeitando e adaptando às configurações do usuário para efeitos
- Acomodar configurações do usuário para animações
- Otimização da interface do usuário para os recursos de hardware específica

Aqui, abordaremos como adaptar seu efeitos e animações com a camada Visual nas áreas acima, mas há muitos outros meios para adaptar seu aplicativo para garantir uma experiência de usuário final excelente. Documentos de orientação estão disponíveis como [adaptar sua interface do usuário](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) para vários dispositivos e [criar a interface do usuário responsiva](/design/layout/responsive-design.md).

## <a name="user-effects-settings"></a>Configurações de efeitos de usuário

Os usuários podem personalizar a experiência do Windows por vários motivos, quais aplicativos devem respeitar e se adaptar a. Uma área que os usuários finais podem controlar está mudando os tipos de efeitos que eles verão usados em todo o sistema.

### <a name="transparency-effects-settings"></a>Configurações de efeitos de transparência

Uma configuração assim efeito os usuários podem personalizar está ativando efeitos de transparência ativar/desativar. Isso pode ser encontrado no aplicativo configurações em personalização > cores, ou por meio do aplicativo Configurações > facilidade de acesso > exibição.

![Opção de transparência nas configurações](images/tailoring-transparency-setting.png)

Quando ativado, qualquer efeito que usa transparência aparecerão conforme o esperado. Isso se aplica a acrílico, HostBackdropBrush ou qualquer gráfico de efeito personalizado que não é totalmente opaco.

Quando desativadas, material acrílico automaticamente se voltará para uma cor sólida porque pincel acrílico do XAML tem ouvido esse evento por padrão. Aqui, vemos que o aplicativo Calculadora adequadamente voltando para uma cor sólida quando efeitos de transparência não estão habilitados:

![Calculadora com acrílico](images/tailoring-acrylic.png)
![Calculadora com acrílico responder às configurações de transparência](images/tailoring-acrylic-fallback.png)

No entanto, para qualquer efeitos personalizados o aplicativo deve responder à propriedade [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) ou [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) evento e alternar o gráfico de efeito/efeito para usar um efeito que tem sem transparência. Um exemplo disso é abaixo:

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

## <a name="animations-settings"></a>Configurações de animações

Da mesma forma, os aplicativos devem ouvir e responder à propriedade [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) para garantir que animações são ativadas ou desativado com base nas configurações do usuário em Configurações > facilidade de acesso > exibição.

![Opção de animações em configurações](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Aproveitar os recursos de API

Aproveitando [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) APIs, você pode detectar quais composição são os recursos disponível e eficiente em determinadas hardware e adaptar o design para garantir que os usuários finais obter uma experiência bela e eficiente em qualquer dispositivo. As APIs fornecem um meio para verificar se há recursos de sistema de hardware para implementar o efeito normal dimensionamento em uma variedade de fatores forma. Isso torna mais fácil personalizar adequadamente o aplicativo para criar um belo e uma experiência perfeita ao usuário final.

Essa API fornece métodos e um ouvinte de eventos que pode ser usado para tornar o efeito dimensionamento decisões para o aplicativo da interface do usuário. O recurso detecta quão bem o sistema pode manipular complexa composição e renderização operações e, em seguida, retorna as informações em um modelo para os desenvolvedores utilizar fácil consumir.

### <a name="using-composition-capabilities"></a>Usando recursos de composição

A funcionalidade de CompositionCapabilities já está sendo usada para recursos como material acrílico, onde o material de volta para um efeito de alto desempenho mais dependendo do cenário e hardware.

A API pode ser adicionada ao código existente em algumas etapas simples.

1. Adquira o objeto de recursos no construtor do seu aplicativo.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registramos um ouvinte de eventos alterados de recursos para seu aplicativo.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Adicione conteúdo para o método de retorno de chamada de eventos para lidar com vários níveis de recursos. Isso pode ou não ser semelhante para a próxima etapa abaixo.
1. Ao usar efeitos, verifique primeiro o objeto de recursos. Considere usar verificações condicionais ou alternar instruções de controle, dependendo de como você deseja personalizar os efeitos.

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

Código de exemplo completo pode ser encontrado no [repositório do Github de interface do usuário do Windows](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Rápida versus efeitos lentos

Com base nos comentários dos métodos [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) e [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) fornecidos na API CompositionCapabilties, o aplicativo pode optar por trocar efeitos caros ou sem suporte para outros efeitos de sua escolha que são otimizados para o dispositivo. Alguns efeitos são conhecidos por serem consistentemente mais intensivo que outros e devem ser usados com moderação e outros efeitos podem ser usados mais livremente. Para todos os efeitos, no entanto, cuidado deve ser usado quando encadeamento e animação como alguns cenários ou combinações podem alterar as características de desempenho do gráfico de efeito. Abaixo estão algumas características de desempenho de regra prática para efeitos individuais:

- Efeitos que são conhecidos por ter impacto de alto desempenho são as seguintes – Desfoque Gaussiano, máscara de sombra, BackDropBrush, HostBackDropBrush e a camada Visual. Eles não são recomendados para dispositivos de baixa capacidade [(nível de recursos 9.1 9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)e devem ser usados criteriosamente em dispositivos de high-end.
- Efeitos com impacto no desempenho médio incluem a matriz de cores, determinados BlendModes de efeito de mesclagem (luminosidade, cor, saturação e matiz), destaque, SceneLightingEffect e (dependendo do cenário) BorderEffect. Esses efeitos podem funcionar com determinados cenários em dispositivos de baixa capacidade, mas cuidado deve ser usado quando encadeamento e animar. Recomendamos restringir o uso de dois ou menos e criando a animação transições somente.
- Todos os outros efeitos têm impacto no desempenho de baixa e funcionam em todos os cenários razoáveis quando animando e encadeamento.

## <a name="related-articles"></a>Artigos relacionados

- [Técnicas de Design responsivo de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Adaptar de dispositivo UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
