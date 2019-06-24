---
description: A API de solicitação de pagamento fornece uma solução integrada para aplicativos UWP ignorar o processo de exigir que o usuário inserir informações de pagamento e selecione os métodos de envio.
title: Simplificar pagamentos com a API de Solicitação de Pagamento
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, uwp, solicitação de pagamento
ms.openlocfilehash: f055bacbddae88cdbd100b460d933682b3c78a13
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320058"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Simplificar pagamentos com a API de Solicitação de Pagamento
A API de solicitação de pagamento para aplicativos UWP se baseia a [especificações de API de solicitação de pagamento de W3C](https://w3c.github.io/browser-payment-api/). Ele fornece a capacidade de simplificar o processo de check-out em seus aplicativos UWP. Os usuários podem acelerar por meio do check-out, usando as opções de pagamento e já salvos com a respectiva conta da Microsoft de endereços para entrega. Você pode aumentar sua taxa de conversão e reduzir o risco de violações de dados, pois as informações de pagamento são indexadas. Começando com o Windows 10 Creators Update, os usuários podem usar suas opções de pagamento salvo para pagar com facilidade entre as experiências em aplicativos UWP.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar a usar a API de solicitação de pagamento, há algumas coisas que você deve fazer ou estar atento.

### <a name="getting-a-merchant-id"></a>Obter uma ID do comerciante
Como parte do processo de solicitação de pagamento, a Microsoft solicitar tokens de pagamento em seu nome de seu provedor de serviços. Portanto, antes de começar a usar a API, precisamos que sua autorização para solicitar esses tokens.  Você deve seguir algumas etapas para se registrar para uma conta de vendedor e forneça a autorização necessária. Para fazer isso, vá para [Microsoft Seller Center](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp). Assim que tiver feito isso, copie o comerciante resultante ID do Partner Center em seu aplicativo ao construir a solicitação de pagamento. Em seguida, quando seu aplicativo envia uma solicitação de pagamento, você receberá um token de pagamento do seu processador que você precisará enviar seu pagamento.

### <a name="geographic-restrictions-and-language-support"></a>As restrições geográficas e suporte a idiomas
A API de solicitação de pagamento pode ser usada somente por empresas baseado nos EUA para processar as transações nos Estados Unidos.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Usando a API de solicitação de pagamento do seu aplicativo: passo a passo
Esta seção demonstra como usar o [API de solicitação de pagamento de UWP](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments) em seu aplicativo. Podemos usar a API aqui em sua forma mais simples para fins de clareza. Para obter um exemplo de uso mais avançado dessas APIs, consulte o [UWP compras aplicativo de exemplo no GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Crie um conjunto de todas as opções de pagamento que você aceite.
> [!Note]
> Substitua os **comerciante id do vendedor portal** texto com o comerciante da ID que você recebeu do centro do vendedor.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Reunir os detalhes de pagamento. 

Esses detalhes serão mostrados para o usuário no aplicativo de pagamento. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. Inclui o imposto sobre vendas. 

> [!Important]
> A API não adicionar os itens ou calcular o imposto sobre vendas para você. Lembre-se de que as taxas de imposto variam de acordo com a jurisdição. Para maior clareza, usamos uma taxa de imposto de 9,5% hipotético.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (Opcional)  Adicione descontos ou outros modificadores no total. 

Aqui está um exemplo de como adicionar um desconto para usar um cartão de crédito Contoso específico para os itens de exibição. (*Contoso* é um nome fictício.)

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. Monte todos os detalhes de pagamento.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. Envie a solicitação de pagamento. 

Chame o **SubmitPaymentRequestAsync** método para enviar sua solicitação de pagamento. Isso abre o aplicativo de pagamento mostrando as opções de pagamento disponíveis.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

O usuário é solicitado a entrar com sua conta da Microsoft.

Depois que o usuário faz logon, eles podem selecionar opções de pagamento e endereço de envio que foram salvos anteriormente.

![Solicitação de pagamento da interface do usuário](./images/33.png "solicitação de pagamento da interface do usuário")

Seu aplicativo aguarda o usuário tocar **pagar**, em seguida, conclui a ordem.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Depois que o pagamento for concluído, o usuário é apresentado com uma **ordem confirmada** tela.

![Ordem confirmada](./images/44.png "ordem confirmada ")

## <a name="see-also"></a>Consulte também
- [Documentação de referência Windows.ApplicationModel.Payments](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [Exemplo de aplicativo UWP em compras no GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [Especificação da API de solicitação de pagamento do W3C](https://www.w3.org/TR/payment-request/)
- [Solicitação de pagamento de API ](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

