---
Description: Se você permitir que os clientes usem seu aplicativo gratuitamente durante um período de avaliação, incentive-os a atualizar para a versão completa do aplicativo excluindo ou limitando alguns recursos durante o período de avaliação.
title: Excluir ou limitar recursos em uma versão de avaliação
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: avaliação gratuita
keywords: período de avaliação gratuita
keywords: código de exemplo de avaliação gratuita
keywords: código de exemplo de avaliação gratuita
---

# Excluir ou limitar recursos em uma versão de avaliação


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Se você permitir que os clientes usem seu aplicativo gratuitamente durante um período de avaliação, incentive-os a atualizar para a versão completa do aplicativo excluindo ou limitando alguns recursos durante o período de avaliação. Determine quais recursos devem ser limitados antes de começar a codificação, depois certifique-se de que o seu aplicativo permita que eles funcionem após a compra de uma licença completa. Você também pode habilitar recursos, como faixas ou marcas-d'água que são mostrados apenas durante a avaliação, antes de o cliente comprar o aplicativo.

Vamos examinar como adicionar isso a seu aplicativo.

## Pré-requisitos

Um aplicativo do Windows no qual devem ser adicionados os recursos que os clientes podem comprar.

## Etapa 1: Escolha os recursos que você deseja habilitar ou desabilitar durante o período de avaliação.

O estado da licença atual de seu aplicativo é armazenado como propriedades da classe [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157). Geralmente, você coloca as funções que dependem do estado da licença em um bloco condicional, conforme descrito na próxima etapa. Ao considerar esses recursos, verifique se você pode implementá-los de maneira que funcionem em todos os estados de licença.

Decida também como você gostaria de habilitar as alterações na licença do aplicativo durante sua execução. O aplicativo de avaliação pode conter todos os recursos, mas ter faixas de anúncios no aplicativo que a versão paga não tem. O aplicativo de avaliação também pode desabilitar determinados recursos ou exibir mensagens regulares solicitando a compra.

Analise sobre o tipo de aplicativo sendo criado e uma boa estratégia de avaliação ou expiração para ele. Para uma versão de avaliação de um jogo, uma boa estratégia é limitar a quantidade de conteúdo do jogo que um usuário pode jogar. Para uma versão de avaliação de um utilitário, você pode considerar definir uma data de expiração ou limitar os recursos que um possível comprador pode usar.

Nos aplicativos não destinados a jogos, a configuração de uma data de expiração funciona bem, pois os usuários podem desenvolver um bom entendimento do aplicativo como um todo. Veja aqui alguns cenários comuns de expiração e as opções para lidar com eles.

-   **A licença de avaliação expira enquanto o aplicativo está em execução**

    Se a avaliação expirar enquanto o aplicativo estiver em execução, o aplicativo poderá:

    -   Não fazer nada.
    -   Exibir uma mensagem para o cliente.
    -   Fechar.
    -   Solicitar que o cliente faça a compra.

    A prática recomendada é exibir uma mensagem com uma solicitação de compra do aplicativo e, se o cliente comprá-lo, continuar com todos os recursos habilitados. Se o usuário não se decidir pela compra, feche o aplicativo ou lembre periodicamente o usuário para comprá-lo.

-   **A licença de avaliação expira antes de o aplicativo ser iniciado**

    Se a avaliação expirar antes de o usuário iniciar o aplicativo, ele não será iniciado. Em vez disso, os usuários veem uma caixa de diálogo que lhes dá a opção de comprar o aplicativo na loja.

-   **O cliente compra o aplicativo enquanto ele está em execução**

    Se o cliente comprar o aplicativo enquanto ele estiver em execução, aqui estão algumas ações que o aplicativo poderá executar.

    -   Não fazer nada e manter o cliente no modo de avaliação enquanto o aplicativo não for reiniciado.
    -   Agradeça-os pela compra ou exiba uma mensagem.
    -   Habilitar silenciosamente os recursos disponibilizados pela licença completa (ou desabilitar os avisos de somente avaliação).

Se quiser detectar a mudança de licença e tomar alguma providência no seu aplicativo, adicione um manipulador de eventos para isso, conforme descrito na próxima etapa.
## Etapa 2: Inicializar as informações de licença

Quando seu aplicativo estiver sendo inicializado, obtenha o objeto [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) do aplicativo, conforme mostrado nesta amostra. Pressupomos que a **licenseInformation** é uma variável global ou um campo global do tipo **LicenseInformation**.

Inicialize o [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) ou [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) para acessar as informações de licença do aplicativo.

```CSharp
void initializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;
      
}
```

Adicione um manipulador de eventos para receber as notificações quando a licença for alterada durante a execução do aplicativo. A licença do aplicativo pode ser alterada quando o período de avaliação expira ou o cliente compra o aplicativo por meio de uma Loja, por exemplo.

```CSharp
void InitializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // Register for the license state change event.
     licenseInformation.LicenseChanged += new LicenseChangedEventHandler(licenseChangedEventHandler);

}

// ...

void licenseChangedEventHandler()
{
    ReloadLicense(); // code is in next steps
}
```

## Etapa 3: Codificar recursos em blocos condicionais

Quando o evento de alteração da licença for gerado, o aplicativo deverá chamar a API de Licença para determinar se o status de avaliação foi alterado. O código nesta etapa mostra como estruturar o manipulador desse evento. Nesse ponto, se um usuário comprou o aplicativo, é uma prática recomendada fornecer comentários para o usuário informando que o status de licença foi alterado. Você pode precisar solicitar que o usuário reinicie o aplicativo, caso este tenha sido codificado assim. Mas faça essa transição de maneira mais transparente e suave possível.

Este exemplo mostra como avaliar o status de licença do aplicativo para que você possa habilitar ou desabilitar um recurso do aplicativo de forma adequada.

```CSharp
void ReloadLicense()
{
    if (licenseInformation.IsActive)
    {
         if (licenseInformation.IsTrial)
         {
             // Show the features that are available during trial only.
         }
         else
         {
             // Show the features that are available only with a full license.
         }
     }
     else
     {
         // A license is inactive only when there' s an error.
     }
}
```

## Etapa 4: Obter uma data de expiração da avaliação do aplicativo

Inclua o código para determinar a data de expiração do aplicativo.

O código neste exemplo define uma função para obter a data de expiração da licença de avaliação do aplicativo. Se a licença ainda for válida, exiba a data de expiração com o número de dias que restam até a expiração da avaliação.

```CSharp
void DisplayTrialVersionExpirationTime()
{
    if (licenseInformation.IsActive)
    {
        if (licenseInformation.IsTrial)
        {
            var longDateFormat = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longdate");
                                                
            // Display the expiration date using the DateTimeFormatter. 
            // For example, longDateFormat.Format(licenseInformation.ExpirationDate)

            var daysRemaining = (licenseInformation.ExpirationDate - DateTime.Now).Days;

            // Let the user know the number of days remaining before the feature expires
        }
        else
        {
            // ...
        }
    }
    else
    {
       // ...
    }
}
```

## Etapa 5: Testar os recursos usando chamadas simuladas para a API de Licença

Agora, teste o aplicativo usando chamadas simuladas para o servidor de licenças No JavaScript, no C#, no Visual Basic ou no Visual C++, substitua as referências ao [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) por [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) no código de inicialização do aplicativo.

[**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) obtém informações específicas do teste de um arquivo XML denominado "WindowsStoreProxy.xml", localizado em %userprofile%\\AppData\\local\\packages\\&lt;nome do pacote&gt;\LocalState\\Microsoft\\Windows Store\\ApiData. Se esse caminho e esse arquivo não existirem, você deverá criá-los ou fornecê-los durante a instalação ou no tempo de execução. Se você tentar acessar a propriedade [**CurrentAppSimulator.LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/hh779768) sem o WindowsStoreProxy.xml presente nesse local específico, será apresentado um erro.

Este exemplo ilustra como você pode adicionar código ao aplicativo para testá-lo sob os estados diferentes de licença.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

Você pode editar o WindowsStoreProxy.xml para alterar as datas de expiração simuladas do aplicativo e seus recursos. Teste todas as configurações possíveis de expiração e licença para verificar se tudo funciona conforme o esperado.

## Etapa 6: Substituir os métodos da API de Licença simulada pela API real

Depois de testar seu aplicativo com o servidor de licenças simulado e antes de enviá-lo para uma Loja para certificação, substitua [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) por [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765), conforme mostrado no código de exemplo a seguir.

**Importante**  O aplicativo deverá usar o objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) quando você enviá-lo para uma Loja, caso contrário, haverá falha na certificação.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    //   licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Etapa 7: Descrever para os clientes como funciona a versão de avaliação gratuita

Lembre-se de explicar como o aplicativo se comportará durante e após o período de avaliação gratuita, assim, os clientes não serão surpreendidos pelo comportamento do aplicativo.

Para saber mais sobre a descrição de seu aplicativo, consulte [Criar descrições de aplicativos](https://msdn.microsoft.com/library/windows/apps/mt148529).

## Tópicos relacionados

* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [Definir a disponibilidade e o preço do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 






<!--HONumber=Mar16_HO1-->


