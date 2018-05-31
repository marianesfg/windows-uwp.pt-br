---
author: Mtoepke
title: Problemas conhecidos com o Programa de desenvolvedor UWP no Xbox
description: Lista os problemas conhecidos da UWP no programa para desenvolvedores do Xbox.
ms.author: mstahl
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: b93da1603056a75c746e0e5c2a8ebe1d4044a1e5
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690392"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Problemas conhecidos com o Programa de desenvolvedor UWP no Xbox

Este tópico descreve problemas conhecidos com o Programa de desenvolvedor UWP no Xbox One. Para saber mais sobre esse programa, consulte [UWP no Xbox](index.md). 

\[Se você chegou até aqui a partir de um link em um tópico de referência de API e estiver procurando informações sobre a API da família de dispositivos Universal, consulte [Recursos da UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/?LinkID=760755).\]

A lista a seguir destaca alguns problemas conhecidos que podem ocorrer, embora essa não seja uma lista completa. 

**Queremos receber seu feedback**, portanto, relate todos os problemas que você encontrar no fórum [Desenvolvendo aplicativos da Plataforma Universal do Windows](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop). 

Se você se perder, leia as informações neste tópico. Consulte [Perguntas frequentes](frequently-asked-questions.md) e use os fóruns para pedir ajuda.

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>A implantação com base em VS falha com os Controle dos Pais ativado

A inicialização do aplicativo com base em VS falhará se o console tiver os Controles dos Pais ativados em Configurações.

Para contornar esse problema, desabilite temporariamente os Controles dos Pais ou:
1. Implante seu aplicativo no console com os Controles dos Pais desativados.
2. Ative os Controles dos Pais.
3. Inicie seu aplicativo a partir do console.
4. Digite um PIN ou uma senha para permitir a inicialização do aplicativo.
5. O aplicativo será iniciado.
6. Feche o aplicativo.
7. Inicie a partir do VS usando F5, e o aplicativo será iniciado sem aviso.

Nesse caso, a permissão é _fixa_ até você desconectar o usuário, mesmo se você desinstalar e reinstalar o aplicativo.
 
Há outro tipo de isenção disponível apenas para contas de crianças. Uma conta de criança requer que o pai conecte-se para conceder permissão. Ao conectar, o pai tem a opção de escolher **Sempre** para permitir que a criança inicie o aplicativo. Essa isenção é armazenada na nuvem e persistirá mesmo que a criança saia e entre novamente.

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>StorageFile.CopyAsync falha ao copiar arquivos criptografados para um destino não criptografado 

Quando StorageFile.CopyAsync for usado para copiar um arquivo criptografado para um destino que não esteja criptografado, a chamada falhará com esta exceção:

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

Isso pode afetar os desenvolvedores do Xbox que desejam copiar os arquivos implantados como parte do pacote do app para outro local. O motivo disso é que o conteúdo do pacote é criptografado em um Xbox em modo de varejo, mas não no Modo de Desenvolvimento. Como resultado, o aplicativo aparentemente pode funcionar como esperado durante o desenvolvimento e os testes, mas depois falhará quando for publicado e instalado em um Xbox de varejo.
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Portas de rede bloqueadas no Xbox One

Os aplicativos da Plataforma Universal do Windows (UWP) em dispositivos do Xbox One não podem se associar a portas na faixa [57344, 65535], inclusive. Embora a associação a essas portas possa parecer ter sido bem-sucedida em tempo de execução, o tráfego de rede pode diminuir silenciosamente antes de atingir o aplicativo. Seu aplicativo deverá se associar à porta 0 sempre que possível, o que permite que o sistema selecione a porta local. Se você precisar usar uma porta específica, o número da porta deverá estar no intervalo [1025, 49151], e você deve verificar e evitar conflitos com o registro IANA. Para obter mais informações, consulte o [Nome do serviço e registro de número de porta de protocolo de transporte](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml).

## <a name="uwp-api-coverage"></a>Cobertura de API UWP

Nem todas as APIs UWP têm suporte no Xbox. Para obter a lista de APIs que sabemos que não funcionam, consulte [Recursos da UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755). Se você encontrar problemas com outras APIs, informe-os nos fóruns. 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>Navegar para o WDP provoca um aviso de certificado

Você receberá um aviso sobre o certificado que foi fornecido, semelhante à captura de tela a seguir, porque o certificado de segurança assinado por seu console do Xbox One não é considerado um publicador confiável conhecido. Para acessar o Windows Device Portal, clique em **Continuar para este site**.

![Aviso de certificado de segurança do site](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>Limitação de KnownFolders.MediaServerDevices no Xbox

Na Área de trabalho, os servidores de mídia são "emparelhados" ao computador e o Serviço de associação de dispositivos sempre rastreia quais servidores estão online no momento para que uma consulta inicial do sistema de arquivo imediatamente possa retornar uma lista dos servidores emparelhados online no momento.

No Xbox, não há nenhuma interface do usuário para adicionar ou remover servidores, portanto, a consulta inicial do sistema de arquivos sempre retornará vazia. Você deve criar uma consulta e assinar o evento ContentsChanged, em seguida, atualizar a consulta sempre que receber uma notificação. Os servidores oferecem informações e a maioria será descoberta em três segundos.

Código de exemplo simples:

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>Consulte também
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)
