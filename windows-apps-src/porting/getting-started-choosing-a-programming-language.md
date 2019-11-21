---
title: Escolhendo uma linguagem de programação
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Escolhendo uma linguagem de programação
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 037c079881dbb2634b31cc0cf5b9248115dbceef
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259160"
---
# <a name="getting-started-choosing-a-programming-language"></a>Introdução: escolhendo uma linguagem de programação


## <a name="choosing-a-programming-language"></a>Escolhendo uma linguagem de programação

Antes de seguirmos adiante, você deve conhecer as linguagens de programação que poderá escolher quando for desenvolver aplicativos da Plataforma Universal do Windows (UWP). Embora as orientações passo a passo deste artigo usem C#, é possível desenvolver aplicativos UWP usando uma ou mais linguagens de programação (veja [Linguagens, ferramentas e estruturas](https://docs.microsoft.com/previous-versions/windows/apps/dn465799(v=win.10))).

Você pode desenvolver usando C++, C#, Microsoft Visual Basic e JavaScript. O JavaScript usa uma marcação HTML5 para o layout da interface do usuário, e as outras linguagens usam uma linguagem de marcação chamada *Linguagem (XAML, Extensible Application Markup Language)* para descrever sua interface de usuário.

Apesar de nosso foco ser a linguagem C# neste artigo, as outras linguagens oferecem benefícios exclusivos, que convém explorar. Por exemplo, se o desempenho do aplicativo for uma preocupação prioritária, principalmente para gráficos intensos, então C++ pode ser a escolha certa. A versão Microsoft .NET do Visual Basic é excelente para desenvolvedores de aplicativos do Visual Basic. JavaScript com HTML5 é ótimo para quem vem de uma experiência de desenvolvimento da Web. Para obter mais informações, consulte um destes artigos:

-   [Crie seu primeiro aplicativo UWP usandoC++](../get-started/create-a-basic-windows-10-app-in-cpp.md)
-   [Crie seu primeiro aplicativo UWP usando C# ou Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Crie seu primeiro aplicativo UWP usando JavaScript](../get-started/create-a-hello-world-app-js-uwp.md)

**Observação**  para aplicativos que usam gráficos 3D, os padrões OpenGL e OpenGL ES não estão disponíveis nativamente para aplicativos UWP. Caso prefira não reescrever seu código OpenGL ES no Microsoft DirectX, talvez se interesse em aprender sobre o **Angle**. O Angle é um projeto em andamento criado para converter OpenGL para DirectX traduzindo as chamadas à API OpenGL em chamadas à API DirectX. Para saber mais, consulte o seguinte:
-   [Firmeza](https://bugs.chromium.org/p/angleproject/)
-   [Criar seu primeiro aplicativo UWP usando DirectX](https://docs.microsoft.com/previous-versions/windows/apps/br229580(v=win.10))
-   [Exemplos de aplicativos UWP que usam DirectX](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=Technology&f%5B0%5D.Value=DirectX)
-   [Onde está o SDK do DirectX?](https://docs.microsoft.com/windows/desktop/directx-sdk--august-2009-)

## <a name="giving-c-a-go"></a>Dando uma chance para C#

Como desenvolvedor do iOS, você está habituado ao Objective-C e Swift. A linguagem de programação da Microsoft mais próxima é o C#. Para a maioria dos desenvolvedores e aplicativos, achamos que o C# é a linguagem mais fácil e rápida para aprender a usar, por isso as informações e tutoriais deste artigo se concentram nessa linguagem. Para saber mais sobre C#, consulte:

-   [Crie seu primeiro aplicativo UWP usando C# ou Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Exemplos de aplicativos UWP que usamC#](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=ProgrammingLanguage&f%5B0%5D.Value=C%23&f%5B0%5D.Text=C%23)
-   [VisualizarC#](https://msdn.microsoft.com/library/kx37x362.aspx)

A seguir, há uma classe escrita em Objective-C e C#. A versão Objective-C é exibida primeiro, seguida pela versão C#.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

Agora para a versão C#. Você verá que, como Swift, o cabeçalho e a implementação não estão em arquivos separados.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# é uma linguagem fácil de usar e vem com as muitas classes de suporte e estruturas que compõem o .NET. Você nunca gravará seu código sem um colchete visível.

## <a name="next-step"></a>Próximas etapas

[Introdução: introdução ao Visual Studio](getting-started-getting-around-in-visual-studio.md)
