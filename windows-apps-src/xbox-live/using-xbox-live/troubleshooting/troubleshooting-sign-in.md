---
title: Solução de problemas de Xbox Live entrar
description: Saiba como solucionar problemas com o Xbox Live entrar.
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, entrar, solucionar problemas
ms.localizationpriority: medium
ms.openlocfilehash: ff8d66105d8a1a44708bf23a681767a3044cb654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630321"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Solução de problemas de Xbox Live entrar

Há vários problemas que podem causar dificuldade para entrar.  Se você seguir as etapas em [Introdução ao Visual Studio para jogos da UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) você pode minimizar a chance de erros inesperados.

## <a name="common-issues"></a>Problemas comuns

### <a name="sandbox-problems"></a>Problemas de área restrita

Em termos gerais, você deve se familiarizar com o conceito de áreas de segurança e como eles pertencem ao Xbox Live.  Você pode obter mais informações na [áreas de segurança do Xbox Live](../../xbox-live-sandboxes.md) guia.

Em resumo, as áreas restritas impõem conteúdo isolamento e controle de acesso antes do lançamento comercial.  Os usuários sem acesso à sua área restrita de desenvolvimento não é possível executar qualquer leitura ou operações de gravação que pertencem ao seu título.  Você também pode publicar variações de configuração do serviço para diferentes áreas restritas para teste.

Algumas coisas a observar em relação a áreas restritas são discutidas abaixo.

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>Conta de desenvolvedor não tem acesso à área restrita à direita para acesso de tempo de execução

* Uma conta de teste (também conhecido como conta de desenvolvimento) ou uma conta de desenvolvedor autorizados deve ser usada para entrar em um título que está em desenvolvimento.  Verifique se você está tentando entrar com uma ou contas de teste são criadas no XDP na [ https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts ](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts). Você pode autorizar uma conta de desenvolvedor associado ao vivo do xbox no Partner Center em [https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* Certifique-se de que a conta tem acesso à área restrita que seu título é publicado.  As contas de teste que você criar no XDP herdam as permissões da conta XDP que foram criadas

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>Seu dispositivo não estiver na área restrita correta

O dispositivo que você estiver desenvolvendo em deve ser definido como uma área restrita de desenvolvimento.  No Xbox One, você pode definir usando sua área restrita *Xbox One Manager*.  Para Windows 10 Desktop, você pode usar o script de SwitchSandbox.cmd que está localizado no diretório de ferramentas da instalação do Xbox Live SDK.

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>Configuração de serviço do seu título não é publicada para a área restrita para desenvolvimento correto

Verifique se a que configuração de serviço do seu título for publicada em uma área restrita de desenvolvimento.  Você não pode entrar Xbox Live em uma área restrita de desenvolvimento fornecido para um título, a menos que título publicada à mesma área restrita.  Consulte a [documentação XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) para obter informações sobre como fazer isso. Você pode ler o [documentação do Partner Center](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration) para saber como publicar sua configuração do Partner Center.

### <a name="ids-configured-incorrectly"></a>IDs configuradas incorretamente

Há várias partes de identificação necessárias para configurar o seu jogo.  Você pode ver mais informações em [Introdução ao Visual Studio para jogos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) e [guia de Introdução cruzada jogue](../../get-started-with-partner/get-started-with-cross-play-games.md) dependendo do tipo de título que você está criando.

Algumas coisas a observar são:

* Verifique se que sua ID do aplicativo foi digitada corretamente em XDP ou Partner Center
* Certifique-se de que seu PFN foi digitado corretamente em XDP ou Partner Center
* Verifique novamente criou um xboxservices.config no mesmo diretório do projeto do Visual Studio, conforme descrito na [adicionando o Xbox Live para um projeto novo ou existente do UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) guia.
* Certifique-se de que o "pacote de identidade" em seu appxmanifest está correta.  Isso é mostrado no Partner Center como "/ identidade/nome do pacote" na seção de identidade do aplicativo.

### <a name="title-id-or-scid-not-configured-correctly"></a>ID de título ou SCID não está configurado corretamente

* Para títulos UWP, o título da ID e SCID deve ser definido como o valor correto em seu arquivo xboxservices.config.  Certifique-se também de que esse arquivo está formatado corretamente como UTF8.  Você pode ver mais informações em [Introdução ao Visual Studio para jogos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md). O arquivo xboxservices.config diferencia maiusculas de minúsculas.
* Para títulos XDK, esses valores são definidos em seu Package. appxmanifest.
* Você pode ver exemplos para a UWP e XDK configuração título no diretório de exemplos do SDK do Xbox Live.

## <a name="test-using-the-xbox-app"></a>Teste usando o App Xbox

Se você estiver desenvolvendo um aplicativo UWP, você pode depurar alguns problemas usando o Xbox App:

1. Conjunto de proteção do dispositivo para uma área restrita de desenvolvimento usando o script SwitchSandbox.cmd
2. Abra o App Xbox e tentar entrar usando uma conta de teste com acesso à mesma área restrita.

Se você conseguir entrar com êxito, em seguida, isso verifica se a área restrita de desenvolvimento foi definida corretamente em seu dispositivo, e sua conta de teste tem acesso a ele.

Se você ainda estiver recebendo erros de entrada, é provável que sua configuração de serviço não for publicada em sua área restrita, ou seu xboxservices.config não está configurada corretamente. O arquivo xboxservices.config diferencia maiusculas de minúsculas.

## <a name="debug-based-on-error-code"></a>Depuração com base no código de erro

A seguir estão alguns dos códigos de erro que você pode ver no sign-in e etapas que você pode executar para depurar esses.  Você verá o código de erro como mostra a captura de tela abaixo.

![captura de tela de erro de conexão 0x8015DC12](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003 o aplicativo está desabilitado ou configurado incorretamente

1. Tente excluir o arquivo PFX.

![arquivo PFX no Gerenciador de soluções](../../images/troubleshooting/pfx_file.png)

Se você não entrar no Visual Studio com a Microsoft Account usada para provisionar o aplicativo no Partner Center, o Visual Studio serão automaticamente gerar um arquivo pfx de assinatura com base em sua Account pessoal da Microsoft ou sua conta de domínio. Ao criar o pacote appx, o Visual Studio usará que auto pfx gerado para assinar o pacote e alterar a parte "publisher" da identidade do pacote no Package. appxmanifest. Como resultado, os bits produzidos (especificamente, appxmanifest. xml) terá uma identidade de pacote diferente que você pretende usar. 

2. Verifique se seu Package. appxmanifest está definido como a mesma identidade de aplicativo como seu título no Partner Center. Você pode clicar em seu projeto e escolher Store -> associar aplicativo com Store... como mostra a captura de tela abaixo. Ou editar manualmente seu Package. appxmanifest. Ver [Introdução ao Visual Studio para jogos UWP](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) para obter mais informações.

![Associar ao repositório](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>Erro de isolamento de conteúdo 0x8015DC12

Em resumo, isso significa que o dispositivo ou o usuário não tem acesso para o título especificado.

1. Isso pode significar que você não estiver usando uma conta de teste para tentar entrar, ou sua conta de teste não tem acesso à mesma área restrita que está conectado como. Verifique novamente as instruções sobre como criar contas de teste na [documentação XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx) e [documentação do Partner Center.](../../xbox-live-test-accounts.md) Se for necessário crie uma nova conta de teste com acesso à área restrita apropriado.

Talvez você precise remover sua conta antiga do Windows 10, você pode fazer isso, vá para configurações no Menu Iniciar, e, em seguida, indo para contas

2. Verifique se o título está publicado para a área restrita que você está tentando usar. Consulte a [documentação XDP](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) ou [documentação do Partner Center](../../xbox-live-service-configuration.md#sandbox-ids) para obter informações sobre como fazer isso.

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 título desconhecido ou inesperado

Verifique novamente a configuração de ID do aplicativo e o Centro de desenvolvimento XDP de associação. Você pode exibir as instruções em [adicionando Xbox Live suporte em um UWP Visual Studio novo ou existente](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)

### <a name="0x87dd000e-title-not-authorized"></a>título 0x87DD000E não autorizado

Verifique se o seu dispositivo está definido como a área restrita de desenvolvimento adequadas e se o usuário tem acesso à área restrita. Consulte a [teste usando o App Xbox](#test-using-the-xbox-app) seção para obter mais informações sobre como você pode verificar isso com o App Xbox.

Se isso não resolver o problema, também Verifique a configuração de associação do Dev Center e a ID do aplicativo conforme descrito acima.

Se você estiver recebendo um erro não descrito aqui, consulte a lista de erros na documentação do xbox::services::xbox_live_error_code para obter mais informações sobre os códigos de erro. Você também pode consultar errors.h no XSAPI inclui.

Após essas etapas, se você ainda não é possível entrar com seu título, poste um thread de suporte sobre o [fóruns](https://forums.xboxlive.com) ou entre em contato para sua mãe.
