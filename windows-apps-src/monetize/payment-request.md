---
description: A API de solicitação de pagamento fornece uma solução integrada para que os aplicativos UWP ignorem o processo de exigir que um usuário insira informações de pagamento e selecione métodos de envio.
title: Simplificar pagamentos com a API de Solicitação de Pagamento
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, solicitação de pagamento
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685040"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Simplificar pagamentos com a API de Solicitação de Pagamento
A API de solicitação de pagamento para aplicativos UWP é baseada na [especificação de API de solicitação de pagamento W3C](https://w3c.github.io/browser-payment-api/). Ele oferece a capacidade de simplificar o processo de check-out em seus aplicativos UWP. Os usuários podem acelerar o check-out usando opções de pagamento e endereços de envio já salvos com seus conta Microsoft. Você pode aumentar sua taxa de conversão e reduzir o risco de violações de dados, pois as informações de pagamento são indexadas. Começando com a atualização do Windows 10 para criadores, os usuários podem usar suas opções de pagamento salvas para pagar facilmente entre experiências em aplicativos UWP.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar a usar a API de solicitação de pagamento, há algumas coisas que você deve fazer ou estar atentos.

### <a name="getting-a-merchant-id"></a>Obtendo uma ID de comerciante
Como parte do processo de solicitação de pagamento, a Microsoft solicita tokens de pagamento em seu nome do seu provedor de serviços. Portanto, antes de poder começar a usar a API, precisamos de sua autorização para solicitar esses tokens.  Você deve seguir algumas etapas para registrar-se para uma conta de vendedor e fornecer a autorização necessária. Para fazer isso, vá para [Microsoft seller Center](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp). Depois de fazer isso, copie a ID de comerciante resultante do Partner Center em seu aplicativo ao construir a solicitação de pagamento. Em seguida, quando seu aplicativo enviar uma solicitação de pagamento, você receberá um token de pagamento do processador que você precisará para enviar seu pagamento.

### <a name="geographic-restrictions-and-language-support"></a>Restrições geográficas e suporte a idiomas
A API de solicitação de pagamento pode ser usada somente por empresas baseadas nos EUA para processar transações no Estados Unidos.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Usando a API de solicitação de pagamento em seu aplicativo: passo a passo
Esta seção demonstra como usar a [API de solicitação de pagamento UWP](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) em seu aplicativo. Usamos a API aqui em sua forma mais simples para fins de clareza. Para obter um exemplo de uso mais avançado dessas APIs, consulte o [exemplo de aplicativo de compras UWP no GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Crie um conjunto de todas as opções de pagamento aceitas.
> [!Note]
> Substitua o texto **comerciante-ID-from-vendedor-portal** pela ID do comerciante que você recebeu do centro de vendedores.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Extraia os detalhes de pagamento juntos. 

Esses detalhes serão mostrados para o usuário no aplicativo de pagamento. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. inclua o imposto sobre vendas. 

> [!Important]
> A API não adiciona itens ou calcula o imposto sobre vendas para você. Lembre-se de que as tarifas de impostos variam por jurisdição. Para maior clareza, usamos uma taxa de imposto de 9,5% hipotética.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (opcional) adicionar descontos ou outros modificadores ao total. 

Aqui está um exemplo de como adicionar um desconto para usar um cartão de crédito da Contoso específico para os itens de exibição. (*Contoso* é um nome fictício.)

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. Reúna todos os detalhes de pagamento.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. envie a solicitação de pagamento. 

Chame o método **SubmitPaymentRequestAsync** para enviar sua solicitação de pagamento. Isso abre o aplicativo de pagamento mostrando as opções de pagamento disponíveis.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

O usuário é solicitado a entrar com seus conta Microsoft.

Depois que o usuário entrar, ele poderá selecionar as opções de pagamento e o endereço de envio que foram salvos anteriormente.

![Interface do usuário de solicitação de pagamento](./images/33.png "Interface do usuário de solicitação de pagamento")

Seu aplicativo aguarda que o usuário toque em **pagar**e, em seguida, conclui o pedido.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Após a conclusão do pagamento, o usuário receberá uma tela de **pedido confirmada** .

![Pedido confirmado](./images/44.png "Pedido confirmado")

## <a name="see-also"></a>Veja também
- [Documentação de referência do Windows. ApplicationModel. Payments](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [Exemplo de aplicativo de compras UWP no GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [Especificação de API de solicitação de pagamento W3C](https://www.w3.org/TR/payment-request/)
- [API de solicitação de pagamento](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

