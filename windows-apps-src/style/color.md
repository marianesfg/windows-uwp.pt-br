---
Description: As cores tornam a orientação intuitiva por meio de vários níveis de informações de um aplicativo e são uma ferramenta essencial para reforçar o modelo de interação.
title: Cores
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# Cores para aplicativos UWP
As cores tornam a orientação intuitiva por meio de vários níveis de informações de um aplicativo e são uma ferramenta essencial para reforçar o modelo de interação.

## Cor de destaque

O usuário pode selecionar uma única cor, chamada de destaque. Eles podem escolher opções em um conjunto administrado de 48 amostras de cores.


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>Como regra geral, quando a cor de destaque original é usada como plano de fundo, sempre use texto branco sobre ela.</figcaption>
</figure>

Quando usuários escolhem uma cor de destaque, esta aparece como parte do seu tema do sistema. As áreas afetadas são a tela inicial, a barra de tarefas, o cromado de janela, estados de interação selecionados e hiperlinks dentro de [controles comuns](https://dev.windows.com/design/controls-patterns). Cada aplicativo pode incorporar ainda mais a cor de destaque em suas tipografias, planos de fundo e interações, ou ainda anulá-la para preservar sua identidade visual específica.

## Seleção de cores

Depois que uma cor de destaque é selecionada, tonalidades claras e escuras dessa cor são criadas com base nos valores HCL de luminosidade de cor. Aplicativos podem usar variações de tonalidades para criar uma hierarquia visual e fornecer uma indicação de interação.

![Uma única cor de destaque com suas 6 tonalidades](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## Temas de cores

O usuário também pode escolher entre um tema claro ou escuro para o sistema (no telefone, mas tablets e desktops ainda não têm essa opção, podendo fornecer uma configuração dentro do aplicativo). Alguns aplicativos optam por modificar seus temas com base na preferência do usuário, enquanto outros se recusam a fazer isso.

Aplicativos que usam o tema claro são para cenários que envolvem aplicativos de produtividade. Exemplos seriam o pacote de aplicativos disponíveis com o Microsoft Office. O tema claro facilita a leitura de textos longos em conjunto com longos períodos de tempo em uma determinada tarefa.

O tema escuro permite um contraste mais visível de conteúdo para aplicativos centrados em mídia ou cenários em que os usuários uma grande variedade de vídeos ou imagens é apresentada ao usuário. Nesses cenários, a leitura não é necessariamente a principal tarefa, embora a experiência de assistir a um filme possa ser, e o conteúdo é mostrado em condições de pouca luz ambiente.

Se o seu aplicativo não se enquadra exatamente em nenhuma dessas descrições, considere seguir o tema do sistema para permitir que o usuário decida qual é a opção ideal para ele.

Para facilitar o design de temas, o Windows fornece uma paleta de cores adicional que se adapta automaticamente ao tema.


<!-- OP version -->
### Tema claro
#### Base
![O tema claro base](images/themes-light-base.png)
#### Alt
![O tema claro alternativo](images/themes-light-alt.png)
#### Lista
![O tema claro de lista](images/themes-light-list.png)
#### Cromado
![O tema claro cromado](images/themes-light-chrome.png)
### Tema escuro
#### Base
![O tema escuro base](images/themes-dark-base.png)
#### Alt
![O tema escuro alternativo](images/themes-dark-alt.png)
#### Lista
![O tema escuro de lista](images/themes-dark-list.png)
#### Cromado
![O tema escuro cromado](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## Acessibilidade

Nossa paleta é otimizada para uso na tela. Convém manter uma proporção de contraste mínima para o texto de 4.5: 1 para permitir uma leitura ideal.


<!--HONumber=Mar16_HO5-->


