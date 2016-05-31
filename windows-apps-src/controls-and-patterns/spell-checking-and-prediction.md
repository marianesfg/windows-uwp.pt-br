---
author: Jwmsft
Description: Durante a edição e a entrada de texto, a verificação ortográfica informa o usuário que uma palavra está com grafia incorreta realçando-a com uma linha ondulada vermelha e fornecendo uma maneira de o usuário corrigir o erro de ortografia.
title: Verificação ortográfica e previsão de texto
ms.assetid: B867C956-5AB2-4207-A8DE-179CE7871180
label: Spell checking and text prediction
template: detail.hbs
---

# Diretrizes para verificação ortográfica

Durante a edição e a entrada de texto, a verificação ortográfica informa o usuário que uma palavra está com grafia incorreta realçando-a com uma linha ondulada vermelha e fornece uma maneira de o usuário corrigir o erro de ortografia.

**APIs importantes**

-   [**IsSpellCheckEnabled property (XAML)**](https://msdn.microsoft.com/library/windows/apps/br209688)


## <span id="checklist_section"></span><span id="CHECKLIST_SECTION"></span>Recomendações


-   Use a verificação ortográfica para ajudar os usuários a inserir palavras ou frases em controles de entrada de texto. A verificação ortográfica funciona com entradas por touch, mouse e teclado.
-   Não use a verificação ortográfica quando uma palavra provavelmente não estará no dicionário ou se os usuários não a valorizarão. Por exemplo, não a ative em caixas de entrada de senhas, números de telefone ou nomes. A verificação ortográfica está desabilitada por padrão para esses controles.
-   Não desabilite a verificação ortográfica só porque o atual mecanismo de correção ortográfica não oferece suporte ao idioma do seu aplicativo. Quando o verificador ortográfico não oferece suporte a um idioma, ele não faz nada, então não há nenhum mal em deixar a opção habilitada. Além disso, alguns usuários podem usar um IME para inserir outro idioma em seu aplicativo, e pode haver suporte para esse idioma. Por exemplo, ao criar um aplicativo em japonês, mesmo que o mecanismo de verificação ortográfica não possa reconhecer no momento esse idioma, não desative a verificação ortográfica. O usuário pode alternar para um IME em inglês e digitar inglês no aplicativo; se a verificação ortográfica estiver habilitada, a ortografia em inglês será verificada.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Diretriz de uso adicional


Os aplicativos da Windows Store fornecem um verificador ortográfico interno para caixas de entrada de texto com várias linhas e de texto simples e elementos que têm a propriedade **contentEditable** definida como **true**. Veja aqui um exemplo do verificador ortográfico interno:

![o verificador ortográfico interno](images/spellchecking.png)

Para obter mais informações, consulte a [ **classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683).

Use a verificação ortográfica com controles de entrada de texto para estas duas finalidades:

-   **Para corrigir erros ortográficos automaticamente**

    O mecanismo de verificação ortográfico corrige automaticamente palavras incorretas quando tem certeza sobre a correção. Por exemplo, o mecanismo altera automaticamente "teh" para "the".

-   **Para mostrar grafias alternativas**

    Quando o mecanismo de verificação ortográfica não tem certeza sobre as correções, ele adiciona uma linha vermelha abaixo da palavra incorreta e exibe as alternativas em um menu de contexto quando você toca ou clica com o botão direito do mouse na palavra.

Para controles JavaScript, a verificação ortográfica está habilitada por padrão para controles de entrada de texto multilinha e desabilitada para controles de linha única. Você pode ativá-la manualmente para controles de linha única definindo a propriedade **spellcheck** do controle como **true**. Você pode desativar a verificação ortográfica para um controle definindo sua propriedade **spellcheck** como **false**.

Para controles TextBox XAML, a verificação ortográfica está desabilitada por padrão. Você pode habilitá-la definindo a propriedade **IsSpellCheckEnabled** como **true**.



## <span id="related_topics"></span>Artigos relacionados

* [Texto e controles de texto](text-controls.md)
* [Diretrizes para entrada de texto](https://msdn.microsoft.com/library/windows/apps/hh750315)
* [Diretrizes para texto e tipografia](https://msdn.microsoft.com/library/windows/apps/hh700394)
            
          
            **Para desenvolvedores (XAML)**
* [**Propriedade TextBox.IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688)
* [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)

 






<!--HONumber=May16_HO2-->


