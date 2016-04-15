---
Description: O Windows 10 versão 1511 e as atualizações das ferramentas de desenvolvedor continuarão a fornecer as ferramentas, os recursos e as experiências avançadas da Plataforma Universal do Windows.
title: Novidades para desenvolvedores no Windows 10, versão 1511: novembro de 2015
---

# Novidades para desenvolvedores no Windows 10, versão 1511: novembro de 2015

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O Windows 10 versão 1511 e as atualizações das ferramentas de desenvolvedor continuarão a fornecer as ferramentas, os recursos e as experiências avançadas da Plataforma Universal do Windows. [Instale as ferramentas e o SDK](https://dev.windows.com/downloads) no Windows 10, versão 1511, e você estará pronto para [criar um novo aplicativo Universal do Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) ou descobrir como você pode usar seu [código de aplicativo existente no Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

## Experiência do usuário

As novas classes <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpList</a> e <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpListItem</a> fornecem aplicativos com a capacidade de selecionar de forma programática o tipo de lista de atalhos gerenciada pelo sistema que desejam usar, para adicionar pontos de entrada da tarefa personalizada e grupos personalizados à sua lista de atalhos.

## Entrada
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx">Interceptador de entrega de teclado</a>
                                        
    Permite a um aplicativo substituir o sistema de processamento de dados brutos de teclado, incluindo teclas de atalho, teclas de acesso (ou hot keys), teclas de aceleração e teclas de aplicativo, mas excluindo as combinações de teclas de sequência segura (SAS).

    As combinações de teclas de sequência segura (SAS), incluindo Ctrl + Alt + Del e Windows L, continuam sendo processadas pelo sistema.
                                        
* Encadeamento de processo cruzado de entrada de ponteiro para os <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx">aplicativos UWP</a> e <a href="https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx">aplicativos do Windows clássico</a>.
                                        
    Novos eventos de ponteiro que permitem encadeamento de processo cruzado de entrada.    
                                        
* <a href="https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx">Apresentador de tinta para aplicativos de área de trabalho clássicos</a>
                                        
    As APIs do apresentador de tinta permitem aos aplicativos do Microsoft Win32 gerenciarem a entrada, processamento e renderização de entrada à tinta (padrão e modificada) por meio de um objeto <a href="https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx">InkPresenter</a> inserido na árvore virtual <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx">DirectComposition</a> do aplicativo.    
                                    
## Rede
                                                                        
Para usuários de WebSockets: <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">MessageWebSocket.OutputStream.FlushAsync</a> e <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">StreamWebSocket.OutputStream.FlushAsync</a> foram implementadas integralmente e aguardam pelas chamadas WriteAsync emitidas anteriormente para concluir. Observe que isso pode causar que um código existente gere uma exceção se o WebSocket estiver em um estado inválido quando você chamar <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">FlushAsync</a>.    

Uma nova propriedade <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">CookieUsageBehavior</a> foi adicionada à classe existente <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">Windows.Web.Http.Filters.HttpBaseProtocolFilter</a>. Isso permite que os desenvolvedores tenham controle sobre como os cookies são manipulados pelo sistema.    
                                    
## ORTC
                                    
O Microsoft Edge agora implementa o <a href="https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx">ORTC (Comunicações em Tempo Real do Objeto)</a> permitindo chamadas de áudio e/ou vídeo em tempo real na web diretamente entre navegadores, dispositivos móveis e servidores por meio de APIs Javascript nativas. Os desenvolvedores agora podem criar aplicativos de comunicação em tempo real de áudio/vídeo avançados sobre o navegador Microsoft Edge usando a API do ORTC, com suporte para chamadas de vídeo do grupo, simulcast, codificação de vídeo escalável (SVC) e muito mais.    

Para obter uma demonstração de uma chamada de áudio/vídeo individual por meio da API ORTC entre navegadores Microsoft Edge, visite <a href="/microsoft-edge/testdrive/demos/ortcdemo/">demonstrações e sites de Test Drive</a>. Para uma visão geral e código de amostra de noções básicas, visite a <a href="https://msdn.microsoft.com/library/mt588497(v=vs.85).aspx">entrada do Guia do desenvolvedor ORTC</a>.
                                        
## Ferramentas de Desenvolvedor F12 do Microsoft Edge
                                                                        
O Microsoft Edge apresenta novas melhorias excelentes para as ferramentas de desenvolvedor F12, incluindo alguns dos recursos mais solicitados de <a href="https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer">UserVoice</a>. Explore novos recursos no Explorador do DOM, Console, depurador, rede, desempenho, memória, emulação e uma nova ferramenta de experimentos, que permite que você experimente novos recursos poderosos antes que eles estejam concluídos. As novas ferramentas são criadas no TypeScript e estão sempre em execução, sem necessidade de recarregamentos. Além disso, a documentação das ferramentas de desenvolvedor F12 agora faz parte do <a href="http://dev.modern.ie/">site de desenvolvimento do Microsoft Edge</a> e está totalmente disponível em <a href="https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation">GitHub</a>. Neste ponto em diante, os documentos não serão apenas influenciados por seus comentários, mas você está convidado para contribuir e ajudar a dar forma a nossa documentação. Para obter uma breve introdução em vídeo para as ferramentas de desenvolvedor F12, visite <a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools">One Dev Minute do Channel 9</a>.    
                                    
## Windows Hello
                                    
O Windows Hello fornece ao seu aplicativo a capacidade de habilitar o reconhecimento de face ou de impressão digital para fazer logon em um sistema Windows ou dispositivo.

As APIs de provedores permitem que os IHVs e OEMs exponham câmera de profundidade, infravermelho e em cores (e metadados relacionados) para visão do computador em UWP e designem uma câmera como participando da autenticação de face do Windows Hello. O namespace <a href="http://go.microsoft.com/fwlink/?LinkId=691697">Windows.Devices.Perception</a> contém as APIs de cliente que permitem que um aplicativo UWP acesse os dados em cores, profundidade ou em infravermelho das câmeras de visão do computador.
                                    
## Nova API de jogos

Use a nova classe Windows.Gaming.UI.GameBar para receber notificações quando a barra de jogo é mostrada ou ignorada.    
                            
                                    
## APIs Bluetooth
                                    
Várias APIs foram adicionadas e atualizadas para estender o suporte para Bluetooth LE, enumeração do dispositivo e outros recursos em Bluetooth. Consulte o namespace <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx">Windows.Devices.Bluetooth</a>.    
                                   
## APIs de cartão inteligente ## 

Várias APIs SmartCardCryptogram foram adicionadas para o namespace <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx">Windows.Devices.SmartCards</a> para dar suporte a protocolos de pagamento criptografados seguros. Os aplicativos de pagamento usam a emulação de cartão de host para dar suporte ao encostar para pagar possa usar essas APIs para desempenho e segurança adicionais. Os aplicativos podem criar uma chave e proteger chaves de transação de uso limitado usando o TPM. Os aplicativos também podem aproveitar a estrutura NGC (Credenciais de próxima geração) para proteger as chaves com o PIN do usuário. Essas APIs delegam a geração de criptogramas para o sistema para melhorar o desempenho. Isso também impede qualquer acesso às chaves e criptogramas por outros aplicativos.    
                                    
## APIs de armazenamento atualizado ## 
    
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx">Classe Windows.Storage.DownloadsFolder</a><br />
Seu aplicativo agora pode <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx">criar um arquivo</a> ou <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx">criar uma pasta</a> dentro da pasta de Downloads para um determinado <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Usuário</a>.
                                            
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx">Classe Windows.Storage.StorageLibrary</a><br />
Seu aplicativo agora pode <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx">obter uma biblioteca especificada</a> para um determinado <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Usuário</a>.
                                    
## Kit de Certificação de Aplicativos Windows ## 
                                    
O Kit de Certificação de Aplicativos Windows foi atualizado com testes aprimorados. Para obter uma lista completa de atualizações, visite a página do <a href="/develop/app-certification-kit">Kit de Certificação de Aplicativos Windows</a>.    
                                    
## Downloads de design ## 

Confira nossos novos modelos de design de aplicativo UWP para Adobe Photoshop. Nós também atualizamos nossos modelos do Microsoft PowerPoint e Adobe Illustrator e disponibilizamos uma versão em PDF de nossas diretrizes. <a href="/design/assets">Visite a página de Downloads de design</a>    




<!--HONumber=Mar16_HO5-->


