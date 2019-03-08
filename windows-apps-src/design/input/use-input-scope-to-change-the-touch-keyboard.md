---
Description: Para ajudar os usuários a inserir dados usando o teclado virtual ou SIP (Soft Input Panel), você pode configurar o escopo de entrada do controle de texto para corresponder ao tipo de dado que se espera que o usuário insira.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Usar o escopo de entrada para alterar o teclado virtual
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: teclado, acessibilidade, navegação, foco, texto, entrada e interação do usuário
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: 1350c6e0eae057386fb721a358f71acb19c4efc1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591761"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Usar o escopo de entrada para alterar o teclado virtual

Para ajudar os usuários a inserir dados usando o teclado virtual ou SIP (Soft Input Panel), você pode configurar o escopo de entrada do controle de texto para corresponder ao tipo de dado que se espera que o usuário insira.

### <a name="important-apis"></a>APIs Importantes
- [InputScope](https://msdn.microsoft.com/library/windows/apps/hh702632)
- [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/hh702028)


O teclado virtual pode ser usado para entrada de texto, quando o aplicativo é executado em um dispositivo com tela sensível ao toque. O teclado virtual é invocado quando o usuário toca em um campo de entrada editável, como um **[TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)** ou um **[RichEditBox](https://msdn.microsoft.com/library/windows/apps/br227548)**. Você pode tornar a entrada de dados muito mais rápida e fácil para os usuários em seu aplicativo definindo o *escopo de entrada* do controle de texto para corresponder ao tipo de dados que o usuário deve inserir. O escopo de entrada oferece uma dica para o sistema sobre o tipo de entrada de texto esperado pelo controle, para que o sistema possa fornecer um layout de teclado virtual especializado para o tipo de entrada.

Por exemplo, se uma caixa de texto for usada somente para a inserção de um PIN de 4 dígitos, defina a propriedade [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) como **Number**. Isso informa o sistema para mostrar o layout do teclado numérico, facilitando a inserção do PIN.

> [!IMPORTANT]
> - Essas informações se aplicam somente ao SIP. Elas não se aplicam a teclados de hardware nem ao Teclado Virtual disponível nas opções de Facilidade de Acesso do Windows.
> - O escopo de entrada não faz com que qualquer validação de entrada seja executada, e não impede que o usuário forneça qualquer entrada por meio de um teclado de hardware ou de outro dispositivo de entrada. Você ainda é o responsável pela validação da entrada em seu código, conforme necessário.

## <a name="changing-the-input-scope-of-a-text-control"></a>Alterando o escopo de entrada de um controle de texto

Os escopos de entrada disponíveis para seu aplicativo são membros da enumeração **[InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/hh702028)**. Você pode definir a propriedade **InputScope** de uma **[TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)** ou **[RichEditBox](https://msdn.microsoft.com/library/windows/apps/br227548)** para um desses valores.

> [!IMPORTANT]
> O **[InputScope](https://msdn.microsoft.com/library/windows/apps/dn996570)** propriedade **[PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)** dá suporte apenas a **senha** e  **NumericPin** valores. Qualquer outro valor é ignorado.

Aqui, você altera o escopo de entrada de várias caixas de texto para corresponder aos dados esperados para cada caixa de texto.

**Para alterar o escopo de entrada em XAML**

1.  No arquivo XAML para sua página, localize a marca para o controle de texto que você deseja alterar.
2.  Adicione o atributo [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) ao sinalizador e especifique o valor [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) que corresponde à entrada esperada.

    Aqui estão algumas caixas de texto que podem aparecer em um formulário comum de contato com o cliente. Com o [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) definido, um teclado de toque com um layout adequado para os dados aparece para cada caixa de texto.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Para alterar o escopo de entrada no código**

1.  No arquivo XAML para sua página, localize a marca para o controle de texto que você deseja alterar. Se não estiver definido, defina o [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788) para poder referenciar o controle no seu código.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Instancie um novo objeto [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025).

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Crie uma instância de um novo objeto [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027).
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Defina a propriedade [**NameValue**](https://msdn.microsoft.com/library/windows/apps/hh702032) do objeto [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) como um valor da enumeração [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028).

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Adicione o objeto [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) à coleção [**Names**](https://msdn.microsoft.com/library/windows/apps/hh702034) do objeto [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025).

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Defina o objeto [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025) como o valor da propriedade [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) do controle de texto.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

Aqui está o código completo.

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

As mesmas etapas podem ser condensadas neste código resumido.

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>Previsão de texto, verificação ortográfica e correção automática

Os controles [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) e [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) têm várias propriedades que influenciam o comportamento do SIP. Para fornecer a melhor experiência para seus usuários, é importante entender como essas propriedades afetam a entrada de texto usando toque.

-   [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688)— quando a verificação ortográfica está habilitada para um controle de texto, o controle interage com o mecanismo de verificação ortográfica do sistema para marcar as palavras que não são reconhecidas. Você pode tocar em uma palavra para ver uma lista de correções sugeridas. A verificação ortográfica é habilitada por padrão.

    Para o escopo de entrada **Default**, essa propriedade também habilita o uso automático de maiúsculas da primeira palavra em uma frase e a correção automática das palavras conforme você digita. Esses recursos de correção automática podem estar desabilitados em outros escopos de entrada. Para saber mais, consulte as tabelas mais adiante neste tópico.

-   [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690)— quando a previsão de texto está habilitada para um controle de texto, o sistema mostra uma lista de palavras que você pode estar começando a digitar. Você pode selecionar na lista para não precisar digitar a palavra inteira. A previsão de texto é habilitada por padrão.

    A previsão de texto poderá estar desabilitada se o escopo de entrada for diferente de **Default**, mesmo se a propriedade [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) for **true**. Para saber mais, consulte as tabelas mais adiante neste tópico.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://msdn.microsoft.com/library/windows/apps/dn299273)— quando essa propriedade for **verdadeiro**, impede que o sistema mostrando o SIP quando o foco é definido por meio de programação em um controle de texto. Em vez disso, o teclado só será mostrado quando o usuário interagir com o controle.

## <a name="touch-keyboard-index-for-windows"></a>Índice de teclado de toque para Windows

Essas tabelas mostram os layouts de Windows reversível entrada SIP (painel virtual) para valores de escopo de entrada comuns. O efeito do escopo de entrada sobre os recursos habilitados pelas propriedades **IsSpellCheckEnabled** e **IsTextPredictionEnabled** é listado para cada escopo de entrada. Esta não é uma lista abrangente de escopos de entrada disponíveis.

> [!Tip] 
> Você pode alternar a maioria dos teclados de toque entre um layout alfabético e um layout de símbolos e números, pressionando as **& 123** para alterar para o layout de símbolos e números e pressione a **abcd** chave para Altere para o layout alfabético.

### <a name="default"></a>Padrão

`<TextBox InputScope="Default"/>`

Teclado de toque do Windows padrão.

![Teclado virtual padrão do Windows](images/input-scopes/default.png)
- Verificação ortográfica: habilitada se **IsSpellCheckEnabled** = **true**, desabilitada se **IsSpellCheckEnabled** = **false**
- Correção automática: habilitada se **IsSpellCheckEnabled** = **true**, desabilitada se **IsSpellCheckEnabled** = **false**
- Uso automático de maiúsculas: habilitado se **IsSpellCheckEnabled** = **true**, desabilitado se **IsSpellCheckEnabled** = **false**
- Previsão de texto: habilitada se **IsTextPredictionEnabled** = **true**, desabilitada se **IsTextPredictionEnabled** = **false**

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

O layout de teclado padrão de números e símbolos.

![Teclado virtual do Windows para moeda](images/input-scopes/currencyamountandsymbol.png)

- Inclui as chaves de esquerda/direita da página para mostrar mais símbolos
- Verificação ortográfica: ativada por padrão, pode ser desabilitada
- Correção automática: ativada por padrão, pode ser desabilitada
- Uso automático de maiúsculas: sempre desabilitada
- Previsão de texto: ativada por padrão, pode ser desabilitada
 
### <a name="url"></a>Url

`<TextBox InputScope="Url"/>`

![Teclado virtual do Windows para URLs](images/input-scopes/url.png)

- Inclui as teclas **.com** e ![tecla ir](images/input-scopes/kbdgokey.png) (Ir). Pressione e segure a **.com** tecla para exibir opções adicionais (**. org**, **.net**e sufixos de específico da região)
- Inclui o **:**, **-**, e **/** chaves
- Verificação ortográfica: desativada por padrão, pode ser habilitada
- Correção automática: desativada por padrão, pode ser habilitada
- Capitalização automática: desativada por padrão, pode ser habilitada
- Previsão de texto: desativada por padrão, pode ser habilitada


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Teclado virtual do Windows para endereços de email](images/input-scopes/emailsmtpaddress.png)
- Inclui as teclas **@** e **.com**. Pressione e segure a **.com** tecla para exibir opções adicionais (**. org**, **.net**e sufixos de específico da região)
- Inclui o **_** e **-** chaves
- Verificação ortográfica: desativada por padrão, pode ser habilitada
- Correção automática: desativada por padrão, pode ser habilitada
- Capitalização automática: desativada por padrão, pode ser habilitada
- Previsão de texto: desativada por padrão, pode ser habilitada


### <a name="number"></a>Número

`<TextBox InputScope="Number"/>`

![Teclado virtual do Windows para números](images/input-scopes/number.png)
- Verificação ortográfica: ativada por padrão, pode ser desabilitada
- Correção automática: ativada por padrão, pode ser desabilitada
- Uso automático de maiúsculas: sempre desabilitada
- Previsão de texto: ativada por padrão, pode ser desabilitada

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![Teclado virtual do Windows para números de telefone](images/input-scopes/telephonenumber.png)
- Verificação ortográfica: ativada por padrão, pode ser desabilitada
- Correção automática: ativada por padrão, pode ser desabilitada
- Uso automático de maiúsculas: sempre desabilitada
- Previsão de texto: ativada por padrão, pode ser desabilitada

### <a name="search"></a>Pesquisa

`<TextBox InputScope="Search"/>`

![Teclado virtual do Windows para pesquisa](images/input-scopes/search.png)
- Inclui o **pesquisa** chave em vez da **Enter** chave
- Verificação ortográfica: ativada por padrão, pode ser desabilitada
- Correção automática: ativada por padrão, pode ser desabilitada
- Uso automático de maiúsculas: sempre desabilitada
- Previsão de texto: ativada por padrão, pode ser desabilitada

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Teclado de toque do Windows para a pesquisa incremental](images/input-scopes/searchincremental.png)
- Mesmo layout **padrão**
- Verificação ortográfica: desativada por padrão, pode ser habilitada
- Correção automática: sempre desabilitada
- Uso automático de maiúsculas: sempre desabilitada
- Previsão de texto: sempre desabilitada

### <a name="formula"></a>Fórmula

`<TextBox InputScope="Formula"/>`

![Windows teclado de toque para fórmula](images/input-scopes/formula.png)
- Inclui o **=** chave
- Também inclui o **%**, **$**, e **+** chaves
- Verificação ortográfica: ativada por padrão, pode ser desabilitada
- Correção automática: ativada por padrão, pode ser desabilitada
- Uso automático de maiúsculas: sempre desabilitada
- Previsão de texto: ativada por padrão, pode ser desabilitada

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![Teclado virtual padrão do Windows](images/input-scopes/default.png)
- Mesmo layout **padrão**
- Verificação ortográfica: ativada por padrão, pode ser desabilitada
- Correção automática: ativada por padrão, pode ser desabilitada
- Capitalização automática: ativada por padrão, pode ser desabilitada
- Previsão de texto: ativada por padrão, pode ser desabilitada

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Teclado virtual padrão do Windows](images/input-scopes/default.png)
- Mesmo layout **padrão**
- Verificação ortográfica: desativada por padrão, pode ser habilitada
- Correção automática: desativada por padrão, pode ser habilitada
- Capitalização automática: desativada por padrão, pode ser habilitada (a primeira letra de cada palavra é maiuscula)
- Previsão de texto: desativada por padrão, pode ser habilitada
