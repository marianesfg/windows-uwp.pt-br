---
author: laurenhughes
title: Hospedagem de pacotes de aplicativo UWP no AWS para instalação pela Web
description: Tutorial para configurar o servidor de web AWS validar a instalação do aplicativo por meio do instalador de aplicativo do aplicativo
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, sideload de instalador, AppInstaller, aplicativo, relacionados pacotes opcionais, definidos, AWS
ms.localizationpriority: medium
ms.openlocfilehash: f24abac93e2444a3c9f454c8883902e5db4d31be
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5946524"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>Hospedagem de pacotes de aplicativo UWP no AWS para instalação pela Web

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

Este tópico descreve as etapas para configurar um site Amazon Web Services (AWS) para pacotes do aplicativo host UWP e como usar o instalador de aplicativo para instalar os pacotes de aplicativos.

## <a name="setup"></a>Configuração

Para seguir com sucesso este tutorial, você precisará do seguinte:
 
1. Assinatura AWS 
2. Página da Web
3. Pacote do aplicativo UWP: o pacote do aplicativo que você distribuirá

Opcional: [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tem um pacote do aplicativo ou página da Web para trabalhar, mas ainda quer aprender a usar esse recurso.

Este tutorial passará pela como configurar uma página da web e pacotes de host no AWS. Isso exigirá uma assinatura AWS. Dependendo da operação de escala, você pode usar sua participação livre para seguir este tutorial. 

## <a name="step-1---aws-membership"></a>Etapa 1 - associação AWS
Para obter uma associação AWS, visite a [página de detalhes da conta AWS](https://aws.amazon.com/free/). Para os fins deste tutorial, você pode usar uma associação gratuita.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Etapa 2 - criar uma classificação Amazon S3

Amazon Simple Storage Service (S3) é um AWS oferta para coletar, armazenar e analisar dados. S3 buckets são uma maneira conveniente de pacotes de aplicativo UWP de host e páginas da web para distribuição. 

Depois de fazer logon AWS com suas credenciais, em `Services` localizar `S3`. 

Selecione **criar compartimento de memória**e insira um **nome de partição de memória** para seu site. Siga os prompts de caixa de diálogo para definir propriedades e permissões. Para garantir que seu aplicativo UWP pode ser distribuído pelo seu site, habilitar a **leitura** e permissões de **gravação** para o compartimento de memória e selecione **conceder acesso de leitura público para esse compartimento de memória**.

![Definir permissões no compartimento de memória Amazon S3](images/aws-permissions.png) 

Revise o resumo para garantir que as opções selecionadas são refletidas. Clique em **Criar partição de memória** para concluir esta etapa. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>Etapa 3 - carregar o pacote de aplicativo UWP e páginas da web para uma classificação S3

Um você criou uma classificação Amazon S3, você poderá vê-lo em sua exibição Amazon S3. Aqui está um exemplo da aparência de nosso compartimento de memória de demonstração:

![Modo de exibição de classificação Amazon S3](images/aws-post-create.png)

Agora estamos prontos para carregar os pacotes de aplicativo e páginas da web que gostaríamos para hospedar nosso compartimento Amazon S3 em. 

Clique no compartimento de memória recém-criado para carregar o conteúdo. O compartimento de memória está vazio, já que nada tem sido carregado. Clique no botão **carregar** e selecione os pacotes de aplicativos e arquivos de página da web que você deseja carregar.

> [!NOTE]
> Você pode usar o pacote do aplicativo que faz parte do repositório de [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) fornecido no GitHub se não tiver um pacote do aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. O certificado deve ser instalado em seu dispositivo antes de instalar o aplicativo.

![Carregue o pacote de aplicativo](images/aws-upload-package.png)

Assim como as permissões para a criação de uma classificação Amazon S3, o conteúdo no compartimento de memória também deve ter **ler**, **gravar**e permissões de **conceder acesso de leitura público para esse objeto (s)** .

Se você quiser testar carregar uma página da web, mas não tiver um, você pode usar a página de html de exemplo (default) do [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Antes de carregar a página da web, confirme se a referência do pacote do aplicativo na página da web está correta. 

Para obter a referência de pacote do aplicativo, carregue o pacote do aplicativo pela primeira vez e copie a URL do pacote de aplicativo. Edite a página da web html para refletir o caminho do pacote de aplicativo correta. Consulte o exemplo de código para obter mais detalhes. 

Selecione o arquivo de pacote do aplicativo carregados para obter o link de referência para o pacote do aplicativo, ele deve ser semelhante a este exemplo:

![caminho do pacote carregados](images/aws-package-path.png)

**Copie** o link para o aplicativo do pacote e adicionar a referência em sua página da web. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
Carregue o arquivo html para sua classificação Amazon S3. Lembre-se de definir as permissões para permitir o acesso de **leitura** e **gravação** .

## <a name="step-4---test"></a>Etapa 4 - teste

Depois que a página da web são carregados em sua classificação Amazon S3, obtenha o link para a página da web, selecionando o arquivo html carregados.

Use o link para abrir a página da web. Como definimos permissões para conceder acesso público para o pacote do aplicativo e a página da web, qualquer pessoa com o link para a página da web poderão acessá-lo e instalar seus pacotes de aplicativo UWP usando o instalador de aplicativo. Observe que o instalador de aplicativo é parte da plataforma Windows 10. Como desenvolvedor, você não precisará adicionar qualquer código adicional ou recursos ao seu aplicativo para habilitar o uso do instalador de aplicativo. 

## <a name="troubleshooting"></a>Solução de problemas

### <a name="app-installer-fails-to-install"></a>Falha na instalação do instalador de aplicativo 

Instalação do aplicativo falhará se o certificado assinado com o pacote do aplicativo não estiver instalado no dispositivo. Para corrigir isso, você precisará instalar o certificado antes da instalação do aplicativo. Se você estiver hospedando um pacote do aplicativo para distribuição pública, é recomendável assinar o pacote de aplicativo com um certificado de autoridade de certificação. 


