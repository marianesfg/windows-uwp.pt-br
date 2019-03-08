---
title: Adaptando de composição
description: Use as APIs de composição para personalizar a interface do usuário, otimizar o desempenho e acomodar as configurações de usuário e características de dispositivo.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644411"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Adaptando efeitos & experiências usando a interface do usuário do Windows

Interface do usuário do Windows fornece muitos efeitos belos, animações e meios para diferenciação. No entanto, atendendo às expectativas do usuário para desempenho e capacidade de personalização ainda é uma parte necessária da criação de aplicativos com êxito. A plataforma Universal do Windows oferece suporte a uma família de grande e diversificada de dispositivos que têm recursos e funcionalidades diferentes. Para fornecer uma experiência inclusiva para todos os usuários, você precisa garantir que seu dimensionamento de aplicativos entre dispositivos e respeitar as preferências do usuário. Adaptando de interface do usuário pode fornecer uma maneira eficiente de aproveitar os recursos de um dispositivo e garantir uma experiência de usuário agradável e inclusivo.

Adaptando de interface do usuário é uma categoria ampla que abranja o trabalho de alto desempenho, bela interface do usuário em relação aos seguintes áreas:

- Respeitando e se adaptando a configurações de usuário para efeitos
- Acomodar configurações do usuário para animações
- Otimizando a interface do usuário para os recursos de hardware específico

Aqui, vamos abordar como adaptar seus efeitos e animações com a camada Visual nas áreas acima, mas há muitos outros meios para personalizar seu aplicativo para garantir uma experiência excelente dos usuários finais. Os documentos de diretrizes estão disponíveis sobre como [adaptar sua interface do usuário](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) para vários dispositivos e [criar a interface de usuário responsiva](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Configurações de efeitos de usuário

Os usuários podem personalizar sua experiência do Windows para uma variedade de motivos, quais aplicativos devem respeitar e se adaptar a. Uma área que os usuários finais pode controlar é alterar os tipos de efeitos que eles veem usados em todo o seu sistema.

### <a name="transparency-effects-settings"></a>Configurações de efeitos de transparência

Efeitos de transparência liga/desliga está se tornando um tal configuração de efeito, os usuários podem personalizar. Isso pode ser encontrado no aplicativo de configurações em personalização > cores, ou por meio do aplicativo de configurações > facilidade de acesso > exibição.

![Opção nas configurações de transparência](images/tailoring-transparency-setting.png)

Quando ativada, qualquer efeito que usa transparência será exibida conforme o esperado. Isso se aplica a tinta Acrílica, HostBackdropBrush ou qualquer outro gráfico efeito personalizado que não é totalmente opaco.

Quando desativado, material acrílico fará automaticamente o fallback para uma cor sólida porque o pincel de acrílico do XAML ouviu esse evento por padrão. Aqui, podemos ver o aplicativo de calculadora adequadamente fazer fallback para uma cor sólida quando efeitos de transparência não estão habilitados:

![Calculadora com tinta Acrílica](images/tailoring-acrylic.png)
![Calculadora com tinta Acrílica respondendo às configurações de transparência](images/tailoring-acrylic-fallback.png)

No entanto, para qualquer personalizada afeta o aplicativo precisa responder à [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) propriedade ou [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) eventos e switch out o efeito/efeito gráfico a ser usado um efeito com nenhuma transparência. Veja abaixo um exemplo disso:

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

Da mesma forma, os aplicativos devem ouvir e responder à [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) propriedade para garantir que as animações são ativados ou desativados com base nas configurações do usuário em Configurações > facilidade de acesso > exibição.

![Opção de animações nas configurações](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Aproveitando os recursos de API

Aproveitando os [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) APIs, você pode detectar a composição de quais recursos estão disponíveis e alto desempenho em determinado hardware e ajustar o design para garantir que os usuários finais obtêm uma experiência bonita e alto desempenho em qualquer dispositivo. As APIs fornecem um meio para verificar os recursos de sistema de hardware para implementar o dimensionamento em uma variedade de fatores forma de efeito normal. Isso torna mais fácil de ajustar adequadamente o aplicativo para criar um lindo e uma experiência perfeita ao usuário final.

Essa API fornece métodos e um ouvinte de evento que pode ser usado para fazer efeito dimensionar decisões para o aplicativo da interface do usuário. O recurso detecta o quão bem o sistema pode manipular composição complexa e operações de renderização e, em seguida, retorna as informações em um modelo fácil de consumir para os desenvolvedores a utilizar.

### <a name="using-composition-capabilities"></a>Usando recursos de composição

A funcionalidade de CompositionCapabilities já é utilizada para recursos como o material acrílico, em que o material reverterá para um efeito de alto desempenho mais dependendo do cenário e hardware.

A API pode ser adicionada ao código existente em poucas etapas simples.

1. Adquira o objeto de recursos no construtor do seu aplicativo.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registre um ouvinte de evento de alteração de recursos para seu aplicativo.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Adicione conteúdo para o método de retorno de chamada do evento para lidar com vários níveis de recursos. Isso pode ou não pode ser semelhante à próxima etapa.
1. Ao usar efeitos, verifique o objeto de recursos primeiro. Considere o uso de verificações condicionais ou instruções de controle, dependendo de como você deseja personalizar os efeitos de troca.

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

Exemplo de código completo pode ser encontrado na [repositório Github de interface do usuário do Windows](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Fast versus efeitos lento

Com base nos comentários fornecidos [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) e [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) métodos na API CompositionCapabilities, o aplicativo pode decidir trocar os efeitos de caros ou sem suporte para outros efeitos de sua escolha que são otimizados para o dispositivo. Alguns efeitos são conhecidos como recursos mais intensivamente que outros consistentemente e devem ser usados com moderação e outros efeitos podem ser usados mais livremente. Para todos os efeitos, no entanto, cuidado deve ser usado quando encadeamento e animação como alguns cenários ou combinações podem alterar as características de desempenho do gráfico em vigor. Abaixo estão algumas características de desempenho de regra para efeitos de individuais:

- Os efeitos que são conhecidos por ter impacto de alto desempenho são os seguintes – Desfoque Gaussiano, máscara de sombra, BackDropBrush, HostBackDropBrush e camada Visual. Não são recomendadas para dispositivos de baixo nível [(9.3 9.1 de nível de recurso)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)e deve ser usada com cuidado em dispositivos de high-end.
- Efeitos de impacto de desempenho médio incluem a matriz de cores, determinados BlendModes de efeito do Blend (luminosidade, cor, saturação e matiz), (dependendo do cenário), SceneLightingEffect e destaque BorderEffect. Esses efeitos podem funcionar com determinados cenários em dispositivos de baixo nível, mas cuidado deve ser usado quando o encadeamento e animação. Recomendável restringir o uso para dois ou menos e animação nas transições apenas.
- Todos os outros efeitos tem impacto baixo desempenho e trabalhar em todos os cenários razoáveis quando a animação e encadeamento.

## <a name="related-articles"></a>Artigos relacionados

- [Técnicas de Design dinâmico de UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [Adaptando de dispositivo com a UWP](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
