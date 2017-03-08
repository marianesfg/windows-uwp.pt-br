---
author: jnHs
Description: "Siga estas diretrizes para preparar os pacotes de seu app para envio à Windows Store."
title: Requisitos do pacote do app
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 59660de0adb6ff1247ea90f0ace3bcca35f19d1a
ms.lasthandoff: 02/07/2017

---

# <a name="app-package-requirements"></a>Requisitos do pacote do app

Siga estas diretrizes para preparar os pacotes de seu app para envio à Windows Store.

## <a name="before-you-build-your-apps-package-for-the-windows-store"></a>Antes de construir o pacote de seu app para a Windows Store

Certifique-se de [testar seu app com o Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186449). Também recomendamos testar seu app em diferentes tipos de hardware. Até o app ser certificado e disponibilizado na Windows Store, ele só poderá ser instalado e executado em computadores com licenças de desenvolvedor.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Criando o pacote do app usando o Microsoft Visual Studio

Se estiver usando o Microsoft Visual Studio como seu ambiente de desenvolvimento, você já tem ferramentas internas que tornam a criação do pacote do app um processo rápido e fácil. Para obter mais informações, consulte [Empacotando apps](https://msdn.microsoft.com/library/windows/apps/mt270969).

> **Observação**  Certifique-se de que todos os seus nomes de arquivo usem ANSI. 


Quando for criar seu pacote no Visual Studio, certifique-se de estar conectado com a mesma conta associada a sua conta de desenvolvedor. Algumas partes do manifesto do pacote têm detalhes específicos relacionados à sua conta. Essas informações são detectadas e adicionadas automaticamente.

Quando você cria os pacotes de seu app, o Visual Studio pode criar um arquivo .appx ou .appxupload (ou .xap para Windows Phone 8.1 e versões anteriores). Para apps voltados para o Windows 10, sempre carregue o arquivo .appxupload na página [Pacotes](upload-app-packages.md). Para obter mais informações sobre o empacotamento de apps UWP para a Loja, consulte [Empacotando apps universais do Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

Os pacotes do seu app não precisam ser assinados com um certificado proveniente de uma autoridade de certificação confiável.

### <a name="app-bundles"></a>Pacotes de apps

Para apps voltados para Windows 8.1, Windows Phone 8.1 e versões posteriores, o Visual Studio pode gerar um lote de app (.appxbundle) para reduzir o tamanho do app que os usuários baixam. Isso pode ser útil se você definiu ativos específicos de idioma, uma variedade de ativos em escala de imagem ou recursos que se aplicam a versões específicas do Microsoft DirectX.

> **Observação**  Um lote de app pode conter seus pacotes para todas as arquiteturas. Você deve enviar apenas um pacote para cada sistema operacional de destino.


Com um lote de app, o usuário só baixa os arquivos relevantes, em vez de todos os recursos possíveis. Para obter mais informações sobre lotes de app, consulte [Empacotando apps](https://msdn.microsoft.com/library/windows/apps/mt270969) e [Empacotando apps universais do Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

## <a name="building-the-app-package-manually"></a>Compilando o pacote do app manualmente

Se você não usa o Visual Studio para criar pacotes, [crie o manifesto do pacote manualmente](https://msdn.microsoft.com/library/windows/apps/br211476).

Confira a documentação [Manifesto do pacote do app](https://msdn.microsoft.com/library/windows/apps/br211474) para ver os todos os detalhes e requisitos do manifesto. O manifesto deve seguir o esquema de manifesto do app para ser aprovado na certificação.

Seu manifesto deve incluir algumas informações específicas sobre sua conta e seu app. Você pode encontrar essa informação em [visualizar detalhes de identidade do app](view-app-identity-details.md) na seção **Gerenciamento de app** da página de visão geral do seu app no painel.

> **Observação**  Os valores no manifesto diferenciam maiúsculas de minúsculas. Espaços e outros sinais de pontuação também devem corresponder. Insira os valores com cuidado e os revise para garantir que estejam corretos.


Pacotes de apps usam outro manifesto. Analise a documentação do [Manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/dn263089) para obter os detalhes e os requisitos para manifestos de lote de app.

> **Dica**  Certifique-se de executar o [Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) antes de enviar seus pacotes. Ele pode ajudar a determinar se o manifesto apresenta algum problema que possa causar falhas na certificação ou no envio.


Se seu app tiver mais de um pacote, os elementos de manifesto do app devem ser os mesmos em cada pacote (por sistema operacional de destino):

-   [**Pacote/Funcionalidades**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**Pacote/Dependências**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**Pacote/Recursos**](https://msdn.microsoft.com/library/windows/apps/br211462)

## <a name="package-format-requirements"></a>Requisitos de formato de pacote

Os pacotes de seu app devem atender a estes requisitos.

| Propriedade de pacote de apps | Requisito                                                          |
|----------------------|----------------------------------------------------------------------|
| Tamanho do pacote         | .appxbundle: máximo de 25 GB por pacote <br>Pacotes .appx voltados para Windows 10: máximo de 25 GB por pacote<br>Pacotes .appx voltados para Windows 8.1: máximo de 8 GB por pacote <br> Pacotes .appx voltados para Windows 8: máximo de 2 GB por pacote <br> Pacotes .appx para Windows Phone 8.1: máximo de 4 GB por pacote <br> Pacotes .xap: máximo de 1 GB por pacote                                                                           |
| Hashes do mapa de blocos     | Algoritmo SHA2-256                                                   |
 

## <a name="storemanifest-xml-file"></a>Arquivo XML StoreManifest

StoreManifest.xml é um arquivo de configuração opcional que pode ser incluído em pacotes de apps. Seu objetivo é habilitar recursos, como declarar seu app como um app de dispositivo da Windows Store ou declarar que os requisitos dos quais um pacote depende são aplicáveis a um dispositivo, que o manifesto de pacote não abrange. StoreManifest.xml é enviado com o pacote do app e deve estar na pasta raiz do projeto principal de seu app. Para saber mais, consulte [Esquema StoreManifest](https://msdn.microsoft.com/library/windows/apps/mt617325).

 

 





