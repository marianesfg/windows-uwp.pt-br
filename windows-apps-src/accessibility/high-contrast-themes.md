---
author: Xansky
Description: "Descreve as etapas necessárias para garantir que seu aplicativo UWP (Plataforma Universal do Windows) seja utilizável quando um tema de alto contraste estiver ativo."
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Temas de alto contraste
label: High-contrast themes
template: detail.hbs
ms.sourcegitcommit: 50c37d71d3455fc2417d70f04e08a9daff2e881e
ms.openlocfilehash: 4201f5a0b08f1fc8d691218da0803ee04ab2c86a

---

# Temas de alto contraste  

Descreve as etapas necessárias para garantir que seu aplicativo UWP (Plataforma Universal do Windows) seja utilizável quando um tema de alto contraste estiver ativo.

O aplicativo UWP oferece suporte a temas de alto contraste. Quando o usuário decide que o sistema deve utilizar um tema de alto contraste das configurações de sistema ou ferramentas de acessibilidade, a estrutura usa automaticamente as cores e as configurações de estilo que produzem layout e renderização de alto contraste para controles e componentes na interface do usuário.

Esse suporte padrão baseia-se no uso de temas e modelos padrão. Esses temas e modelos fazem referência às cores do sistema como definições de recursos, e as origens de recursos são alteradas automaticamente quando o sistema usa um modo de alto contraste. No entanto, se você usar modelos, temas e estilos personalizados para o seu controle, tenha cuidado para não desabilitar o suporte interno a alto contraste. Se você usar um dos designers XAML do Microsoft Visual Studio para criar estilo, o designer gerará um tema de alto contraste separado, juntamente com o tema principal, sempre que você definir um modelo que seja significativamente diferente do modelo padrão. Os dicionários de temas separados vão para a coleção [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx), uma propriedade dedicada de um elemento [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794).

Para obter mais informações sobre temas e modelos de controle, consulte [Guia de início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374). Às vezes, é útil procurar controles específicos no dicionário de recursos XAML e ver como os temas são construídos e como fazem referência a recursos semelhantes, mas diferentes para cada configuração de alto contraste possível.

## Dicionários de temas

Quando você precisar alterar uma cor do padrão do sistema ou precisar adicionar imagens como decoração, como uma imagem da tela de fundo, crie uma coleção **ThemeDictionaries** para seu aplicativo.

* Comece criando o caminho adequado, caso ainda não exista. Em App.xaml, crie uma coleção **ThemeDictionaries**:

``` xaml
 <Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">

            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">

            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources
```

* **HighContrast** não é o único nome de chave disponível. Também há **HighContrastBlack**, **HighContrastWhite** e **HighContrastCustom**. Na maioria dos casos, **HighContrast** é tudo que você precisa.
* Em **Default**, crie o tipo de [**Brush**](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) de que você precisa, geralmente um **SolidColorBrush**. Dê a ele um nome de **x:Key** específico para o que ele está sendo usado:<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" />`
* Atribua a **Color** desejada para ele:<br/>
    `<SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />`
* Copie esse **Brush** em **HighContrast**:

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

* Determinar qual cor seu **Brush** deve ter e modifique-o em **HighContrast**.

Determinar uma cor para alto contraste requer um pouco de aprendizagem. O caminho que você criou acima facilita a atualização.

## Cores de alto contraste

Os usuários podem alternar para usar a página de configurações de alto contraste. Há 4 temas de alto contraste por padrão. Depois que o usuário seleciona uma opção, a página mostra uma visualização de como os aplicativos provavelmente ficarão.

![Configurações de alto contraste](images/high-contrast-settings.png)<br/>
_Configurações de alto contraste_

 Cada quadrado na visualização pode ser clicado para alterar seu valor. Cada quadrado também é mapeado diretamente para um recurso do sistema.

![Recursos de alto contraste](images/high-contrast-resources.png)<br/>
_Recursos de alto contraste_

Se você prefixar os nomes explicados acima com _SystemColor_ e sufixá-los com _Color_, por exemplo: **SystemColorWindowTextColor**, eles serão atualizados dinamicamente para coincidir com o que o usuário especificou. Isso libera você de precisar escolher uma cor específica para alto contraste. Em vez disso, escolha um recurso do sistema que corresponda àquele para o qual a cor está sendo usada. No exemplo acima, nomeamos nossa cor da tela de fundo da página como **SolidColorBrushBrandedPageBackground**. Como ela será usada para uma tela de fundo, é possível mapeá-la para o **SystemColorWindowColor** em alto contraste:

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Se você continuar com a paleta de 8 cores de alto contraste, não precisará criar quaisquer **ResourceDictionaries** de alto contraste adicionais. Essa paleta limitada muitas vezes pode apresentar desafios difíceis na representação de estados visuais complexos. Muitas vezes, adicionar uma borda a uma área somente em alto contraste pode ajudar a esclarecer uma situação.

### O que fazer e o que não fazer

* Faça o teste no modo de alto contraste com antecedência e frequência.
* Use as cores nomeadas para a finalidade definida.
* Coloque primitivos como **Color**, **Brush** e **Thickness** dentro de **ThemeDictionaries**. Evite colocar recursos mais complexos como elementos **Style** neles. O exemplo a seguir funciona bem:

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="Red" />
            </ResourceDictionary>
            <!-- HighContrast is used in any high contrast theme -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackground" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>

        <Style x:Key="MyButtonStyle" TargetType="Button">
            <Setter Property="Foreground" Value="{ThemeResource BrandedPageBackground}" />
        </Style>
    </ResourceDictionary>
</Application.Resources>

...

<Button Style="{StaticResource MyButtonStyle}" />
```

* Use cores de primeiro plano de alto contraste para elementos da interface do usuário de primeiro plano.
* Use cores de alto contraste com o par de cores definido. Por exemplo, use sempre **BUTTONTEXT** com **BUTTONFACE**, especialmente em uma situação de primeiro/segundo plano.
* Use o par de cores de alto contraste recomendado para um determinado elemento da interface do usuário para garantir que o contraste 14:1 necessário seja atendido.
* NÃO separe os pares de cores de alto contraste nem misture e combine cores de alto contraste arbitrariamente. Isso geralmente cria uma interface do usuário invisível para no mínimo um dos temas de alto contraste pré-instalados.
* NÃO coloque quaisquer objetos **Brush** que você criar fora de uma coleção **ThemeDictionaries**.
* Nunca use **StaticResource** para fazer referência a um recurso em uma coleção **ThemeDictionaries**. Isso vai dar a ilusão de funcionar até o usuário alterar os temas enquanto seu aplicativo estiver em execução. Use **ThemeResource**.
* NÃO use valores de cor codificados.
* NÃO use uma cor só porque você gosta dela.

Consulte [Recursos de temas XAML](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources) para obter mais informações.

## Quando usar bordas
No modo de alto contraste, adicione uma borda a um elemento de interface do usuário quando for necessário manter uma forma de limite reconhecível no item. Use bordas para diferenciar entre as áreas de conteúdo de navegação, ações e conteúdo.

![Um painel de navegação separado do restante da página](images/high-contrast-actions-content.png)<br/>
_Um painel de navegação separado do restante da página_

Se um elemento da interface do usuário _não_ tiver uma borda ou tela de fundo por padrão, não adicione uma borda ou tela de fundo ao estado padrão no modo de alto contraste.

Se um elemento da interface do usuário _tiver_ uma borda por padrão, mantenha a borda no modo de alto contraste.

Cores sobrepostas ou adjacentes devem ser distinguíveis entre si, mas eles não necessariamente precisam atender à taxa de contraste de cor de 14:1. No entanto, a prática recomendada é uma taxa de contraste de 3:1 para esses tipos de cenários.

Se as cores da tela de fundo de alto contraste forem usadas para diferenciar os elementos sobrepostos da interface do usuário, o único método garantido para assegurar o contraste entre esses elementos é introduzir bordas.

## Detectando quando um tema de alto contraste está habilitado  
Use membros da classe [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) para detectar as configurações atuais de temas de alto contraste. A propriedade [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrast) determina se um tema de alto contraste está selecionado no momento. Se **HighContrast** estiver definido como **true**, a próxima etapa será verificar o valor da propriedade [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.accessibilitysettings.highcontrastscheme) para obter o nome do tema de alto contraste que será usado. "Branco em alto contraste" e "Preto em alto contraste" geralmente são valores para **HighContrastScheme** ao qual seu código deve responder. As chaves de [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) definidas por XAML não podem ter espaços, portanto, as chaves desses temas em um dicionário de recursos normalmente são respectivamente "HighContrastWhite" e "HighContrastBlack". Você também deve ter lógica de fallback para um tema de alto contraste padrão caso o valor seja alguma outra cadeia de caracteres. O [Exemplo de alto contraste XAML](http://go.microsoft.com/fwlink/p/?linkid=254993) mostra essa lógica.

> [!NOTE]
> Assegure-se de chamar um construtor [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) de um escopo onde o aplicativo é inicializado e o conteúdo já esteja sendo exibido.

Os aplicativos podem alternar o uso para valores de recursos de alto contraste durante sua execução. Isso funciona quando os recursos são solicitados pela [Extensão de marcação {ThemeResource}](https://msdn.microsoft.com/library/windows/apps/Mt185591) no XAML de estilo ou modelo. Todos os temas padrão (generic.xaml) usam essa técnica de extensão de marcação {ThemeResource}, portanto, você conseguirá esse comportamento ao usar os temas de controle padrão. Os controles personalizados ou o estilo de controle personalizado fazem isso quando você também utilizou essa técnica de recurso extensão de marcação {ThemeResource} em seus modelos e estilos personalizados.

## Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Amostra de configurações e contraste da interface do usuário](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Amostra de alto contraste XAML](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)



<!--HONumber=Jun16_HO4-->


