---
author: normesta
Description: Teste seu aplicativo para Windows 10 S sem ter que instalar o Windows 10 S.
Search.Product: eADQiWindows 10XVcnh
title: Teste seu aplicativo do Windows para o Windows 10 S
ms.author: normesta
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 S, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: 52cd0a7cadbedc3a843d6ce21ba5b985cfef4db8
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="test-your-windows-app-for-windows-10-s"></a>Teste seu aplicativo do Windows para o Windows 10 S

Você pode testar seu aplicativo do Windows para verificar se ele funcionará corretamente em dispositivos que executam o Windows 10 S. Na verdade, se você planeja publicar seu app na Windows Store, deverá fazer isso porque é um requisito da loja. Para testar seu aplicativo, você pode aplicar uma política de integridade de código do Device Guard em um dispositivo que estiver executando o Windows 10 Pro. Esta política impõe as regras com as quais os aplicativos devem estar de acordo para serem executados no Windows 10 S.

> [!IMPORTANT]
>Recomendamos que você aplique essas políticas a uma máquina virtual, mas se você quiser aplicá-las à sua máquina local, certifique-se de revisar nossa orientação sobre as melhores práticas na seção "Próximo, instalar a política e reiniciar o sistema" deste tópico antes de aplicar uma política.

<span id="choose-policy" />
## <a name="first-download-the-policies-and-then-choose-one"></a>Primeiro, baixe as políticas e escolha uma

Baixe as políticas de integridade de código do Device Guard [aqui](https://go.microsoft.com/fwlink/?linkid=849018).

Em seguida, escolha o que faz mais sentido para você. Aqui está o resumo de cada política.

|Política |Imposição |Certificado de autenticação |Nome do arquivo |
|--|--|--|--|
|Política do modo de auditoria |Registra problemas / não bloqueia |Loja |SiPolicy_Audit.p7b |
|Política de modo de produção |Sim |Loja |SiPolicy_Enforced.p7b |
|Política de modo de produto com aplicativos auto-assinados |Sim |Certificado de teste do AppX  |SiPolicy_DevModeEx_Enforced.p7b |

Recomendamos que você comece com a política de modo de auditoria. Você pode revisar os registros de eventos de integridade do código e usar essas informações para ajudá-lo a fazer ajustes em seu aplicativo. Em seguida, aplique a política de modo de produção quando estiver pronto para testes finais.

Aqui está um pouco mais informações sobre cada política.

### <a name="audit-mode-policy"></a>Política do modo de auditoria
Com este modo, seu aplicativo é executado, mesmo que ele execute tarefas que não são suportadas no Windows 10 S. O Windows registra todos os executáveis que teriam sido bloqueados nos Registros de eventos de integridade do código.

Você pode encontrar esses logs ao abrir o **Visualizador de Eventos** e então navegar até este local: Logs de Aplicativo e Serviços -> Microsoft -> Windows -> CodeIntegrity -> Operacional.

![code-integrity-event-logs](images/desktop-to-uwp/code-integrity-logs.png)


#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(Opcional) Encontrar pontos de falha específicos na pilha de chamadas
Para encontrar pontos específicos na pilha de chamadas onde ocorrem problemas de bloqueio, adicione esta chave do Registro e depois [configure um ambiente de depuração do modo kernel](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging).

|Chave|Nome|Tipo|Valor|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>Política de modo de produção
Esta política impõe as regras de integridade do código que combinam o Windows 10 S para que você possa simular a execução no Windows 10 S. Essa é a política mais rigorosa e é excelente para o teste de produção final. Neste modo, seu aplicativo está sujeito às mesmas restrições que estarão sujeitas no dispositivo de um usuário. Para usar este modo, seu aplicativo deve ser assinado pela Windows Store.

### <a name="production-mode-policy-with-self-signed-apps"></a>Política de modo de produção com aplicativos auto-assinados
Este modo é semelhante à política de modo de produção, mas também permite que as coisas sejam executadas que são assinadas com o certificado de teste incluído no arquivo zip. Instale o arquivo PFX incluído na pasta **AppxTestRootAgency** desse arquivo zip. Em seguida, assine seu app com ele. Dessa forma, você pode iterar rapidamente sem exigir a assinatura da loja.

Como o nome do publicador do seu certificado deve corresponder ao nome do fornecedor do seu app, você precisará alterar temporariamente o valor do atributo **Publisher** do elemento **Identity** para "CN=Appx Test Root Agency Ex". Você pode alterar esse atributo para seu valor original depois de concluir os testes.

## <a name="next-install-the-policy-and-restart-your-system"></a>Em seguida, instale a política e reinicie o sistema

Recomendamos que você aplique essas políticas a uma máquina virtual porque essas políticas podem levar a falhas de inicialização. Isso ocorre porque essas políticas bloqueiam a execução do código que não foi assinado pela Windows Store, incluindo drivers.

Se você deseja aplicar essas políticas à sua máquina local, é melhor começar com a política de Modo de auditoria. Com esta política, você pode revisar os registros de eventos de integridade do código para garantir que nada crítico seja bloqueado em uma política forçada.

Quando estiver pronto para aplicar uma política, encontre o arquivo .P7B para a política que você escolheu, renomeie-o para **SIPolicy.P7B** e, em seguida, guarde esse arquivo para esse local no seu sistema: **C:\Windows\System32\CodeIntegrity\**.

Em seguida, reinicie o sistema.

## <a name="next-steps"></a>Próximas etapas

**Examinar um artigo do blog detalhado lançado por nossa Equipe de Consultoria de App**

Consulte [Como portar e testar seus aplicativos da área de trabalho clássicos no Windows 10 S com a Ponte de Desktop](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/).

**Saiba mais sobre as ferramentas que facilitam os testes do Windows S**

Consulte [Descompactar, modificar, reempacotar, assinar um APPX](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/).

**Encontre respostas para dúvidas específicas**

Nossa equipe monitora estas [marcas do StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.