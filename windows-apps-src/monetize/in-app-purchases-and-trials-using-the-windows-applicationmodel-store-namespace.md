---
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: Saiba como habilitar compras no aplicativo e avaliações em aplicativos UWP destinados a versões antes do Windows 10, versão 1607.
title: Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
keywords: uwp, compras no aplicativo, IAPs, complementos, avaliações, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: 72f5875721d17bda79842989c1ac22475a06e938
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193875"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store

Você pode usar membros no namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para adicionar a funcionalidade de compras realizadas em aplicativo e avaliação ao seu aplicativo UWP (Plataforma Universal do Windows) para ajudar na monetização de seu app. Estas APIs também fornecem acesso às informações de licença de seu app.

Os artigos nesta seção fornecem orientações detalhadas e exemplos de código para usar os membros no namespace **Windows.ApplicationModel.Store** para vários cenários comuns. Para uma visão geral dos conceitos básicos relacionados a compras realizadas em aplicativo em aplicativos UWP, consulte [Compras realizadas em aplicativo e avaliações](in-app-purchases-and-trials.md). Para obter um exemplo completo que demonstra como implementar avaliações e compras no aplicativo usando o namespace **Windows.ApplicationModel.Store**, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

> [!IMPORTANT]
> O namespace **Windows.ApplicationModel.Store** não está mais sendo atualizado com os novos recursos. Se o seu projeto se destina ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior no Visual Studio (ou seja, você tem como destino o Windows 10, versão 1607 ou posterior), recomendamos que você use o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Para obter mais informações, consulte [Compras no aplicativo e avaliações](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials). O namespace **ApplicationModel** não é suportado em aplicativos da área de trabalho do Windows que usam a [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop) ou em aplicativos ou jogos que usam uma área restrita de desenvolvimento no Partner Center (por exemplo, esse é o caso para qualquer jogo que se integra ao Xbox Live). Estes produtos devem usar o namespace **Windows.Services.Store** para implementar compras no aplicativo e avaliações.

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>Introdução às classes CurrentApp e CurrentAppSimulator

O ponto de entrada principal para o namespace **Windows.ApplicationModel.Store** é a classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx). Essa classe fornece métodos e propriedades estáticos que você pode usar para obter informações do aplicativo atual e seus complementos disponíveis, obter informações de licença do aplicativo atual ou seus complementos, comprar um aplicativo ou um complemento para o usuário atual e realizar outras tarefas.

A classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) obtém seus dados da Microsoft Store, portanto você deve ter uma conta de desenvolvedor e o app deve ser publicado na Store para que você possa usar com êxito esta classe em seu app. Antes de enviar seu app para a Store, você pode testar o código com uma versão simulada dessa classe chamada [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx). Depois de testar o app e antes de enviá-lo para a Microsoft Store, você deve substituir as instâncias de **CurrentAppSimulator** por **CurrentApp**. O app será reprovado na certificação se ele usar **CurrentAppSimulator**.

Quando **CurrentAppSimulator** é usado, o estado inicial do licenciamento e dos produtos no aplicativo é descrito em um arquivo local no computador de desenvolvimento chamado WindowsStoreProxy.xml. Para obter mais informações sobre esse arquivo, consulte [Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator](#proxy).

Para obter mais informações sobre tarefas comuns que você pode realizar usando **CurrentApp** e **CurrentAppSimulator**, consulte os artigos a seguir.

| Tópico       | Descrição                 |
|----------------------------|-----------------------------|
| [Excluir ou limitar recursos em uma versão de avaliação](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Se você permitir que os clientes usem seu app gratuitamente durante um período de avaliação, incentive-os a atualizar para a versão completa do app excluindo ou limitando alguns recursos durante o período de avaliação. |
| [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)      |  Seja seu app gratuito ou não, você pode vender conteúdo, outros apps ou uma nova funcionalidade do app (como o desbloqueio do próximo nível de um jogo) no próprio app. Veja a seguir como habilitar esses produtos no seu aplicativo.  |
| [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)      | Ofereça produtos consumíveis no aplicativo — itens que podem ser comprados, usados e comprados novamente — por meio da plataforma de comércio da Loja para proporcionar aos seus clientes uma experiência de compra robusta e confiável. Isso é especialmente útil para itens como moedas em jogos (ouro, moedas etc.) que podem ser comprados e então usados para comprar power-ups específicos. |
| [Gerenciar um catálogo abrangente de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md)      |   Se o seu app oferecer um catálogo abrangente de produtos no aplicativo, você também poderá seguir o processo descrito neste tópico para ajudar a gerenciar seu catálogo.    |
| [Usar recibos para verificar compras de produtos](use-receipts-to-verify-product-purchases.md)      |   Cada transação da Microsoft Store que resulta em uma compra do produto bem-sucedida pode retornar um recibo de transação que fornece informações sobre o produto listado e o custo monetário ao cliente. Ter acesso a essas informações dá suporte a cenários nos quais seu app precisa confirmar que um usuário adquiriu seu app ou fez compras de produtos no aplicativo da Microsoft Store. |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>Usando o arquivo WindowsStoreProxy.xml com CurrentAppSimulator

Quando **CurrentAppSimulator** é usado, o estado inicial do licenciamento e dos produtos no aplicativo é descrito em um arquivo local no computador de desenvolvimento chamado WindowsStoreProxy.xml. Os métodos **CurrentAppSimulator** que alteram o estado do app, por exemplo ao comprar uma licença ou realizar uma compra no aplicativo, atualizam somente o estado do objeto **CurrentAppSimulator** na memória. O conteúdo de WindowsStoreProxy.xml não é alterado. Quando o app é iniciado novamente, o estado da licença é revertido para o que está descrito no WindowsStoreProxy.xml.

Um arquivo WindowsStoreProxy.xml é criado por padrão no seguinte local: %UserProfile%\AppData\Local\Packages\\&lt;pasta do pacote do aplicativo&gt;\LocalState\Microsoft\Windows Store\ApiData. Você pode editar esse arquivo para definir o cenário que deseja simular nas propriedades **CurrentAppSimulator**.

Embora você possa modificar os valores nesse arquivo, recomendamos que crie seu próprio arquivo WindowsStoreProxy.xml (em uma pasta de dados do seu projeto do Visual Studio) para **CurrentAppSimulator** usar no lugar. Ao simular a transação, chame [ReloadSimulatorAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) para carregar o arquivo. Se você não chamar **ReloadSimulatorAsync** para carregar seu próprio arquivo WindowsStoreProxy.xml, **CurrentAppSimulator** criará/carregará (mas não substituirá) o arquivo WindowsStoreProxy.xml padrão.

> [!NOTE]
> Lembre-se de que **CurrentAppSimulator** não é totalmente inicializado até **ReloadSimulatorAsync** ser concluído. E, uma vez que **ReloadSimulatorAsync** é um método assíncrono, evite a condição de corrida de consulta de **CurrentAppSimulator** em um thread enquanto ele está sendo inicializado em outro. Uma técnica é usar um sinalizador para indicar que a inicialização foi concluída. Um app instalado da Microsoft Store deve usar **CurrentApp** em vez de **CurrentAppSimulator** e, nesse caso, **ReloadSimulatorAsync** não é chamado. Dessa forma, a condição de corrida mencionada há pouco não se aplica. Por esse motivo, projete seu código para que ele funcione nos dois casos, de forma assíncrona e síncrona.


<span id="proxy-examples" />

### <a name="examples"></a>Exemplos

Este exemplo é um arquivo WindowsStoreProxy.xml (UTF-16 codificado) que descreve um app com um modo de avaliação que expira às 05:00 (UTC) em 19 de janeiro de 2015.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

O exemplo a seguir é um arquivo WindowsStoreProxy.xml (UTF-16 codificado) que descreve um app que foi comprado, tem um recurso que expira às 05:00 (UTC) em 19 de janeiro de 2015 e tem uma compra de consumível realizada em aplicativo.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>Esquema

Esta seção lista o arquivo XSD que define a estrutura do arquivo WindowsStoreProxy.xml. Para aplicar esse esquema ao editor de XML no Visual Studio ao trabalhar com o arquivo WindowsStoreProxy.xml, faça o seguinte:

1. Abra o arquivo WindowsStoreProxy.xml no Visual Studio.
2. No menu **XML**, clique em **Criar Esquema**. Isso criará um arquivo WindowsStoreProxy.xsd temporário com base no conteúdo do arquivo XML.
3. Substitua o conteúdo desse arquivo. xsd pelo esquema abaixo.
4. Salve o arquivo em um local onde você pode aplicá-lo a vários projetos de app.
5. Alterne para o arquivo WindowsStoreProxy.xml no Visual Studio.
6. No menu **XML**, clique em **Esquemas** e localize a linha na lista para o arquivo WindowsStoreProxy.xsd. Se o local do arquivo não for aquele que você deseja (por exemplo, se o arquivo temporário ainda for exibido), clique em **Adicionar**. Navegue até o arquivo correto e clique em **OK**. Agora você deve ver o arquivo na lista. Verifique se uma marca de seleção aparece na coluna **Uso** para o esquema.

Depois de ter feito isso, as edições realizadas no WindowsStoreProxy.xml estarão sujeitas ao esquema. Para obter mais informações, consulte [Instruções: selecionar os esquemas XML a serem usados](http://go.microsoft.com/fwlink/p/?LinkId=403014).

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>Descrições de elemento e atributo

Esta seção descreve os elementos e atributos no arquivo WindowsStoreProxy.xml.

O elemento raiz desse arquivo é o elemento **CurrentApp**, que representa o app atual. Este elemento contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  |  Descrição   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    Sim        |  1  |  Contém dados dos detalhes do aplicativo.            |
|  [LicenseInformation](#licenseinformation)  |     Sim       |   1    |   Descreve as licenças disponíveis para esse app e seus complementos duráveis.     |
|  [ConsumableInformation](#consumableinformation)  |      Não      |   0 ou 1   |   Descreve os complementos consumíveis que estão disponíveis para esse app.      |
|  [Simulation](#simulation)  |     Não       |      0 ou 1      |   Descreve como as chamadas para vários métodos [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) funcionarão no app durante o teste.    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>Elemento ListingInformation

Este elemento contém dados dos detalhes do aplicativo. **ListingInformation** é um filho obrigatório do elemento **CurrentApp**.

**ListingInformation** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  |  Descrição   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    Sim   |  1   |    Fornece dados sobre o app.         |
|  [Product](#product-child-of-listinginformation)  |    Não  |  0 ou mais   |      Descreve um complemento para o app.     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>Elemento App (filho de ListingInformation)

Este elemento descreve a licença do app. **App** é um filho obrigatório do elemento [ListingInformation](#listinginformation).

**App** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  | Descrição   |
|-------------|------------|--------|--------|
|  **AppId**  |    Sim   |  1   |   O GUID que identifica o app na Loja. Pode ser qualquer GUID para testes.        |
|  **LinkUri**  |    Sim  |  1   |    O URI da página de listagem na loja. Pode ser qualquer URI válido para testes.         |
|  **CurrentMarket**  |    Sim  |  1   |    O país/região do cliente.         |
|  **AgeRating**  |    Sim  |  1   |     Um inteiro que representa a classificação etária mínima do app. Isso é o mesmo valor especificado no Partner Center quando você envia o aplicativo. Os valores usados pela Loja são: 3, 7, 12 e 16. Para obter mais informações sobre essas classificações, consulte [Classificações etárias](../publish/age-ratings.md).        |
|  [MarketData](#marketdata-child-of-app)  |    Sim  |  1 ou mais      |    Contém informações sobre o app para um determinado país/região. Para cada país/região em que o app estiver listado, você deve incluir um elemento **MarketData**.       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>Elemento MarketData (filho de App)

Este elemento fornece informações sobre o app para um determinado país/região. Para cada país/região em que o app estiver listado, você deve incluir um elemento **MarketData**. **MarketData** é um filho obrigatório do elemento [App](#app-child-of-listinginformation).

**MarketData** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  | Descrição   |
|-------------|------------|--------|--------|
|  **Name**  |    Sim   |  1   |   O nome do app nesse país/região.        |
|  **Description**  |    Sim  |  1   |      A descrição do app para esse país/região.       |
|  **Price**  |    Sim  |  1   |     O preço do app nesse país/região.        |
|  **CurrencySymbol**  |    Sim  |  1   |     O símbolo de moeda usado nesse país/região.        |
|  **CurrencyCode**  |    Não  |  0 ou 1      |      O código de moeda usado nesse país/região.         |  |

**MarketData** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **xml:lang**  |    Sim        |     Especifica o país/região ao qual as informações de dados do mercado se aplicam.          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Elemento Product (filho de ListingInformation)

Este elemento descreve um complemento para o app. **Product** é um filho opcional do elemento [ListingInformation](#listinginformation) e contém um ou mais elementos [MarketData](#marketdata-child-of-product).

**Product** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **ProductId**  |    Sim        |    Contém a cadeia de caracteres usada pelo app para identificar o complemento.           |
|  **LicenseDuration**  |    Não        |    Indica o número de dias em que a licença será válida após a aquisição do item. A data de expiração da nova licença criada pela compra de um produto é a data de compra mais a duração da licença. Este atributo é usado somente se o atributo **ProductType** for **Durable**; esse atributo é ignorado para complementos consumíveis.           |
|  **ProductType**  |    Não        |    Contém um valor para identificar a persistência do produto no aplicativo. Os valores com suporte são **Durable** (o padrão) e **Consumable**. Para os tipos duráveis, as informações adicionais são descritas por um elemento [Product](#product-child-of-licenseinformation) em [LicenseInformation](#licenseinformation); para os tipos consumíveis, as informações adicionais são descritas por um elemento [Product](#product-child-of-consumableinformation) em [ConsumableInformation](#consumableinformation).           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>Elemento MarketData (filho de Product)

Este elemento fornece informações sobre o complemento para um determinado país/região. Para cada país/região em que o complemento estiver listado, você deve incluir um elemento **MarketData**. **MarketData** é um filho obrigatório do elemento [Product](#product-child-of-listinginformation).

**MarketData** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  | Descrição   |
|-------------|------------|--------|--------|
|  **Name**  |    Sim   |  1   |   O nome do complemento nesse país/região.        |
|  **Price**  |    Sim  |  1   |     O preço do complemento nesse país/região.        |
|  **CurrencySymbol**  |    Sim  |  1   |     O símbolo de moeda usado nesse país/região.        |
|  **CurrencyCode**  |    Não  |  0 ou 1      |      O código de moeda usado nesse país/região.         |  
|  **Description**  |    Não  |   0 ou 1   |      A descrição do complemento para esse país/região.       |
|  **Tag**  |    Não  |   0 ou 1   |      Os [dados personalizados do desenvolvedor](../publish/enter-add-on-properties.md#custom-developer-data) (também chamados de tag) para o complemento.       |
|  **Keywords**  |    Não  |   0 ou 1   |      Contém até 10 elementos **Keyword** que contêm as [palavras-chave](../publish/enter-add-on-properties.md#keywords) para o complemento.       |
|  **ImageUri**  |    Não  |   0 ou 1   |      O [URI da imagem](../publish/create-add-on-store-listings.md#icon) na descrição do complemento.           |  |

**MarketData** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **xml:lang**  |    Sim        |     Especifica o país/região ao qual as informações de dados do mercado se aplicam.          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>Elemento LicenseInformation

Este elemento descreve as licenças disponíveis para esse app e seus produtos duráveis no aplicativo. **LicenseInformation** é um filho obrigatório do elemento **CurrentApp**.

**LicenseInformation** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  | Descrição   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    Sim   |  1   |    Descreve a licença do app.         |
|  [Product](#product-child-of-licenseinformation)  |    Não  |  0 ou mais   |      Descreve o status da licença de um complemento durável no app.         |   |

A tabela a seguir mostra como simular algumas condições comuns combinando valores sob os elementos **App** e **Product**.

|  Condição para simulação  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  Totalmente licenciado  |    true   |  false  |    Ausente. Na verdade, ele pode estar presente e especificar uma data futura, mas é recomendável omitir o elemento do arquivo XML. Se estiver presente e especificar uma data no passado, **IsActive** será ignorado e considerado como false.          |
|  Em período de avaliação  |    true  |  true   |      &lt;uma data/hora no futuro&gt; Este elemento deve estar presente porque **IsTrial** é true. Você pode visitar um site mostrando o Tempo Universal Coordenado (UTC) atual para saber quanto tempo no futuro deve ser definido para obter o período de avaliação restante desejado.         |
|  Avaliação expirada  |    false  |  true   |      &lt;uma data/hora no passado&gt; Este elemento deve estar presente porque **IsTrial** é true. Você pode visitar um site mostrando o Tempo Universal Coordenado (UTC) atual para saber quando "o passado" está no UTC.         |
|  Inválido  |    false  | false       |     &lt;qualquer valor ou omitido&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>Elemento App (filho de LicenseInformation)

Este elemento descreve a licença do app. **App** é um filho obrigatório do elemento [LicenseInformation](#licenseinformation).

**App** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  | Descrição   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Sim   |  1   |    Descreve o estado atual da licença do app. O valor **true** indica que a licença é válida; **false** indica uma licença inválida. Normalmente, esse valor é **true**, não importa se o app tem um modo de avaliação ou não.  Defina esse valor como **false** para testar o comportamento do app quando ele tem uma licença inválida.           |
|  **IsTrial**  |    Sim  |  1   |      Descreve o estado atual da avaliação do app. O valor **true** indica que o app está sendo usado durante o período de avaliação; **false** indica que o app não está em uma avaliação, porque foi comprado ou o período de avaliação expirou.         |
|  **ExpirationDate**  |    Não  |  0 ou 1       |     A data em que o período de avaliação do app expira, no Tempo Universal Coordenado (UTC). A data deve ser expressa como: yyyy-mm-ddThh:mm:ss.ssZ. Por exemplo, 05:00 em 19 de janeiro de 2015 deve ser especificada como 2015-01-19T05:00:00.00Z. Esse elemento é necessário quando **IsTrial** é **true**. Caso contrário, não será necessário.          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Elemento Product (filho de LicenseInformation)

Este elemento descreve o status da licença de um complemento durável no app. **Product** é um filho opcional do elemento [LicenseInformation](#licenseinformation).

**Product** contém os elementos filho a seguir.

|  Elemento  |  Obrigatório  |  Quantidade  | Descrição   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Sim   |  1     |    Descreve o estado atual da licença do complemento. O valor **true** indica que o complemento pode ser usado; **false** indica que o complemento não pode ser usado ou não foi comprado           |
|  **ExpirationDate**  |    Não   |  0 ou 1     |     A data em que o complemento expira, no Tempo Universal Coordenado (UTC). A data deve ser expressa como: yyyy-mm-ddThh:mm:ss.ssZ. Por exemplo, 05:00 em 19 de janeiro de 2015 deve ser especificada como 2015-01-19T05:00:00.00Z. Se esse elemento estiver presente, o complemento tem uma data de expiração. Se não estiver presente, o complemento não expira.  |  

**Product** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **ProductId**  |    Sim        |   Contém a cadeia de caracteres usada pelo app para identificar o complemento.            |
|  **OfferId**  |     Não       |   Contém a cadeia de caracteres usada pelo app para identificar a categoria à qual pertence o complemento. Isso fornece suporte para catálogos abrangentes de itens, conforme descrito em [Gerenciar um catálogo abrangente de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md).           |

<span id="simulation"/>

#### <a name="simulation-element"></a>Elemento Simulation

Este elemento descreve como as chamadas para vários métodos [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) funcionarão no app durante o teste. **Simulation** é um filho opcional do elemento **CurrentApp** e contém zero ou mais elementos [DefaultResponse](#defaultresponse).

**Simulation** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **SimulationMode**  |    Não        |      Os valores podem ser **Interactive** ou **Automatic**. Quando esse atributo é definido como **Automatic**, os métodos retornarão automaticamente os códigos de erro HRESULT especificados. Isso pode ser usado durante a execução de casos de teste automatizados.       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>Elemento DefaultResponse

Este elemento descreve o código de erro padrão retornado por um método **CurrentAppSimulator**. **DefaultResponse** é um filho opcional do elemento [Simulation](#simulation).

**DefaultResponse** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **MethodName**  |    Sim        |   Designe esse atributo a um dos valores de enumeração mostrados para o tipo **StoreMethodName** no [esquema](#schema). Cada um desses valores de enumeração representa um método **CurrentAppSimulator** para o qual você deseja simular um valor retornado de código de erro em seu app durante o teste. Por exemplo, o valor **RequestAppPurchaseAsync_GetResult** indica que você deseja simular o valor retornado do código de erro do método [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync).            |
|  **HResult**  |     Sim       |   Designe esse atributo a um dos valores de enumeração mostrados para o tipo **ResponseCodes** no [esquema](#schema). Cada um desses valores de enumeração representa o código de erro que você deseja retornar para o método designado ao atributo **MethodName** para este elemento **DefaultResponse**.           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>Elemento ConsumableInformation

Este elemento descreve os complementos consumíveis disponíveis para o app. **ConsumableInformation** é um filho opcional do elemento **CurrentApp** e pode conter zero ou mais elementos [Product](#product-child-of-consumableinformation).

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Elemento Product (filho de ConsumableInformation)

Este elemento descreve um complemento consumível. **Product** é um filho opcional do elemento [ConsumableInformation](#consumableinformation).

**Product** tem os atributos a seguir.

|  Atributo  |  Obrigatório  |  Descrição   |
|-------------|------------|----------------|
|  **ProductId**  |    Sim        |   Contém a cadeia de caracteres usada pelo app para identificar o complemento consumível.            |
|  **TransactionId**  |     Sim       |   Contém um GUID (como uma cadeia de caracteres) usado pelo app para acompanhar a transação de compra de um consumível pelo processo de atendimento. Consulte [Habilitar compras de produtos consumíveis realizada em aplicativo](enable-consumable-in-app-product-purchases.md).            |
|  **Status**  |      Sim      |  Contém a cadeia de caracteres usada pelo app para indicar o status de atendimento de um consumível. Os valores podem ser **Active**, **PurchaseReverted**, **PurchasePending** ou **ServerError**.             |
|  **OfferId**  |     Não       |    Contém a cadeia de caracteres usada pelo app para identificar a categoria à qual pertence o consumível. Isso fornece suporte para catálogos abrangentes de itens, conforme descrito em [Gerenciar um catálogo abrangente de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md).           |
