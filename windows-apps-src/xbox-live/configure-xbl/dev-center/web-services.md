---
title: Serviços Web
description: Saiba como criar um serviço Web para seu aplicativo
ms.assetid: ''
ms.date: 06-04-2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, serviços da web
ms.openlocfilehash: 66668336e3575201b305e6ecf09b4f277d2fc7b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600011"
---
# <a name="set-up-web-services-in-udc"></a>Configurar os serviços Web em UDC

> [!WARNING]
> O artigo a seguir é para ID@Xbox e desenvolvedores de parceiros gerenciadas apenas devido a restrições colocadas em configuração do serviço Web. Configuração de serviços da Web só está disponível para desenvolvedores com a permissão de nível de conta do terceiras concedida. Se você não tiver controle da sua conta permissões no nível entre em contato com seu gerente de conta de desenvolvimento (mãe) para obter assistência.

Os editores podem criar serviços da Web se eles desejam personalizar o modo como seus aplicativos/títulos interagir com serviços do Xbox Live. Serviços Web são configurações no nível do publicador e podem ser chamados por qualquer título em uma área restrita pelo editor de propriedade ao configurar o logon único.

Motivos para definir os serviços da Web:

1. Fornecer logon único para usuários do Xbox Live - para seu serviço web fornecer logon único para usuários do Xbox Live, ele precisa ser configurado como parte confiante do Xbox Live. Quando configurado dessa forma, os usuários autenticados com o Xbox Live automaticamente serão autenticados ao seu serviço sem a necessidade de inserir novamente um conjunto diferente de credenciais.
2. Fazer chamadas de serviço de seu serviço para serviços do Xbox Live - se o seu produto usará um dos seus serviços web para fazer chamadas para um serviço Xbox Live, diretamente ou em nome de usuários individuais, será necessário um certificado do parceiro de negócios.

1. ## <a name="create-a-web-service"></a>Criar um serviço Web

1. Vá para o [painel da Central de parceiro](https://partner.microsoft.com/dashboard/windows/overview)  
2. Clique no ícone em forma de engrenagem no canto superior direito da página para acessar o **configurações** lista suspensa.
3. No menu suspenso, selecione **configurações do desenvolvedor**.
4. Na barra de navegação à esquerda, expanda a opção **Xbox Live** e selecione **serviços Web**.

![gif de serviços Web ](../../images/dev-center/web-services/web-services.gif)

5. Na página de serviços da Web, clique em **novo serviço Web**.
6. Insira o nome do serviço Web e escolha o tipo de acesso conforme necessário.  
    * Acesso de telemetria permite que seu serviço recuperar dados de telemetria do jogo para qualquer um dos seus jogos.
    * Acesso ao canal de aplicativo fornece o provedor de mídia que possui o serviço de autoridade para publique programaticamente os canais de aplicativo para consumo no console por meio de Trança OneGuide.
7. Clique em **Salvar**

Neste ponto, você definiu o serviço e Xbox Live está ciente da existência do serviço. Dependendo os motivos para criar o serviço web, você precisará configurar Parties(Single Sing-On) da terceira parte confiável ou negócios parceiro Certificates(Service-to-service calls).  

## <a name="configure-relying-party"></a>Configurar a terceira parte confiável

Um serviço web precisa ser configurado como uma terceira parte confiável do Xbox live para fornecer a experiência de logon único para usuários do Xbox Live – os usuários autenticados com o Xbox Live serão automaticamente autenticados para o serviço web sem precisar reinserir uma outro conjunto de credenciais. Para facilitar isso, a relação de confiança deve ser estabelecida entre serviços do Xbox e o serviço web. Um conjunto de declarações (como o gamertag, tipo de dispositivo, ID de título) são usados como parte das configurações de terceiros de terceira parte confiável para impor essa relação de confiança. Isso é que as informações trocadas entre o Xbox Live e o serviço web para ajudar a autenticar automaticamente os usuários.

### <a name="create-a-relying-party"></a>Crie uma terceira

1. Vá para o painel do Partner Center  
2. Clique no ícone em forma de engrenagem no canto superior direito da página para acessar o **configurações** lista suspensa.
3. No menu suspenso, selecione **configurações do desenvolvedor**.
4. Na barra de navegação à esquerda, expanda a opção **Xbox Live** e selecione **terceiras**.
5. Na página de partes confiáveis, clique em **novo de terceira parte confiável**.
6. Insira um URI para a terceira parte confiável neste formato: *exemplo.com*.
7. Selecione o tipo de criptografia a ser usado para garantir a segurança do serviço de terceiros de terceira parte confiável.
8. Se você tiver selecionado a criptografia simétrica com chaves compartilhadas na etapa anterior, clique em **gerar nova chave** para obter uma nova chave compartilhada. Siga as instruções na tela para salvar com segurança a chave.
9. Insira o **tempo de vida de Token** em horas.
10. Sob **declarações**, a lista suspensa oferece uma lista de declarações que o serviço de terceiros de terceira parte confiável pode usar para fins de autenticação. Selecione todas as declarações que você deseja usar. As declarações selecionadas serão exibida abaixo da lista suspensa. Algumas declarações padrão serão populadas nesse espaço por padrão.
11. Clique em **salvar** quando você terminar.  

## <a name="configure-a-business-partner-certificate"></a>Configurar um certificado do parceiro de negócios

Se seu produto usará um dos seus serviços web para fazer chamadas para um serviço Xbox Live, diretamente ou em nome de usuários individuais, será necessário um certificado do parceiro de negócios.

### <a name="generate-a-business-partner-certificate"></a>Gerar um certificado do parceiro de negócios

Continue com as etapas abaixo depois de criar com êxito um serviço Web.  

1. Na página de Web Services, localize o serviço web que você deseja associar um certificado do parceiro de negócios com.
2. Selecione o **gerar certificado** link ao serviço web escolhido.
3. Clique em **Mostrar opções** ao lado de gerar um novo certificado. Isso exibirá os comandos que devem ser executados a partir do PowerShell com privilégios de administrador.
4. Executar todos os comandos de um após o outro com êxito deve dar uma Base64 codificado em blob. Esta é a chave pública. Copie a chave pública do PowerShell e cole-o no espaço reservado para o Blob de CSP.
5. Clique em **baixar** e siga as instruções na página para associar os certificados.
    1. Use o mesmo computador em que você usou para gerar a chave pública.
    2. Execute este comando no PowerShell: *mmc.exe*
    3. Selecione o arquivo e selecione Adicionar/Remover Snap-In do.
    4. Selecione certificados e selecione Adicionar. Certifique-se de selecionar conta de computador para o snap-in de certificado e, em seguida, clique em Concluir e clique Okey.
    5. Abra o repositório Personal\Certificate.
    6. Clique com botão direito em certificados e selecione todas as tarefas e selecione Importar.
    7. Selecione o certificado que você baixou do UDC.
    8. Clique com botão direito no certificado na interface do usuário depois que ele foi importado e selecione todas as tarefas e selecione Exportar.
    9. Siga o Assistente de exportação e certifique-se de selecionar para exportar a chave privada com o certificado.
    10. Conclua o Assistente de exportação.