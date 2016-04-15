---
title: Problemas conhecidos com a UWP no Xbox One Developer Preview
description: 
area: Xbox
---

# Problemas conhecidos com a UWP no Xbox One Developer Preview

A Atualização do Sistema do Xbox Developer Preview inclui software de pré-lançamento experimental e inicial. 
Isso significa que alguns aplicativos e jogos populares não funcionarão conforme o esperado, e você poderá experimentar falhas ocasionais e perda de dados. 
Se você sair da Developer Preview, o console será restaurado para as configurações de fábrica e você precisará reinstalar seus jogos, aplicativos e conteúdo.

Para os desenvolvedores, isso significa que nem todas as ferramentas de desenvolvedor e APIs funcionam como o esperado. 
Isso também significa que nem todos os recursos pretendidos para versão final estão incluídos ou estão com qualidade de versão. 
**Em particular, isso significa que o desempenho do sistema nesta visualização não reflete o desempenho do sistema da versão final.**

A lista a seguir destaca alguns problemas conhecidos que podem ocorrer nessa versão, embora essa não seja uma lista completa. 

**Queremos receber seu feedback**, portanto, relate todos os problemas que você encontrar no fórum [Developing Universal Windows apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Se você ficar paralisado, procure aqui e em [Perguntas frequentes](frequently-asked-questions.md)e use o fórum para solicitar ajuda.


## Desenvolvendo jogos

### x86 versus x64

No momento em que lançarmos posteriormente neste ano, teremos excelente suporte para x86 e x64 e oferecemos suporte ao x86 nesta visualização. 
No entanto, o x64 teve muito mais testes até a data (o shell do Xbox e todos os aplicativos em execução no console atualmente são x64) e, portanto, recomendamos usar o x64 para seus projetos. 
Isso é especialmente verdadeiro para jogos.

Se você decidir usar o x86, relate todos problemas que você vir no Fórum.

Consulte também [A mudança das configurações de build pode provocar falhas na implantação](known-issues.md#switching-build-flavors-can-cause-deployment-failures) posteriormente nesta página.

### Mecanismos de jogo

Testamos alguns mecanismos de jogos populares, mas não todos eles, e nossa cobertura de teste para esta visualização não foi abrangente. 
Sua milhagem pode variar. 
Adoraríamos receber seu feedback sobre o que você encontrar. 
Use o fórum para relatar todos os problemas que você vir.

### Suporte ao DirectX 12

A UWP no Xbox One dá suporte ao Nível de Recursos 10 do DirectX 11. 
Não há suporte para o DirectX 12 no momento. 
O Xbox One, como todos os consoles de jogos tradicionais, é um componente especializado de hardware que requer um SDK específico para acessar todo o seu potencial. 
Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, você pode se registrar no programa [ID@XBOX](http://www.xbox.com/en-us/Developers/id) para obter acesso a esse SDK, que inclui suporte ao DirectX 12.


## Recursos do sistema para aplicativos e jogos UWP no Xbox One

Os aplicativos e jogos UWP em execução no Xbox One compartilham recursos com o sistema e outros aplicativos e, portanto, o sistema governa os recursos que estão disponíveis para qualquer jogo ou aplicativo. 
Se você estiver encontrando problemas de memória ou de desempenho, esse poderá ser o motivo. 
Para obter mais detalhes, consulte [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md).


## Rede usando soquetes tradicionais

Nessa Developer Preview, o acesso à rede de entrada e de saída do console que usa soquetes TCP/UDP tradicionais (WinSock, Windows.Networking.Sockets) não está disponível. 
Os desenvolvedores ainda podem usar HTTP e WebSockets. 


## Cobertura de API UWP

Nem todas as APIs UWP funcionam conforme o esperado no Xbox nesta visualização. 
Consulte [Limitações da área de recursos da família de dispositivos UWP no Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755) para obter a lista de APIs que sabemos que não funcionam. 
Se você encontrar problemas com outras APIs, informe-os nos fóruns. 

## Os controles XAML não se parecem ou se comportam como os controles no shell do Xbox One

Nesta Developer Preview, os controles XAML não estão em sua forma final. Em particular:
* A navegação Gamepad X-Y não funciona de forma confiável para todos os controles.
* Os controles não se parecem com os controles no shell do Xbox. Isso inclui o retângulo de foco do controle.
* A navegação entre controles não faz "sons de navegação" automaticamente.

Esses problemas serão abordados em uma Developer Preview futura.

## Problemas do Visual Studio e de implantação

### Alternar tipos de compilação, o que pode provocar falhas na implantação

Alternar entre compilações de depuração e de versão ou entre x86 e x64 ou entre compilações gerenciadas e de .Net Native pode provocar falhas na implantação. 

A maneira mais simples de evitar esses problemas nesta visualização é aderir à depuração e a uma arquitetura. 

Se você encontrar esse problema, desinstalar seu aplicativo no aplicativo Coleções em seu Xbox One normalmente o resolverá.

> **Observação**&nbsp;&nbsp;Desinstalar seu aplicativo no Windows Device Portal (WDP) não resolverá o problema.

Se os problemas persistirem, desinstale seu aplicativo ou jogo no aplicativo Coleções, saia do Modo de Desenvolvedor, reinicie no Modo de Varejo e alterne para o Modo de Desenvolvedor.

Para obter mais informações, consulte a seção "Corrigindo falhas na implantação" em [Perguntas frequentes](frequently-asked-questions.md).

### Desinstalar um aplicativo enquanto você o estiver depurando no Visual Studio fará com que ele falhe silenciosamente

Tentar desinstalar um aplicativo que está sendo executado sob o depurador por meio da ferramenta "Aplicativos Instalados" do WDP fará com que ele falhe silenciosamente. 
A alternativa é parar a depuração do aplicativo no Visual Studio antes de tentar removê-lo por meio do WDP.

### Falhas de emparelhamento de PIN do Visual Studio/Xbox

É possível chegar a um estado em que o emparelhamento de PIN entre o Visual Studio e o Xbox One fica fora de sincronia. 
Se o emparelhamento de PIN falhar, use o botão "Remover todos os pares" na Dev Home, reinicie o Xbox One, reinicie seu computador de desenvolvimento e tente novamente. 


## Visualização do Windows Device Portal (WDP)

### Iniciar o WDP no Dev Home trava a Dev Home

Quando você inicia o WDP na Dev Home, isso faz com que a Dev Home falhe depois que você inseriu seu nome de usuário e senha e selecionou **Salvar**. 
As credenciais são salvas mas o WDP não é iniciado. 
Você pode iniciar o WDP reiniciando o Xbox One. 

### Desabilitar o WDP na Dev Home não funciona

Se você desabilitar o WDP na Dev Home, ela será desativada. 
No entanto, quando você reiniciar seu Xbox One, o WDP será iniciado novamente. 
Você pode contornar esse problema usando **Redefinir e manter meus aplicativos e jogos** para excluir qualquer estado armazenado em seu Xbox One. 
Vá para Configurações > Sistema > Informações e atualizações do console > Redefinir console e, em seguida, selecione o botão **Redefinir e manter meus aplicativos e jogos**.

> **Cuidado**&nbsp;&nbsp;Fazer isso excluirá todas as configurações salvas em seu Xbox One incluindo configurações sem fio, contas de usuário e qualquer progresso de jogo que não tenha sido salvo no armazenamento em nuvem.

> **Cuidado**&nbsp;&nbsp;NÃO selecione o botão **Redefinir e remover tudo**.
Isso irá excluir todos os jogos, aplicativos, configurações e conteúdo, desativar o Modo de Desenvolvedor e remover sua console do grupo Developer Preview.

### As colunas na tabela de "Aplicativos em Execução" não são atualizadas de forma previsível. 

Às vezes, isso é resolvido classificando uma coluna na tabela.

### A interface do usuário do WDP não é exibida corretamente no Internet Explorer 11 

Por padrão, a interface do usuário do WDP não é exibida corretamente no navegador ao usar o Internet Explorer 11. 
Você pode corrigir isso desativando o Modo de Exibição de Compatibilidade do Internet Explorer 11 para WDP.

### Navegar para o WDP provoca um aviso de certificado

Você receberá um aviso sobre o certificado que foi fornecido, semelhante à seguinte captura de tela, 
porque o certificado de segurança assinado por sua console do Xbox One não é considerado um publicador confiável conhecido. 
Clique em "Continuar para este site" para acessar o Windows Device Portal.

![Aviso de certificado de segurança do site](images/security_cert_warning.jpg)

## Dev Home
Ocasionalmente, selecionar a opção "Gerenciar o Windows Device Portal" na Dev Home faz com que a Dev Home saia da tela inicial silenciosamente. 
Isso é causado por uma falha na infraestrutura do WDP no console e pode ser resolvido reiniciando o console.

## Consulte também
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)


<!--HONumber=Mar16_HO5-->


