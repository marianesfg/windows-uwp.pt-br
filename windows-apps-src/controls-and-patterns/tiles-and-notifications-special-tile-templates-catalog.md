---
author: mijacobs
Description: "Modelos de blocos especiais são modelos exclusivos que são animados ou apenas permitem fazer coisas que não são possíveis com blocos adaptáveis."
title: Modelos de blocos especiais
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
label: TBD
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 791722752ee6ab6de92f5717d62ed03d833f184c
ms.lasthandoff: 02/07/2017

---
# <a name="special-tile-templates"></a>Modelos de blocos especiais
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Modelos de blocos especiais são modelos exclusivos que são animados ou apenas permitem fazer coisas que não são possíveis com blocos adaptáveis. Cada modelo de bloco especial foi compilado especificamente para o Windows 10, exceto para o modelo de bloco icônico, um modelo especial clássico que foi atualizado para o Windows 10. Este artigo aborda três modelos de blocos especiais: Icônico, Fotos e Pessoas.

## <a name="iconic-tile-template"></a>Modelo de bloco icônico


O modelo icônico (também conhecido como o modelo "IconWithBadge") permite exibir uma imagem pequena no centro do bloco. O Windows 10 dá suporte ao modelo em telefone e tablet/desktop.

![blocos de email pequenos e médios](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>Como criar um bloco icônico

As etapas a seguir abrangem tudo o que você precisa saber para criar um bloco icônico para o Windows 10. Em um nível alto, você precisa do ativo de imagem icônico e, em seguida, envia uma notificação para o bloco usando o modelo icônico e, por fim, uma notificação de selo que fornece o número a ser exibido no bloco.

![fluxo de desenvolvedor de bloco icônico](images/iconic-template-dev-flow.png)

**Etapa 1: Criar os ativos de imagem em formato PNG**

Crie os ativos de ícone para o bloco e os coloque nos recursos de projeto com os outros ativos. No mínimo, crie um ícone de 200x200 pixels, que funciona para blocos pequenos e médios no telefone e no desktop. Para proporcionar a melhor experiência de usuário, crie um ícone para cada tamanho. Consulte os detalhes de dimensionamento na imagem abaixo.

Salve ativos de ícone em formato PNG e com transparência. No Windows Phone, cada pixel não transparentes é exibido como branco (RGB 255, 255, 255). Para manter a consistência e a simplicidade, use branco para ícones da área de trabalho também.

O Windows 10 em tablet, laptop e desktop só dá suporte a ativos de ícone quadrados. O telefone dá suporte a ativos quadrados e ativos mais altos do que têm de largura, até uma proporção largura:altura 2:3, que é útil para imagens como um ícone de telefone.

![dimensionamento de ícone em blocos pequenos e médios, em telefone e desktop](images/iconic-template-sizing-info.png)

**Etapa 2: Criar o bloco base**

É possível usar o modelo icônico em blocos primários e secundários. Se estiver usando-o em um bloco secundário, você primeiro precisará criar o bloco secundário ou usar um bloco secundário já fixado. Os blocos primários estão fixos implicitamente e sempre podem receber notificações.

**Etapa 3: Enviar uma notificação para o bloco**

Embora essa etapa possa variar com base na notificação enviada localmente ou por push de servidor, a carga XML que você envia permanece a mesma. Para enviar uma notificação de bloco local, crie um [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) para o bloco (primário ou secundário) e envie uma notificação para o bloco que usa o modelo de bloco icônico conforme visto abaixo. O ideal é que você também inclua associações para tamanhos de bloco largo e grande usando [modelos de bloco adaptável](tiles-and-notifications-adaptive-tiles-schema.md).

Aqui está o código de exemplo da carga XML:

```XML
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

Essa carga XML do modelo de bloco icônico usa um elemento de imagem que aponta para a imagem que você criou na Etapa 1. Agora o bloco está pronto para exibir a notificação próxima do ícone; tudo o que está à esquerda está enviando notificações de selo.

**Etapa 4: Enviar uma notificação de selo para o bloco**

Assim como na Etapa 3, essa etapa pode variar com base na notificação enviada localmente ou por push de servidor, além da carga XML que você envia permanecer a mesma. Para enviar uma notificação de selo local, crie um [**BadgeUpdater**](https://msdn.microsoft.com/library/windows/apps/br208537) para o bloco (primário ou secundário) e envie uma notificação de selo com o valor desejado (ou limpe a notificação).

Aqui está o código de exemplo da carga XML:

```XML
<badge value="2"/>
```

A notificação do bloco será atualizada de acordo.

**Etapa 5: Como reunir tudo**

A imagem a seguir ilustra como as diversas APIs e as cargas estão associadas a cada aspecto do modelo de bloco icônico. Uma [notificação de bloco](https://msdn.microsoft.com/library/windows/apps/hh779724) (que contém os elementos &lt;binding&gt;) é usada para especificar o modelo icônico e o ativo de imagem; uma [notificação de selo](https://msdn.microsoft.com/library/windows/apps/hh779719) especifica o valor numérico; as propriedades do bloco controlam o nome de exibição do bloco, a cor e muito mais.

![apis e cargas associadas ao modelo de bloco icônico](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>Modelo de bloco de fotos


O modelo de bloco de fotos permite exibir uma apresentação de slides de fotos no bloco dinâmico. O modelo é compatível com todos os tamanhos de bloco, inclusive pequeno, e se comporta da mesma maneira em cada tamanho de bloco. O exemplo abaixo mostra cinco quadros de um bloco médio que usa o modelo de fotos. O modelo tem um zoom e uma animação de fading cruzado que percorre as fotos selecionadas e permanece em loop indefinidamente.

![apresentação de slides de imagens usando o modelo de bloco de fotos](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>Como usar o modelo de fotos

Usar o modelo de fotos é fácil caso você tenha instalado a [Biblioteca notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). Embora seja possível usar o XML bruto, é altamente recomendável utilizar a biblioteca de maneira que você não precise se preocupar com a geração de XML válido ou conteúdo de escape XML.

O Windows Phone exibe até nove fotos em uma apresentação de slides; tablet, laptop e desktop exibem até 12.

Para obter informações sobre como enviar a notificação de bloco, consulte o [Artigo sobre envio de notificações](tiles-badges-notifications.md).


```XML
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```CSharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/1.jpg" },
                    new TileBasicImage() { Source = "ms-appdata:///local/Images/2.jpg" },
                    new TileBasicImage() { Source = "http://msn.com/images/3.jpg" }
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <a name="people-tile-template"></a>Modelo de bloco Pessoas


O aplicativo Pessoas no Windows 10 usa um modelo de bloco especial que exibe uma coleção de imagens em círculos que deslizam vertical ou horizontalmente no bloco. Esse modelo de bloco está disponível desde o Windows 10 compilação 10572, e qualquer pessoa pode usá-lo no aplicativo.

O modelo de bloco Pessoas funciona em blocos destes tamanhos:

**Bloco médio** (TileMedium)

![bloco Pessoas médio](images/people-tile-medium.png)

 

**Bloco largo** (TileWide)

![bloco Pessoas largo](images/people-tile-wide.png)

 

**Bloco grande (somente desktop)** (TileLarge)

![bloco grande Pessoas](images/people-tile-large.png)

 

Caso esteja usando a [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), tudo o que você precisa fazer para usar o modelo do bloco Pessoas é criar um novo objeto *TileBindingContentPeople* para o conteúdo *TileBinding*. A classe *TileBindingContentPeople* tem uma propriedade Images em que você adicionar as imagens.

Caso você esteja usando XML bruto, defina *hint-presentation* como "people" e adicione as imagens como filhos do elemento binding.

O exemplo de código C# pressupõe que você esteja usando a [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/).

```CSharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/ProfilePics/1.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/2.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/3.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/4.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/5.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/6.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/7.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/8.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/9.jpg" }
                }
            }
        }
    }
};
```

```XML
<tile>
  <visual>
 
    <binding template="TileMedium" hint-presentation="people">
      <image src="Assets/ProfilePics/1.jpg"/>
      <image src="Assets/ProfilePics/2.jpg"/>
      <image src="Assets/ProfilePics/3.jpg"/>
      <image src="Assets/ProfilePics/4.jpg"/>
      <image src="Assets/ProfilePics/5.jpg"/>
      <image src="Assets/ProfilePics/6.jpg"/>
      <image src="Assets/ProfilePics/7.jpg"/>
      <image src="Assets/ProfilePics/8.jpg"/>
      <image src="Assets/ProfilePics/9.jpg"/>
    </binding>
 
  </visual>
</tile>
```

Para obter a melhor experiência do usuário, recomendamos que você forneça o seguinte número de fotos para cada tamanho de bloco:

-   Bloco médio: nove fotos
-   Bloco largo: 15 fotos
-   Bloco grande: 20 fotos

Ter esse número de fotos possibilita alguns círculos vazios, o que significa que o bloco não será visualmente muito ocupado. Sinta-se à vontade para ajustar o número de fotos a fim de obter a aparência que melhor funcione para você.

Para enviar a notificação, consulte [Escolher um método de entrega de notificação](tiles-and-notifications-choosing-a-notification-delivery-method.md).

## <a name="related-topics"></a>Tópicos relacionados


* [Exemplo de código completo em GitHub](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [Biblioteca de notificações](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Blocos, selos e notificações](tiles-badges-notifications.md)
* [Criar blocos adaptáveis](tiles-and-notifications-create-adaptive-tiles.md)
* [Modelos de blocos adaptáveis: esquema e documentação](tiles-and-notifications-adaptive-tiles-schema.md)
 

 





