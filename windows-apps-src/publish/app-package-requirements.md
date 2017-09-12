---
author: jnHs
Description: "Siga estas diretrizes para preparar os pacotes de seu aplicativo para envio à Windows Store."
title: Requisitos do pacote do app
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 39926699a81ce6882a46f4ec89b863d6147717dd
ms.sourcegitcommit: bfa61aae632cca0c68dbfb0168424d38fd607f84
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2017
---
# <a name="app-package-requirements"></a>Requisitos do pacote do app

Siga estas diretrizes para preparar os pacotes de seu aplicativo para envio à Windows Store.

## <a name="before-you-build-your-apps-package-for-the-windows-store"></a>Antes de construir o pacote de seu aplicativo para a Windows Store

Certifique-se de [testar seu aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md). Também recomendamos testar seu aplicativo em diferentes tipos de hardware. Até o aplicativo ser certificado e disponibilizado na Windows Store, ele só poderá ser instalado e executado em computadores com licenças de desenvolvedor.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Criando o pacote do aplicativo usando o Microsoft Visual Studio

Se estiver usando o Microsoft Visual Studio como seu ambiente de desenvolvimento, você já tem ferramentas internas que tornam a criação do pacote do aplicativo um processo rápido e fácil. Para obter mais informações, consulte [Empacotando aplicativos](../packaging/index.md).

> [!NOTE]
> Verifique se todos os nomes de arquivo usam ANSI. 

Quando for criar seu pacote no Visual Studio, certifique-se de estar conectado com a mesma conta associada a sua conta de desenvolvedor. Algumas partes do manifesto do pacote têm detalhes específicos relacionados à sua conta. Essas informações são detectadas e adicionadas automaticamente. Sem as informações adicionais adicionadas ao manifesto, é possível encontrar falhas de carregamento de pacote. 

Quando você cria os pacotes de seu aplicativo, o Visual Studio pode criar um arquivo .appx ou .appxupload (ou .xap para Windows Phone 8.1 e versões anteriores). Para aplicativos voltados para o Windows 10, sempre carregue o arquivo .appxupload na página [Pacotes](upload-app-packages.md). Para obter mais informações sobre o empacotamento de aplicativos UWP para a Loja, consulte [Empacotando aplicativos universais do Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

Os pacotes do seu aplicativo não precisam ser assinados com um certificado proveniente de uma autoridade de certificação confiável.


### <a name="app-bundles"></a>Pacotes de aplicativos

Para aplicativos voltados para Windows 8.1, Windows Phone 8.1 e versões posteriores, o Visual Studio pode gerar um lote de aplicativo (.appxbundle) para reduzir o tamanho do aplicativo que os usuários baixam. Isso pode ser útil se você definiu ativos específicos de idioma, uma variedade de ativos em escala de imagem ou recursos que se aplicam a versões específicas do Microsoft DirectX.

> [!NOTE]
> Um pacote de aplicativos pode conter seus pacotes para todas as arquiteturas. Você deve enviar apenas um pacote para cada sistema operacional de destino.

Com um lote de aplicativo, o usuário só baixa os arquivos relevantes, em vez de todos os recursos possíveis. Para obter mais informações sobre lotes de aplicativos, consulte [Empacotando aplicativos](../packaging/index.md) e [Empacotar um aplicativo UWP com o Visual Studio](../packaging/packaging-uwp-apps.md).


## <a name="building-the-app-package-manually"></a>Compilando o pacote do aplicativo manualmente

Se você não usa o Visual Studio para criar pacotes, [crie o manifesto do pacote manualmente](https://msdn.microsoft.com/library/windows/apps/br211476).

Confira a documentação [Manifesto do pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474) para ver os todos os detalhes e requisitos do manifesto. O manifesto deve seguir o esquema de manifesto do aplicativo para ser aprovado na certificação.

Seu manifesto deve incluir algumas informações específicas sobre sua conta e seu aplicativo. Você pode encontrar essa informação em [visualizar detalhes de identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo** da página de visão geral do seu aplicativo no painel.

> [!NOTE]
> Os valores no manifesto diferenciam maiúsculas de minúsculas. Espaços e outros sinais de pontuação também devem coincidir. Insira os valores com cuidado e os revise para garantir que estejam corretos.


Pacotes de aplicativos usam outro manifesto. Confira a documentação [Manifesto do pacote](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest) para ver os detalhes e requisitos de manifestos de pacotes de aplicativos.

> [!TIP]
> Execute o [Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviar seus pacotes. Ele pode ajudar a determinar se o manifesto apresenta algum problema que possa causar falhas na certificação ou no envio.

Se seu aplicativo tiver mais de um pacote, os elementos de manifesto do aplicativo devem ser os mesmos em cada pacote (por sistema operacional de destino):

-   [**Pacote/Funcionalidades**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**Pacote/Dependências**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**Pacote/Recursos**](https://msdn.microsoft.com/library/windows/apps/br211462)


## <a name="package-format-requirements"></a>Requisitos de formato de pacote

Os pacotes de seu aplicativo devem atender a estes requisitos.

| Propriedade de pacote de aplicativos | Requisito                                                          |
|----------------------|----------------------------------------------------------------------|
| Tamanho do pacote         | .appxbundle: máximo de 25 GB por pacote <br>Pacotes .appx voltados para Windows 10: máximo de 25 GB por pacote<br>Pacotes .appx voltados para Windows 8.1: máximo de 8 GB por pacote <br> Pacotes .appx voltados para Windows 8: máximo de 2 GB por pacote <br> Pacotes .appx para Windows Phone 8.1: máximo de 4 GB por pacote <br> Pacotes .xap: máximo de 1 GB por pacote                                                                           |
| Hashes do mapa de blocos     | Algoritmo SHA2-256                                                   |
 

## <a name="storemanifest-xml-file"></a>Arquivo XML StoreManifest

StoreManifest.xml é um arquivo de configuração opcional que pode ser incluído em pacotes de aplicativos. Seu objetivo é habilitar recursos, como declarar seu aplicativo como um aplicativo de dispositivo da Windows Store ou declarar que os requisitos dos quais um pacote depende são aplicáveis a um dispositivo, que o manifesto de pacote não abrange. StoreManifest.xml é enviado com o pacote do aplicativo e deve estar na pasta raiz do projeto principal de seu aplicativo. Para saber mais, consulte [Esquema StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 




