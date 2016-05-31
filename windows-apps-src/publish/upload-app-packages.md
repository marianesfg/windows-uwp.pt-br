---
author: jnHs
Description: A página Pacotes é onde você carrega todos os arquivos de pacote (.xap, .appx, .appxupload e/ou .appxbundle) para o aplicativo que você está enviando. Você pode carregar pacotes para qualquer sistema operacional visado por seu aplicativo nesta etapa.
title: Carregue os pacotes do aplicativo
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
---

# Carregue os pacotes do aplicativo


A página **Pacotes** é onde você carrega todos os arquivos de pacote (.xap, .appx, .appxupload e/ou .appxbundle) para o aplicativo que você está enviando. Você pode carregar pacotes para qualquer sistema operacional visado por seu aplicativo nesta etapa. Quando um cliente baixar seu aplicativo, a Loja examinará todos os pacotes disponíveis do aplicativo e fornecerá automaticamente para cada cliente o pacote que funciona melhor em seus dispositivos.

Para obter detalhes sobre o que um pacote inclui e como ele deve ser estruturado, veja [Requisitos do pacote do aplicativo](app-package-requirements.md). Você também vai querer saber [como os números de versão podem afetar quais pacotes são entregues a clientes específicos](package-version-numbering.md) e saber [como os pacotes são distribuídos para diferentes sistemas operacionais](guidance-for-app-package-management.md).

## Carregando pacotes para seu envio


Para carregar os pacotes, arraste-os para o campo de carregamento ou clique em para procurar os arquivos. A página **Pacotes** permitirá que você carregue arquivos .xap, .appx, .appxupload e/ou .appxbundle.

Caso tenha criado [pacotes de pré-lançamento](package-flights.md) para seu aplicativo, você verá uma lista suspensa com a opção para copiar pacotes de um dos pacotes de pré-lançamento. Selecione o pacote de pré-lançamento que tiver os pacotes que você deseja puxar. Em seguida, você pode selecionar qualquer um ou todos os seus pacotes para incluir nesse envio.

> **Importante** Para o Windows 10, você deve sempre carregar o arquivo .appxupload aqui e não o .appX nem o .appxbundle. Para obter mais informações sobre o empacotamento de aplicativos UWP da Loja, consulte [Empacotando aplicativos Universais do Windows para Windows 10](../packaging/packaging-uwp-apps.md).

Se detectarmos problemas com seus pacotes ao validá-los, você precisará remover o pacote, corrigir o problema e tentar carregá-lo novamente. Para saber mais, consulte [Corrigir erros de carregamento de pacote](resolve-package-upload-errors.md).

Você também pode ver avisos para que saiba quais questões podem causar problemas, mas não impedem que você continue o seu envio.

## Detalhes do pacote


Depois que os pacotes tiverem sido carregados com êxito, eles serão listados, agrupados por sistema operacional de destino. O nome, a versão e a arquitetura do pacote serão exibidos. Para obter mais informações, como os idiomas compatíveis, os recursos do aplicativo e o tamanho do arquivo de cada pacote, clique em **Detalhes**.

Se estiver usando a [Mediação de anúncios do Windows](../monetize/use-ad-mediation-to-maximize-revenue.md), você também verá um link para configurar a mediação de anúncios de cada pacote.

Se você precisar remover um pacote do envio, clique no link **Remover** na parte inferior da seção **Detalhes** de cada pacote.

## Removendo pacotes redundantes


Se detectarmos que um ou mais dos seus pacotes são redundantes, exibiremos um aviso sugerindo que você remova os pacotes redundantes deste envio. Geralmente, isso ocorre quando você já tiver carregado pacotes e agora está fornecendo pacotes com versões superiores que dão suporte ao mesmo conjunto de clientes. Neste caso, nenhum cliente jamais obteria o pacote redundante porque agora há um pacote melhor para dar suporte a esses clientes (com maior número de versão).

Quando detectarmos que você tem pacotes redundantes, forneceremos uma opção para remover todos os pacotes redundantes deste envio automaticamente. Você também pode remover pacotes individualmente deste envio, se preferir.

## Pacotes com Visual Studio Application Insights


É recomendável usar o [Application Insights do Visual Studio](http://go.microsoft.com/fwlink/?LinkId=615086) em seus pacotes (ou ativá-lo, marcando a caixa para "Mostrar telemetria no Centro de Desenvolvimento do Windows" ao criar seu pacote) para que possamos fornecer [detalhes de telemetria de uso do aplicativo](usage-report.md) a você. Se você não tiver configurado o Application Insights no Microsoft Visual Studio, quando detectarmos que um pacote o inclui, exibiremos uma mensagem confirmando que, enviando seu pacote, você concorda em habilitar a telemetria de uso do aplicativo sobre a sua conta de desenvolvedor. Você pode desabilitar a telemetria de uso do aplicativo a qualquer momento nas **Configurações da conta**.

 

 






<!--HONumber=May16_HO2-->


