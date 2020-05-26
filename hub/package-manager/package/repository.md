---
title: Enviar seu manifesto para o repositório
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4c1a8ab3c6a2cc697729fb5551e686a465bf6a0c
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83825117"
---
# <a name="submit-your-manifest-to-the-repository"></a>Enviar seu manifesto para o repositório

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Depois de criar um [manifesto do pacote](manifest.md) que descreva seu aplicativo, você estará pronto para enviar seu manifesto para o repositório do Gerenciador de Pacotes do Windows. Esse é um repositório voltado para o público que contém uma coleção de manifestos que a ferramenta **winget** pode acessar. Para enviar seu manifesto, você o carregará pagara o repositório [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) de software livre no GitHub.

Depois de enviar uma solicitação de pull para adicionar um novo manifesto ao repositório do GitHub, um processo automatizado validará o arquivo de manifesto e verificará se o pacote não é conhecido como mal-intencionado. Se essa validação for bem-sucedida, seu pacote será adicionado ao repositório do Gerenciador de Pacotes do Windows voltado ao público para que possa ser descoberto pela ferramenta de cliente **winget**. Observe a distinção entre os manifestos no repositório do GitHub de software livre e o repositório do Gerenciador de Pacotes do Windows voltado para o público.

> [!IMPORTANT]
> A Microsoft se reserva o direito de recusar um envio por qualquer motivo.

## <a name="third-party-repositories"></a>Repositórios de terceiros

No momento, não há repositórios de terceiros conhecidos. A Microsoft está trabalhando com vários parceiros para desenvolver protocolos ou uma API para habilitar repositórios de terceiros.

## <a name="manifest-validation"></a>Validação do manifesto

Quando você envia um manifesto para o repositório [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) no GitHub, seu manifesto será validado e avaliado automaticamente quanto à segurança do ecossistema do Windows. Os manifestos também podem ser examinados manualmente.

## <a name="how-to-submit-your-manifest"></a>Como enviar seu manifesto

Para enviar um manifesto para o repositório, siga estas etapas.

### <a name="step-1-validate-your-manifest"></a>Etapa 1: Validar seu manifesto

A ferramenta **winget** fornece o comando [validate](..\winget\validate.md) para confirmar que você criou o manifesto corretamente. Para validar seu manifesto, use este comando.

```CMD
winget validate \<manifest-file>
```

Se a validação falhar, use os erros para localizar o número de linha e fazer uma correção. Depois que o manifesto for validado, você poderá enviá-lo para o repositório.

### <a name="step-2-clone-the-repository"></a>Etapa 2: Clone o repositório

Em seguida, crie uma bifurcação do repositório e a clone.

1. Acesse [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) em seu navegador e clique em **Bifurcação**.
    ![imagem da bifurcação](images\fork.png)

2. Em um ambiente de linha de comando, como o Prompt de Comando do Windows ou o PowerShell, use o comando a seguir para clonar sua bifurcação.
    ```CMD
    git clone \<your-fork-name>
    ```

 3. Se você estiver fazendo vários envios, crie um branch em vez de uma bifurcação. No momento, permitimos apenas um arquivo de manifesto por envio.
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>Etapa 3: Adicionar seu manifesto ao repositório local

É necessário adicionar o arquivo de manifesto ao repositório na seguinte estrutura de pastas:

**manifests** / **publisher** / **application** / **version.yaml**

* A pasta **manifests** é a pasta raiz para todos os manifestos no repositório.
* A pasta **publisher** é o nome da empresa que publica o software. Por exemplo, **Microsoft**.
* A pasta **application** é o nome do aplicativo ou da ferramenta. Por exemplo, **VSCode**.
* **version.yaml** é o nome de arquivo do manifesto. O nome do arquivo deve ser definido como a versão atual do aplicativo. Por exemplo, **1.0.0.yaml**.

>[!IMPORTANT]
> O valor `Id` no manifesto deve corresponder aos nomes do editor e do aplicativo no caminho da pasta de manifesto, e o valor `version` no manifesto deve corresponder à versão no nome do arquivo. Para obter mais informações, confira [Criar seu manifesto do pacote](manifest.md#tips-and-best-practices).

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>Etapa 4: Enviar seu manifesto para o repositório remoto

Agora você já pode enviar seu novo manifesto por push para o repositório remoto.

1. Use o comando `add` para preparar o envio.
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. Use o comando `commit` para confirmar a alteração e fornecer informações sobre o envio.
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. Use o comando `push` para enviar por push as alterações do repositório remoto.
    ```CMD
    `git push`
    ```

### <a name="step-5-create-a-pull-request"></a>Etapa 5: Criar uma solicitação pull

Após enviar suas alterações por push, volte para [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) e crie uma solicitação pull para mesclar a bifurcação ou o branch com o branch **mestre**.

![imagem da guia solicitação pull](images\pull-request.png)

## <a name="validation-process"></a>Processo de validação

Quando você criar uma solicitação pull, isso iniciará um processo de automação que valida o manifesto e processa sua solicitação pull. Adicionamos rótulos à sua solicitação pull para que você possa acompanhar o progresso.

### <a name="submission-expectations"></a>Expectativas de envio

Todos os envios de aplicativo para o repositório do Gerenciador de Pacotes do Windows devem ser bem comportados. Veja algumas expectativas para envios:

* O manifesto está em conformidade com os [requisitos do esquema](manifest.md#manifest-contents).
* Todas as URLs no manifesto levam a sites seguros.
* O instalador e o aplicativo não contêm vírus.
* O aplicativo é instalado e desinstalado corretamente para administradores e não administradores.
* O instalador dá suporte a modos não interativos.
* Todas as entradas de manifesto são precisas e não enganosas.
* O instalador é fornecido diretamente do site do editor.

### <a name="pull-request-labels"></a>Rótulos de solicitação pull

Durante a validação, aplicamos uma série de rótulos à nossa solicitação pull para comunicar o progresso.

* **É necessário: comentários do autor**: Há uma falha no envio. Transferiremos a solicitação pull de volta para você. Se você não resolver o problema dentro de 10 dias, fecharemos a solicitação pull.
* **Manifest-Validation-Error**: o manifesto enviado contém um erro de sintaxe.
* **URL-Validation-Error**: falha na validação [SmartScreen](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview) de uma ou mais URLs no envio.
* **Binary-Validation-Error**: o teste de verificação de vírus do instalador do aplicativo enviado falhou ou há uma incompatibilidade de hash.
* **Pull-Request-Error**: há um problema com a solicitação pull. Por exemplo, a estrutura de pastas não tem o [formato necessário](#step-3-add-your-manifest-to-the-local-repository).
* **Validation-Error**: falha no teste de validação geral do aplicativo enviado.
* **Validation-Installation-Error**: falha no teste de instalação do aplicativo enviado.
* **Validation-Uninstall-Error**: falha no teste de desinstalação do aplicativo enviado.
* **Validation-Virus-Scan-Error**: falha no teste de verificação de vírus do aplicativo enviado.
* **Azure-Pipeline-Passed**: o manifesto concluiu a primeira parte da validação. Após essa etapa, sua solicitação pull será atribuída à nossa equipe de teste para validação final.
* **Validation-Completed**: a validação foi concluída e sua solicitação pull será mesclada.
