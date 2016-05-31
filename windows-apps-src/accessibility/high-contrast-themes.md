---
author: Xansky
Description: Descreve as etapas necessárias para garantir que seu aplicativo UWP (Plataforma Universal do Windows) seja utilizável quando um tema de alto contraste estiver ativo.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Temas de alto contraste
label: High-contrast themes
template: detail.hbs
---

# Temas de alto contraste  



Descreve as etapas necessárias para garantir que seu aplicativo UWP (Plataforma Universal do Windows) seja utilizável quando um tema de alto contraste estiver ativo.

O aplicativo UWP oferece suporte a temas de alto contraste. Quando o usuário decide que o sistema deve utilizar um tema de alto contraste das configurações de sistema ou ferramentas de acessibilidade, a estrutura usa automaticamente as cores e as configurações de estilo que produzem layout e renderização de alto contraste para controles e componentes na interface do usuário.

Esse suporte padrão baseia-se no uso de temas e modelos padrão. Esses temas e modelos fazem referência às cores do sistema como definições de recursos, e as origens de recursos são alteradas automaticamente quando o sistema usa um modo de alto contraste. No entanto, se você usar modelos, temas e estilos personalizados para o seu controle, tenha cuidado para não desabilitar o suporte interno a alto contraste. Se você usar um dos designers XAML do Microsoft Visual Studio para criar estilo, o designer gerará um tema de alto contraste separado, juntamente com o tema principal, sempre que você definir um modelo que seja significativamente diferente do modelo padrão. Os dicionários de temas separados vão para a coleção [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807), uma propriedade dedicada de um elemento [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794).

Para obter mais informações sobre temas e modelos de controle, consulte [Guia de início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374). Às vezes, é útil procurar controles específicos no dicionário de recursos XAML e ver como os temas são construídos e como fazem referência a recursos semelhantes, mas diferentes para cada configuração de alto contraste possível.

<span id="Detecting_when_a_high-contrast_theme_is_enabled"/>
<span id="detecting_when_a_high-contrast_theme_is_enabled"/>
<span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"/>
## Detectando quando um tema de alto contraste está habilitado  
Um aplicativo UWP pode usar membros da classe [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) para detectar as configurações atuais de temas de alto contraste. A propriedade [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast) determina se um tema de alto contraste está selecionado no momento. Se **HighContrast** estiver definido como **true**, a próxima etapa será verificar o valor da propriedade [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme) para obter o nome do tema de alto contraste que será usado. "Branco em alto contraste" e "Preto em alto contraste" geralmente são valores para **HighContrastScheme** ao qual seu código deve responder. As chaves de [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) definidas por XAML não podem ter espaços, portanto, as chaves desses temas em um dicionário de recursos normalmente são respectivamente "HighContrastWhite" e "HighContrastBlack". Você também deve ter lógica de fallback para um tema de alto contraste padrão caso o valor seja alguma outra cadeia de caracteres. O [Exemplo de alto contraste XAML](http://go.microsoft.com/fwlink/p/?linkid=254993) mostra essa lógica.

> [!NOTE] Chame um construtor [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237) de um escopo onde o aplicativo esteja inicializado e o conteúdo já esteja sendo exibido.

Os aplicativos podem alternar o uso para valores de recursos de alto contraste durante sua execução. Isso funciona quando os recursos são solicitados pela [Extensão de marcação {ThemeResource}](https://msdn.microsoft.com/library/windows/apps/Mt185591) no XAML de estilo ou modelo. Todos os temas padrão (generic.xaml) usam essa técnica de extensão de marcação {ThemeResource}, portanto, você conseguirá esse comportamento ao usar os temas de controle padrão. Os controles personalizados ou o estilo de controle personalizado fazem isso quando você também utilizou essa técnica de recurso extensão de marcação {ThemeResource} em seus modelos e estilos personalizados.

<span id="related_topics"/>
## Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Amostra de configurações e contraste da interface do usuário](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Amostra de alto contraste XAML](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)


<!--HONumber=May16_HO2-->


