---
Description: Siga estas diretrizes para preparar os pacotes de seu aplicativo para envio à Microsoft Store.
title: Requisitos do pacote do aplicativo
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, requisitos do pacote, pacotes, formato de pacote, versão com suporte, enviar
ms.localizationpriority: medium
ms.openlocfilehash: 9c478b038ff729f753696d10fc18b21391a3a7de
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945984"
---
# <a name="app-package-requirements"></a>Requisitos do pacote do aplicativo

Siga estas diretrizes para preparar os pacotes de seu aplicativo para envio à Microsoft Store.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Antes de construir o pacote de seu aplicativo para a Microsoft Store

Certifique-se de [testar seu aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md). Também recomendamos testar seu aplicativo em diferentes tipos de hardware. Até o aplicativo ser certificado e disponibilizado na Microsoft Store, ele só poderá ser instalado e executado em computadores com licenças de desenvolvedor.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Criando o pacote do aplicativo usando o Microsoft Visual Studio

Se estiver usando o Microsoft Visual Studio como seu ambiente de desenvolvimento, você já tem ferramentas internas que tornam a criação do pacote do aplicativo um processo rápido e fácil. Para obter mais informações, consulte [Empacotando aplicativos](../packaging/index.md).

> [!NOTE]
> Verifique se todos os nomes de arquivo usam ANSI. 

Quando for criar seu pacote no Visual Studio, certifique-se de estar conectado com a mesma conta associada a sua conta de desenvolvedor. Algumas partes do manifesto do pacote têm detalhes específicos relacionados à sua conta. Essas informações são detectadas e adicionadas automaticamente. Sem as informações adicionais adicionadas ao manifesto, é possível encontrar falhas de carregamento de pacote. 

Quando você cria os pacotes UWP de seu aplicativo, o Visual Studio pode criar um arquivo. msix ou Appx, ou um arquivo. msixupload ou. appxupload. Para aplicativos UWP, recomendamos que você sempre carregue o arquivo. msixupload ou. appxupload na página [pacotes](upload-app-packages.md) . Para obter mais informações sobre como empacotar aplicativos UWP para a Store, consulte [Empacotar um aplicativo UWP com o Visual Studio](/windows/msix/package/packaging-uwp-apps).

Os pacotes do seu aplicativo não precisam ser assinados com um certificado proveniente de uma autoridade de certificação confiável.


### <a name="app-bundles"></a>Pacotes de aplicativos

Para aplicativos UWP, o Visual Studio pode gerar um pacote de aplicativo (. msixbundle ou. appxbundle) para reduzir o tamanho do aplicativo que os usuários baixam. Isso pode ser útil se você definiu ativos específicos de idioma, uma variedade de ativos em escala de imagem ou recursos que se aplicam a versões específicas do Microsoft DirectX.

> [!NOTE]
> Um pacote de aplicativos pode conter seus pacotes para todas as arquiteturas.

Com um lote de aplicativo, o usuário só baixa os arquivos relevantes, em vez de todos os recursos possíveis. Para obter mais informações sobre lotes de aplicativos, consulte [Empacotando aplicativos](../packaging/index.md) e [Empacotar um aplicativo UWP com o Visual Studio](/windows/msix/package/packaging-uwp-apps).


## <a name="building-the-app-package-manually"></a>Compilando o pacote do aplicativo manualmente

Se você não usa o Visual Studio para criar pacotes, [crie o manifesto do pacote manualmente](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Confira a documentação [Manifesto do pacote do aplicativo](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) para ver os todos os detalhes e requisitos do manifesto. O manifesto deve seguir o esquema de manifesto do aplicativo para ser aprovado na certificação.

Seu manifesto deve incluir algumas informações específicas sobre sua conta e seu aplicativo. Você pode encontrar essa informação em [visualizar detalhes de identidade do aplicativo](view-app-identity-details.md) na seção **Gerenciamento de aplicativo** da página de visão geral do seu aplicativo no painel.

> [!NOTE]
> Os valores no manifesto diferenciam maiúsculas de minúsculas. Espaços e outros sinais de pontuação também devem coincidir. Insira os valores com cuidado e os revise para garantir que estejam corretos.


Os pacotes de aplicativo (. msixbundle ou. appxbundle) usam um manifesto diferente. Confira a documentação [Manifesto do pacote](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest) para ver os detalhes e requisitos de manifestos de pacotes de aplicativos. Observe que em um. msixbundle ou. appxbundle, o manifesto de cada pacote incluído deve usar os mesmos elementos e atributos, exceto para o atributo **ProcessorArchitecture** do elemento [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) .

> [!TIP]
> Certifique-se de executar o [Kit de certificação de aplicativos para Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviar seus pacotes. Ele pode ajudar a determinar se o manifesto apresenta algum problema que possa causar falhas na certificação ou no envio.


## <a name="package-format-requirements"></a>Requisitos de formato de pacote

Os pacotes de seu aplicativo devem atender a estes requisitos.

| Propriedade de pacote de aplicativos | Requisito                                                          |
|----------------------|----------------------------------------------------------------------|
| Tamanho do pacote         | . msixbundle ou. appxbundle: máximo de 25 GB por pacote <br>pacotes. msix ou. Appx destinados ao máximo do Windows 10:25 GB por pacote<br>Pacotes .appx voltados para Windows 8.1: máximo de 8 GB por pacote <br> Pacotes .appx voltados para Windows 8: máximo de 2 GB por pacote <br> Pacotes .appx para Windows Phone 8.1: máximo de 4 GB por pacote <br> Pacotes .xap: máximo de 1 GB por pacote                                                                           |
| Hashes do mapa de blocos     | Algoritmo SHA2-256                                                   |

> [!IMPORTANT]
> Você não pode mais carregar novos pacotes XAP criados usando os SDKs do Windows Phone 8. x. Os aplicativos que já estão em armazenamento com pacotes XAP continuarão a funcionar em dispositivos Windows 10 Mobile. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="supported-versions"></a>Versões com suporte

Para aplicativos UWP, todos os pacotes devem direcionar uma versão do Windows 10 suportado pela Store. As versões para as quais seu pacote oferece suporte devem estar indicadas nos atributos **MinVersion** e **MaxVersionTested** do elemento [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) do manifesto do aplicativo.

As versões compatíveis atualmente variam de: 
- Mínimo: 10.0.10240.0
- Máximo: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>Arquivo XML StoreManifest

StoreManifest.xml é um arquivo de configuração opcional que pode ser incluído em pacotes de aplicativos. Seu objetivo é habilitar recursos, como declarar seu aplicativo como um aplicativo de dispositivo da Microsoft Store ou declarar que os requisitos dos quais um pacote depende são aplicáveis a um dispositivo, que o manifesto de pacote não abrange. Se usado, StoreManifest.xml é enviado com o pacote do aplicativo e deve estar na pasta raiz do projeto principal do seu aplicativo. Para saber mais, consulte [Esquema StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 




