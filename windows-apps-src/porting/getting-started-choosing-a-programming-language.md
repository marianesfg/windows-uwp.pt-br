---
title: Escolhendo uma linguagem de programação
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Escolhendo uma linguagem de programação
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 48cea8c1a3a3c35a53a3376c48e3192c94caf709
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620771"
---
# <a name="getting-started-choosing-a-programming-language"></a>Guia de Introdução: Escolhendo uma linguagem de programação


## <a name="choosing-a-programming-language"></a>Escolhendo uma linguagem de programação

Antes de seguirmos adiante, você deve conhecer as linguagens de programação que poderá escolher quando for desenvolver aplicativos da Plataforma Universal do Windows (UWP). Embora as orientações passo a passo deste artigo usem C#, é possível desenvolver aplicativos UWP usando uma ou mais linguagens de programação (veja [Linguagens, ferramentas e estruturas](https://msdn.microsoft.com/library/windows/apps/dn465799)).

Você pode desenvolver usando C++, C#, Microsoft Visual Basic e JavaScript. O JavaScript usa uma marcação HTML5 para o layout da interface do usuário, e as outras linguagens usam uma linguagem de marcação chamada *Linguagem (XAML, Extensible Application Markup Language)* para descrever sua interface de usuário.

Apesar de nosso foco ser a linguagem C# neste artigo, as outras linguagens oferecem benefícios exclusivos, que convém explorar. Por exemplo, se o desempenho do aplicativo for uma preocupação prioritária, principalmente para gráficos intensos, então C++ pode ser a escolha certa. A versão Microsoft .NET do Visual Basic é excelente para desenvolvedores de aplicativos do Visual Basic. JavaScript com HTML5 é ótimo para quem vem de uma experiência de desenvolvimento da Web. Para obter mais informações, consulte um destes artigos:

-   [Criar seu primeiro aplicativo UWP usando C++](../get-started/create-a-basic-windows-10-app-in-cpp.md)
-   [Criar seu primeiro aplicativo de UWP usando C# ou o Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Criar seu primeiro aplicativo UWP usando JavaScript](../get-started/create-a-hello-world-app-js-uwp.md)

**Observação**  para aplicativos que usam elementos gráficos 3D, os padrões de OpenGL e OpenGL ES não estão disponíveis nativamente para aplicativos UWP. Caso prefira não reescrever seu código OpenGL ES no Microsoft DirectX, talvez se interesse em aprender sobre o **Angle**. O Angle é um projeto em andamento criado para converter OpenGL para DirectX traduzindo as chamadas à API OpenGL em chamadas à API DirectX. Para saber mais, consulte o seguinte:
-   [ângulo](https://code.google.com/p/angleproject/)
-   [Criar seu primeiro aplicativo UWP usando o DirectX](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [Exemplos de aplicativos UWP que usam DirectX](https://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [Onde está o SDK do DirectX?](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## <a name="giving-c-a-go"></a>Dando uma chance para C#

Como desenvolvedor do iOS, você está habituado ao Objective-C e Swift. A linguagem de programação da Microsoft mais próxima é o C#. Para a maioria dos desenvolvedores e aplicativos, achamos que o C# é a linguagem mais fácil e rápida para aprender a usar, por isso as informações e tutoriais deste artigo se concentram nessa linguagem. Para saber mais sobre C#, consulte:

-   [Criar seu primeiro aplicativo de UWP usando C# ou o Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Exemplos de aplicativos UWP que usamC#](https://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#](https://go.microsoft.com/fwlink/p/?LinkId=263450)

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

[Guia de Introdução: Conheça o Visual Studio](getting-started-getting-around-in-visual-studio.md)
