---
title: Solução de problemas de Xbox Live usando o Fiddler
description: Saiba como usar o Fiddler para solucionar problemas de chamadas de serviço Xbox Live.
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, fiddler, chamadas de serviço, solucionar problemas
ms.localizationpriority: medium
ms.openlocfilehash: 84c6717a4f9f5aff9fd3ff1f68c870fdd9174865
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606711"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>Solução de problemas de Xbox Live usando o Fiddler

O Fiddler é uma proxy que registra o tráfego de todos os HTTP e HTTPS entre o dispositivo e a Internet de depuração da web. Você o usará para fazer logon e inspecionar o tráfego de e para os serviços do Xbox Live e serviços de web de terceiros terceira parte confiável, para entender e depurar chamadas de serviço web.

## <a name="for-windows-uwp-pc-apps"></a>Para aplicativos Windows UWP PC

1. Verifique se o usuário atual está no grupo de administradores no computador
1. Baixar o Fiddler no [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
1. Certifique-se de que você selecione a versão que é "compilada para .NET 4"
1. Uma vez instalado, vá para Ferramentas -> Opções do Fiddler e habilitar o tráfego de capturar conexões HTTPS e descriptografar HTTPS.  Todas as comunicações entre o tempo de execução e os serviços do Xbox LIVE são criptografadas com SSL.  Sem essa opção, você não verá nada útil.  Aceite todas as caixas de diálogo que Fiddler pop-up (devem ser 5 caixas de diálogo incluindo UAC)
1. Vá para "WinConfig", "Isento tudo" e "Salvar alterações".  Caso contrário, o Fiddler não funcionará com aplicativos da Store.
1. Agora, se você executar seu aplicativo, você deve ver as solicitações/respostas entre o aplicativo, o tempo de execução e o Xbox LIVE.

Para depurar chamadas de nível de sistema operacional de UWP que geralmente não é necessário, mas pode ser útil durante a depuração de eventos de entrada e de jogos, você precisa configurar o Fiddler para capturar o tráfego do WinHTTP.
Isso pode ser feito com:
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
onde a porta 8888 corresponde a porta que você configurou nas ferramentas de Fiddlers | Opções | Guia de conexões que é 8888 por padrão.
Para desfazer isso, execute:
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>Para projetos da UWP do Xbox One com base em

Siga as etapas aqui [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>Para Xbox um XDK projetos com base em

Em operação normal, um console que se comunica por meio de um proxy está em risco de ter suas comunicações modificadas por proxy, possivelmente permitindo que os jogadores trapaceiem. Assim, os consoles são projetados para não permitir a comunicação através de um proxy. Usar o Fiddler com o kit de desenvolvimento do Xbox One requer que você execute algumas etapas de configuração especiais no kit de desenvolvimento para permitir que ele use o proxy do Fiddler.

Fiddler é gratuito e pode ser baixado no [próprio site](https://www.telerik.com/fiddler/).

Fiddler pode impactar no status de rede relatado pelo console. Se uma conexão upstream é desativada da máquina executando o Fiddler, o console não pode detectar essa desconexão até que a autenticação do console expire. Se você estiver usando o Fiddler, certifique-se de desconectar a conexão entre o console e o computador executando o Fiddler, em vez de usar o Fiddler para simular uma desconexão. Melhor ainda, use a sobrecarga de rede ferramentas então simular desconexão para fins de teste.

Instalar e habilitar o Fiddler no PC de desenvolvimento

Siga estas etapas para instalar e habilitar o Fiddler monitorar o tráfego do seu kit de desenvolvimento.

1. Instale o Fiddler no seu computador de desenvolvimento seguindo as instruções do [site do Fiddler](https://www.telerik.com/fiddler/).
1. Inicie o Fiddler e selecione as opções do Fiddler no menu Ferramentas.
1. Selecione a guia conexões e certifique-se de que permitir que computadores remotos para conectar-se a caixa de seleção esteja marcada.
1. Clique em Okey para aceitar a alteração para as configurações. Você verá uma caixa de diálogo afirmando que o Fiddler deve ser reiniciado para que a alteração entre em vigor e que talvez você precise configurar seu firewall manualmente. Clique em Okey nessa caixa de diálogo, mas não reiniciar Fiddler.
1. Configure as regras de firewall necessárias para permitir que os computadores remotos se conectem. Inicie o applet do Painel de Controle de Firewall do Windows. Clique em configurações avançadas e, em seguida, a regra de entrada. Encontre a regra chamada "FiddlerProxy" e role para a direita, verificando se cada uma das configurações a seguir aparece para essa regra.

| Configuração          | Valor preferido                |
|------------------|--------------------------------|
| Nome             | FiddlerProxy                   |
| Grupo            | (não define um valor de grupo) |
| Perfil          | Todas                            |
| Habilitado          | Sim                            |
| Ação           | Permitir                          |
| Substituição         | Não                             |
| Programa          | Caminho para fiddler.exe            |
| LocalAddress     | Qualquer                            |
| RemoteAddress    | Qualquer                            |
| Protocolo         | TCP                            |
| LocalPort        | Qualquer                            |
| RemotePort       | Qualquer                            |
| AllowedUsers     | Qualquer                            |
| AllowedComputers | Qualquer                            |


1. Configure o Fiddler para capturar e descriptografar tráfego HTTPS.
    1. Para habilitar o melhor desempenho, defina o Fiddler para usar o modo de Streaming, clicando no botão de Stream na barra de botões.
    1. No Fiddler, selecione Ferramentas, em seguida, opções do Fiddler e HTTPS.
    1. Marque a caixa de seleção de tráfego de HTTPS descriptografar. Se uma caixa de mensagem pergunta se deseja configurar o Windows confiar no certificado de autoridade de certificação, clique em não.
    1. Clique em Exportar certificado de raiz para o botão da área de trabalho.
1. Sair do Fiddler e iniciá-lo novamente.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Para configurar um kit de desenvolvimento para usar o Fiddler como seu proxy para a Internet
Configuração do Fiddler no kit de desenvolvimento foi simplificada do método usado em versões anteriores.

1. Copie o certificado de raiz do Fiddler que você exportou para a área de trabalho, o Kit de desenvolvimento como``` xs:\Microsoft\Cert\FiddlerRoot.cer```.  Você pode usar o comando a seguir:  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. Crie um arquivo de texto chamado ```ProxyAddress.txt```, com o endereço IP ou nome de host do computador de desenvolvimento que executam o Fiddler e o número da porta na qual o Fiddler está escutando como o único conteúdo no arquivo. Formate o nome/endereço IP e a porta da seguinte maneira: "HOST: PORTA". (Por padrão, o Fiddler usa a porta 8888.) Por exemplo, "10.124.220.250:8888" ou "my_dev_pc.contoso.com:8888". Copie esse arquivo para o kit de desenvolvimento como xs:\Microsoft\Fiddler\ProxyAddress.txt.  Você pode usar o comando a seguir:  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. Reinicialize o kit de desenvolvimento, digitando ```xbreboot``` no prompt de comando

### <a name="to-stop-using-fiddler"></a>Para parar de usar o Fiddler

Para interromper o usando o Fiddler como um proxy para a Internet e o rastreamento, portanto, todo tráfego de rede do kit de desenvolvimento no Fiddler, simplesmente renomear ou excluir o arquivo de FiddlerRoot.cer no kit de desenvolvimento e, em seguida, reinicialize o kit de desenvolvimento.

### <a name="how-it-works"></a>Como funciona

A presença de um arquivo xs:\Microsoft\Cert\FiddlerRoot.cer e um arquivo xs:\Microsoft\Fiddler\ProxyAddress.txt no momento da inicialização faz com que o kit de desenvolvimento para se configurar e usar o servidor proxy especificado na ProxyAddress.txt. Se nenhum desses arquivos estiver ausente, em seguida, o kit de desenvolvimento não se configurará para operar através de um proxy do Fiddler.

Cada computador com o Fiddler instalado usa um certificado raiz Fiddler diferente. Se você tiver mais de um PC que pode ser usado para fornecer um proxy do Fiddler para seu kit de desenvolvimento, você pode copiar o certificado de raiz do PC cada Fiddler no diretório xs:\Microsoft\Cert, desde que uma delas é chamada FiddlerRoot.cer. Todos os certificados no diretório de certificado são verificados quando um proxy do Fiddler está sendo autenticado. A instância do Fiddler no endereço do computador que está nos ProxyAddress.txt será usada como um proxy, desde que seu certificado está presente no diretório de certificado.
