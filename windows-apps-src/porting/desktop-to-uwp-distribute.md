---
author: normesta
Description: Distribuir o app UWP convertido usando a ponte de Desktop para UWP
Search.Product: eADQiWindows 10XVcnh
title: "Distribuição da ponte de Desktop para UWP"
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.openlocfilehash: ee38bd22b6d4737cf5bb64eb489365e3f83efd53
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-distribute"></a>Ponte de Desktop para UWP: distribuição

Existem três maneiras principais de implantar o aplicativo convertido: a Windows Store, o sideload e o registro de arquivo flexível.  

## <a name="windows-store"></a>Windows Store

A Windows Store é a maneira mais prática dos clientes obterem o aplicativo. Para começar, preencha o formulário em [Trazer os aplicativos e jogos existentes para a Windows Store usando a ponte da área de trabalho](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). A Microsoft entrará em contato com você para iniciar o processo de integração.

Você precisa ser o desenvolvedor e/ou publicador do aplicativo ou do jogo a fim de levá-lo para a Windows Store. Assim, certifique-se de que o nome e o endereço de email correspondam aos do site que você envia como a URL abaixo, de maneira que possamos validar você como o desenvolvedor e/ou o publicador.

## <a name="sideloading"></a>Sideload

O sideload oferece uma maneira fácil de implantar em vários computadores. Isso é especialmente útil em cenários de empresa/linha de negócio (LOB) onde você queira um controle mais refinado sobre a experiência de distribuição e não deseja se envolver com o certificado da Loja.

Antes de implantar o aplicativo via sideload, você precisará assiná-lo usando um certificado. Para obter informações sobre a criação de um certificado, consulte [Assinar seu pacote de aplicativo do Windows](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx).

Veja a seguir como importar um certificado que você criou anteriormente. Você pode importar o certificado diretamente com o CERTUTIL, ou pode instalá-lo de um pacote de aplicativo do Windows que você assinou, como o cliente fará.

Para instalar o certificado via CERTUTIL, execute o seguinte comando em um prompt de comando de administrador:

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

Para importar o certificado do pacote de aplicativo do Windows como um cliente faria:

1.    No Explorador de Arquivos, clique com botão direito do mouse em um pacote de aplicativo do Windows que você assinou com um certificado de teste e escolha **Propriedades** no menu de contexto.
2.    Clique ou toque na guia **Assinaturas Digitais**.
3.    Clique ou toque no certificado e escolha **Detalhes**.
4.    Clique ou toque em **Exibir Certificado**.
5.    Clique ou toque em **Instalar Certificado**.
6.    No grupo **Local de Armazenamento**, selecione **Máquina Local**.
7.    Clique ou toque em **Avançar** e **OK** para confirmar a caixa de diálogo do UAC.
8.    Na tela seguinte do Assistente para importação de certificados, mude a opção selecionada para **Colocar todos os certificados no repositório a seguir**.
9.    Clique ou toque em **Procurar**. Na janela Selecionar Repositório de Certificados, role para baixo e selecione **Pessoas Confiáveis** e clique ou toque em **OK**.
10.    Clique ou toque em **Avançar**. Uma nova tela aparece. Clique ou toque em **Concluir**.
11.    Uma caixa de diálogo de confirmação deve aparecer. Em caso afirmativo, clique em **OK**. Caso apareça uma caixa diferente, isso significa que há um problema com o certificado. Talvez seja preciso solucionar esses problemas.

Observação: para o Windows confiar no certificado, o certificado deve estar no nó **Certificados (Computador Local) > Autoridades de Certificação Confiáveis > Certificados** ou no nó **Certificados (Computador Local) > Pessoas Confiáveis > Certificados**. Somente certificados nessas duas localizações podem validar a relação de confiança de certificado no contexto da máquina local. Caso contrário, aparece uma mensagem de erro que se parece com a seguinte cadeia de caracteres:

```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

Agora que o certificado foi marcado como confiável, há 2 maneiras de instalar o pacote, por meio do powershell ou clicando duas vezes no arquivo de pacote de aplicativo do Windows para instalá-lo.  Para instalar por meio do powershell, execute o seguinte cmdlet:

```powershell
Add-AppxPackage <MyApp>.appx
```

### <a name="loose-file-registration"></a>Registro de arquivo flexível

O registro de arquivo flexível é útil para fins onde os arquivos são dispostos em disco em um local que você pode acessar facilmente e atualização e não requer um certificado ou assinatura de depuração.  

Para implantar seu aplicativo durante o desenvolvimento, execute o seguinte cmdlet do PowerShell:

```Add-AppxPackage –Register AppxManifest.xml```

Para atualizar os arquivos .exe ou .dll do seu aplicativo, simplesmente substitua os arquivos existentes em seu pacote pelos novos, aumente o número de versão em AppxManifest.xml e, em seguida, execute o comando acima novamente.

Observe o seguinte:

* Qualquer unidade em que você instale seu aplicativo convertido deve ser formatada para o formato NTFS.
* Um aplicativo convertido sempre é executado como o usuário interativo.
