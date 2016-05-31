---
author: mcleanbyron Description: Seja seu aplicativo gratuito ou não, você pode vender conteúdo, outros aplicativos ou uma nova funcionalidade do aplicativo (como o desbloqueio do próximo nível de um jogo) no próprio aplicativo. Veja a seguir como habilitar esses produtos no seu aplicativo.
title: Habilitar compras de produtos no aplicativo ms.assetid: palavras-chave D158E9EB-1907-4173-9889-66507957BD6B: palavras-chave de oferta no aplicativo: palavras-chave de compra no aplicativo: palavras-chave de produto no aplicativo: como dar suporte a palavras-chave no aplicativo: palavras-chave de exemplo de código de compra no aplicativo: exemplo de código de oferta no aplicativo
---

# Habilitar compras de produtos no aplicativo

\[ Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Seja seu aplicativo gratuito ou não, você pode vender conteúdo, outros aplicativos ou uma nova funcionalidade do aplicativo (como o desbloqueio do próximo nível de um jogo) no próprio aplicativo. Veja a seguir como habilitar esses produtos no seu aplicativo.

**Observação**  Produtos no aplicativo não podem ser oferecidos em uma versão de avaliação do aplicativo. Os clientes que usam uma versão de avaliação do aplicativo só poderão comprar um produto no aplicativo se comprarem a versão completa do seu aplicativo.

## Pré-requisitos

-   Um aplicativo do Windows no qual devem ser adicionados os recursos que os clientes podem comprar.
-   Ao codificar e testar novos produtos no aplicativo pela primeira vez, você deve usar o objeto [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) em vez do objeto [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765). Dessa forma, é possível verificar a lógica do licenciamento usando chamadas simuladas ao servidor de licenças em vez de chamar o servidor ativo. Para fazer isso, você precisa personalizar o arquivo chamado "WindowsStoreProxy.xml" em %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData. O simulador do Microsoft Visual Studio cria esse arquivo quando você executa seu aplicativo pela primeira vez, mas também é possível carregar um arquivo personalizado no tempo de execução. Para saber mais, consulte [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).
-   Este tópico também faz referência a exemplos de código fornecidos no [Exemplo da Loja](http://go.microsoft.com/fwlink/p/?LinkID=627610). Essa amostra é uma ótima maneira de obter experiência prática com as diferentes opções de monetização fornecidas para os aplicativos UWP (Plataforma Universal do Windows).

## Etapa 1: Inicie as informações de licença do aplicativo

Quando seu aplicativo estiver inicializando, obtenha o objeto [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) para seu aplicativo inicializando o [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) ou [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) para habilitar compras de um produto no aplicativo.

```CSharp
void AppInit()
{
    // some app initialization functions 

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## Etapa 2: Adicione as ofertas de produto no aplicativo ao seu aplicativo

Para cada recurso a ser disponibilizado por meio de uma transação de produto no aplicativo, crie uma oferta e adicione-a ao aplicativo.

**Importante**  Você deve adicionar todos os produtos no aplicativo que deseja apresentar para seus clientes antes de enviá-lo para a Loja. Para adicionar novos produtos no aplicativo depois, você deve atualizar o aplicativo e reenviar uma nova versão.

1.  **Crie um token de oferta no aplicativo**

    Você pode identificar cada produto no seu aplicativo por um token. Esse token é uma cadeia de caracteres que você define e usa no aplicativo e na Loja para identificar um produto no aplicativo específico. Dê (ao aplicativo) um nome exclusivo e significativo, para poder identificar o recurso correto que ele representa durante a codificação. Este são alguns exemplos de nomes:

    -   "SpaceMissionLevel4"
    -   "ContosoCloudSave"
    -   "RainbowThemePack".

2.  **Codifique o recurso em um bloco de condições**

    Coloque o código de cada recurso associado a um produto no aplicativo em um bloco de condições que testa se o cliente tem uma licença para usar esse recurso.

    Veja a seguir um exemplo que mostra como é possível codificar um recurso de produto chamado **featureName** em um bloco condicional específico da licença. A cadeia de caracteres, **featureName**, é o token que identifica esse produto de forma exclusiva no aplicativo e também é usada para identificá-lo na Loja.

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive) 
        {
            // the customer can access this feature
        } 
        else
        {
            // the customer can' t access this feature
        }
    ```

3.  **Adicione a interface do usuário de compra para este recurso**

    Seu aplicativo também deve permitir que os clientes comprem o produto ou o recurso proposto para produto no aplicativo. O jeito de comprá-los é diferente da maneira como os clientes compraram o aplicativo completo na Loja.

    Veja aqui como testar se o cliente já possui um produto no aplicativo e, se não tiver, se ele pode visualizar a caixa de diálogo para fazer a compra. Substitua o comentário "mostrar a caixa de diálogo de compra" pelo código personalizado da caixa de diálogo de compra (como uma página com um botão "Compre este aplicativo!" ).

    ```    CSharp
    void BuyFeature1() {
            if (!licenseInformation.ProductLicenses["featureName"].IsActive)
            {
                try
                    {
                    // The customer doesn't own this feature, so 
                    // show the purchase dialog.
                                    
                    await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);
                    //Check the license state to determine if the in-app purchase was successful.
                }
                catch (Exception)
                {
                    // The in-app purchase was not completed because 
                    // an error occurred.
                }
            } 
            else
            {
                // The customer already owns this feature.
            }
        }
    ```

## Etapa 3: Mude o código de teste para as chamadas finais

Esta etapa é fácil: basta mudar todas as referências de [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) para [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) no código do aplicativo. Não é mais preciso fornecer o arquivo WindowsStoreProxy.xml, então, remova-o do caminho do aplicativo (embora você possa salvá-lo para referência ao configurar a oferta no aplicativo, na próxima etapa).

## Etapa 4: Configurar o produto no aplicativo na Loja

No painel do Centro de Desenvolvimento, defina a ID do produto, o tipo, o preço e outras propriedades para seu produto no aplicativo. Lembre-se de configurá-lo com a mesma configuração que você definiu no WindowsStoreProxy.xml durante o teste. Para obter mais informações, consulte [Envios de IAP](https://msdn.microsoft.com/library/windows/apps/mt148551).

## Comentários

Se você tiver interesse em fornecer aos seus clientes opções de compra de produtos no aplicativo (itens que poderão ser comprados, usados e comprados novamente se desejado), vá para o tópico [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md).

Se você precisar usar recibos para verificar se o usuário fez uma compra no aplicativo, confira [Usar recibos para verificar compras de produto](use-receipts-to-verify-product-purchases.md).

## Tópicos relacionados


* [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)
* [Gerenciar um catálogo abrangente de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md)
* [Usar recibos para verificar compras de produtos](use-receipts-to-verify-product-purchases.md)
* [Exemplo da Loja (demonstra avaliações e compras no aplicativo)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
 

 






<!--HONumber=May16_HO2-->


