---
author: Mtoepke
title: Recursos do sistema para aplicativos UWP e jogos no Xbox One
description: UWP em recursos do sistema do Xbox
area: Xbox
---

# Recursos do sistema para aplicativos UWP e jogos no Xbox One

Aplicativos UWP e jogos em execução no Xbox One compartilham recursos com o sistema e outros aplicativos. 
Portanto, jogos e aplicativos UWP terão acesso os seguintes recursos:

* Nesta visualização, o máximo de memória disponível é 448 MB.
    * Em versões futuras, o máximo de memória disponível será 1 GB.
    * Ao executar seu aplicativo ou jogo do depurador do Visual Studio, não se aplicam essas restrições de memória. Esse limite é aplicável apenas quando não está em execução no modo de depuração.

* Compartilhamento de 2 a 4 núcleos de CPU dependendo do número de aplicativos e jogos executados no sistema.

* Compartilhamento de 45% de GPU dependendo do número de aplicativos e jogos executados no sistema.

* O UWP no Xbox One dá suporte ao DirectX 11 Feature Level 10. Não há suporte para o DirectX 12 no momento. 

Para **desenvolvimento de aplicativos**, é importante ter em mente que os recursos disponíveis podem ser limitados em comparação com um PC padrão.

Para **desenvolvimento de jogos**, é importante ter em mente que o Xbox One, como outros consoles de jogos, é um componente especializado de hardware que exige um kit de desenvolvimento baseado em hardware específico para acessar todo o seu potencial. 
Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, você pode se registrar no programa [ID@Xbox](http://www.xbox.com/en-us/Developers/id) para obter acesso aos kits de desenvolvimento do Xbox One, que incluem suporte ao DirectX 12.


<!--HONumber=May16_HO2-->


