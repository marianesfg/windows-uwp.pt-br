---
author: DelfCo
Description: "Coloque recursos de cadeia de caracteres da interface do usuário em arquivos de recurso. Você poderá então referenciar essas cadeias de caracteres no código ou na marcação."
title: "Colocar cadeias de caracteres da interface do usuário em recursos"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: a3e224fc51245a5f91c29da2d745a3740029cda9
ms.sourcegitcommit: 11664964e548a2af30d6e176c515cdbf330934ac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2017
---
# <a name="put-ui-strings-into-resources"></a>Colocar cadeias de caracteres da interface do usuário em recursos
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Coloque recursos de cadeia de caracteres da interface do usuário em arquivos de recurso. Você poderá então referenciar essas cadeias de caracteres no código ou na marcação.

<div class="important-apis" >
<b>APIs Importantes</b><br/>
<ul>
<li>[**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)</li>
<li>[**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)</li>
</ul>
</div>


Este tópico mostra as etapas para adicionar vários recursos de cadeia de caracteres de idiomas ao seu aplicativo Universal do Windows e como testá-lo rapidamente.

## <a name="put-strings-into-resource-files-instead-of-putting-them-directly-in-code-or-markup"></a>Coloque as cadeias de caracteres em arquivos de recurso em vez de colocá-las diretamente no código ou na marcação.


1.  Abra sua solução (ou crie uma nova) no Visual Studio.

2.  Abra package.appxmanifest no Visual Studio, vá até a guia **Aplicativo** e (para este exemplo) defina o idioma Padrão como "en-US". Se houver vários arquivos package.appxmanifest em sua solução, faça isso para cada um deles.
    <br>**Observação**  Isso especifica o idioma padrão do projeto. Os recursos de idioma padrão serão usados se o idioma de preferência do usuário ou os idiomas de exibição não corresponderem aos recursos de idioma fornecidos no aplicativo.
3.  Crie uma pasta para conter os arquivos de recursos.
    1.  No Gerenciador de Soluções, clique com o botão direito no projeto (o projeto Compartilhado se sua solução contiver vários projetos) e selecione **Adicionar** &gt; **Nova Pasta**.
    2.  Dê o nome "Cadeias de caracteres" à nova pasta.
    3.  Se a nova pasta não estiver visível no Gerenciador de Soluções, selecione **Projeto** &gt; **Mostrar Todos os Arquivos** no menu do Microsoft Visual Studio enquanto o projeto ainda estiver selecionado.

4.  Crie uma subpasta e um arquivo de recursos para inglês (Estados Unidos).
    1.  Clique com o botão direito na pasta de cadeias de caracteres e adicione uma nova pasta abaixo dela. Dê a esta pasta o nome "en-US". O arquivo de recursos deve ser colocado em uma pasta que tenha recebido o nome de marca de idioma [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Consulte [Como nomear recursos usando qualificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324) para saber mais sobre o qualificador de idioma e uma lista de marcas de idioma comuns.
    2.  Clique com o botão direito do mouse na pasta en-US e selecione **Adicionar** &gt; **Novo Item…**.
    3.  Selecione "Arquivo de recursos (.resw)".

    4.  Clique em **Adicionar**. Isso adiciona um arquivo de recursos com o nome padrão "Resources.resw". Recomendamos que você use esse nome de arquivo padrão. Os aplicativos podem particionar seus recursos em outros arquivos, mas você deve ter cuidado de fazer a referência a eles corretamente (veja [Como carregar recursos de cadeias de caracteres](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)).
    5.  Se você tiver arquivos .resx apenas com recursos de sequência de caracteres de projetos .NET, selecione **Adicionar** &gt; **Item existente…**, adicione o arquivo .resx e renomeie-o para .resw.
    6.  Abra o arquivo e use o editor para adicionar estes recursos:


        Strings/en-US/Resources.resw ![adicionar recursos, inglês](images/addresource-en-us.png) neste exemplo, "Greeting.Text" e "Farewell" identificam as cadeias de caracteres a serem exibidas. "Greeting.Width" identifica a propriedade Width da cadeia de caracteres "Greeting". Os comentários são um bom lugar para fornecer qualquer instrução especial para tradutores que localizarem as cadeias de caracteres para outros idiomas.

## <a name="associate-controls-to-resources"></a>Associe controles a recursos.

Você precisa associar cada controle que necessita de texto localizado ao arquivo .resw. Para fazer isso, use o atributo **x:Uid** dos elementos XAML da seguinte forma:

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

Para o nome do recurso, você dá o valor do atributo **Uid**, e também especifica qual propriedade obterá a cadeia de caracteres traduzida (neste caso a propriedade Text). Você poderá especificar outra propriedade ou outros valores para culturas diferentes, como Greeting.Width, mas seja cauteloso com essas propriedades relacionadas ao layout. Lembre-se, você deve se esforçar para permitir que os controles sejam dispostos dinamicamente, com base na tela do dispositivo.

Observe que as propriedades anexadas são tratadas de maneira diferente em arquivos resw, como AutomationProperties.Name. Você precisa escrever explicitamente o namespace da seguinte forma:

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="add-string-resource-identifiers-to-code-and-markup"></a>Adicione identificadores de recurso ao código e à marcação.

No seu código, você pode fazer referência dinamicamente às cadeias de caracteres:

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```


## <a name="add-folders-and-resource-files-for-two-additional-languages"></a>Adicione pastas e arquivos de recursos para dois idiomas adicionais.


1.  Adicione outra pasta sob a pasta de Cadeias de caracteres para o idioma alemão. Dê à pasta o nome "de-DE", que representa Deutsch (alemão).
2.  Crie outro arquivo de recursos na pasta de-DE e adicione o seguinte:

    strings/de-DE/Resources.resw

    ![adicionar recursos, alemão](images/addresource-de-de.png)


3.  Crie mais uma pasta, denominada "fr-FR", para francês (França). Crie um novo arquivo de recursos e adicione o seguinte:

    strings/fr-FR/Resources.resw
    
    ![adicionar recurso, francês](images/addresource-fr-fr.png)

## <a name="build-and-run-the-app"></a>Compile e execute o aplicativo.


Teste o aplicativo no idioma de exibição padrão.

1.  Pressione F5 para compilar e executar o aplicativo.
2.  Os itens greeting e farewell são exibidos no idioma de preferência do usuário.
3.  Saia do aplicativo.

Teste o aplicativo nos outros idiomas.

1.  Ative as **Configurações** em seu dispositivo.
2.  Selecione **Hora e Idioma**.
3.  Selecione **Região e Idioma** (ou em um telefone ou emulador de telefone, **Idioma**).
4.  Observe que o idioma exibido quando você executa o aplicativo corresponde ao primeiro da lista (inglês, alemão ou francês). Se o seu idioma principal não for nenhum desses três, o aplicativo utilizará o próximo da lista de opções compatíveis.
5.  Caso nenhum desses três idiomas estejam presentes em seu computador, adicione os que estiverem faltando. Para isto, clique em **Adicionar um idioma** e adicione-os à lista.
6.  Para testar o aplicativo em outro idioma, selecione o idioma na lista e clique em **Definir como padrão** (ou em um telefone ou emulador de telefone, toque e segure o idioma na lista e, em seguida, toque em **Mover para cima** até que ele esteja na parte superior). Em seguida, execute o aplicativo.

## <a name="related-topics"></a>Tópicos relacionados


* [Como nomear recursos usando qualificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [Como carregar recursos de cadeias de caracteres](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [A marca do idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 



