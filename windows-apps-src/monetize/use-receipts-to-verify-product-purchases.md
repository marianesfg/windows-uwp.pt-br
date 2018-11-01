---
author: Xansky
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: Cada transação da Microsoft Store que resulta em uma compra do produto bem-sucedida, pode retornar, opcionalmente, um recibo da transação.
title: Usar recibos para verificar compras de produtos
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, compras no aplicativo, IAPs, recebimentos, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: ed79a3ac50fb3a6cbe735e0ea713256845d39441
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5923543"
---
# <a name="use-receipts-to-verify-product-purchases"></a>Usar recibos para verificar compras de produtos

Cada transação da Microsoft Store que resulta em uma compra do produto bem-sucedida, pode retornar, opcionalmente, um recibo da transação. Esse recibo fornece informações sobre o produto listado e o custo monetário ao cliente.

Ter acesso a essas informações dá suporte a cenários nos quais seu app precisa verificar se um usuário adquiriu seu app ou fez compras de complemento (também chamado de produto no app ou IAP) na Microsoft Store. Por exemplo, imagine um jogo que oferece conteúdo para download. Se o usuário que comprou o conteúdo do jogo desejar jogar em outro dispositivo, será necessário verificar se ele de fato comprou o conteúdo. Consulte aqui como fazer isso.

> [!IMPORTANT]
> Este artigo mostra como usar os membros do namespace [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) para obter e validar um recibo de uma compra realizada em aplicativo. Se você estiver usando o namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store) para compras realizadas em aplicativo (introduzido no Windows 10, versão 1607 e disponível para projetos que visam o **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio), esse namespace não fornece uma API para a obtenção de comprovantes para compras realizadas em aplicativo. No entanto, você pode usar um método REST na API de coleção da Microsoft Store para obter dados de uma transação de compra. Para obter mais informações, consulte [Recibos para compras realizadas em aplicativo](in-app-purchases-and-trials.md#receipts).

## <a name="requesting-a-receipt"></a>Solicitando um recibo


O namespace **Windows.ApplicationModel.Store** dá suporte a várias maneiras de obter um recibo:

* Quando você fizer uma compra usando [CurrentApp.RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) ou [CurrentApp.RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) (ou uma das sobrecargas desse método), o valor de retorno conterá a nota fiscal.
* Você pode chamar o método [CurrentApp.GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) para recuperar as informações do recibo atuais para seu app e os complementos em seu app.

Um recibo de app tem a seguinte aparência:

> [!NOTE]
> Este exemplo é formatado para ajudar a tornar o XML legível. Os recibos do app real não incluem espaços em branco entre os elementos.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

Um recibo de produto tem a seguinte aparência:

> [!NOTE]
> Este exemplo é formatado para ajudar a tornar o XML legível. Os recibos do produto real não incluem espaços em branco entre os elementos.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

Você pode usar qualquer um desses exemplos de recibo para testar seu código de validação. Para obter mais informações sobre o conteúdo do recebimento, consulte o [descrições de atributo e elemento](#receipt-descriptions).

## <a name="validating-a-receipt"></a>Validando um recibo

Para validar a autenticidade do recibo, você precisa de seu sistema back-end (um serviço web ou algo semelhante) para verificar a assinatura do recibo usando o certificado público. Para obter esse certificado, use a URL ```https://go.microsoft.com/fwlink/p/?linkid=246509&cid=CertificateId```, onde ```CertificateId``` é o valor **CertificateId** no recibo.

Veja aqui um exemplo desse processo de validação. Esse código é executado em um aplicativo de console do .NET Framework que inclui uma referência ao assembly **System. Security**.

> [!div class="tabbedCodeSnippets"]
[!code-cs[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>Descrições de atributo e elemento para um recibo

Esta seção descreve os elementos e atributos em um recibo.

### <a name="receipt-element"></a>Elemento de recibo

O elemento raiz desse arquivo é o elemento **Receipt**, que contém informações sobre apps e compras realizadas em aplicativo. Este elemento contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  |  Descrição   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    Não        |  0 ou 1  |  Contém informações de compra para o app atual.            |
|  [ProductReceipt](#productreceipt)  |     Não       |  0 ou mais    |   Contém informações sobre uma compra realizada em aplicativo para o app atual.     |
|  Assinatura  |      Sim      |  1   |   Esse elemento é um padrão [constructo XML-DSIG](http://go.microsoft.com/fwlink/p/?linkid=251093). Ele contém um elemento **SignatureValue**, que contém a assinatura que você pode usar para validar o recebimento, e um elemento **SignedInfo**.      |

**Receipt** tem os atributos a seguir.

|  Atributo  |  Descrição   |
|-------------|-------------------|
|  **Versão**  |    O número de versão do recibo.            |
|  **CertificateId**  |     A impressão digital do certificado usado para assinar o recibo.          |
|  **ReceiptDate**  |    A data do recibo foi assinado e baixado.           |  
|  **ReceiptDeviceId**  |   Identifica o dispositivo usado para solicitar o recibo.         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>Elemento AppReceipt

Este elemento contém informações de compra para o app atual.

**AppReceipt** tem os atributos a seguir.

|  Atributo  |  Descrição   |
|-------------|-------------------|
|  **Id**  |    Identifica a compra.           |
|  **AppId**  |     O valor do Nome da Família de Pacotes que o sistema operacional usa para o app.           |
|  **LicenseType**  |    **Full**, se o usuário tiver adquirido a versão completa do app. **Trial**, se o usuário baixou uma versão de avaliação do app.           |  
|  **PurchaseDate**  |    A data de aquisição do app.          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>Elemento ProductReceipt

Esse elemento contém informações sobre uma compra realizada em aplicativo para o app atual.

**ProductReceipt** tem os atributos a seguir.

|  Atributo  |  Descrição   |
|-------------|-------------------|
|  **Id**  |    Identifica a compra.           |
|  **AppId**  |     Identifica o app por meio do qual o usuário fez a compra.           |
|  **ProductId**  |     Identifica o produto adquirido.           |
|  **ProductType**  |    Determina o tipo do produto. No momento só é compatível com um valor **Durable**.          |  
|  **PurchaseDate**  |    A data em que ocorreu a compra.          |  |

 

 
