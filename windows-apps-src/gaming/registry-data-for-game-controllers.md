---
title: Dados do Registro para controladores de jogo
description: Saiba mais sobre os dados que você pode adicionar ao Registro do computador para permitir que o controlador seja usado em jogos UWP.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 06/25/2018
ms.topic: article
keywords: windows 10, uwp, jogos, entrada, registro, personalizado
ms.localizationpriority: medium
ms.openlocfilehash: 3d30c19a7fd7641d76e810912d33a96dbbeb3132
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8345970"
---
# <a name="registry-data-for-game-controllers"></a>Dados do Registro para controladores de jogo

> [!NOTE]
> Este tópico destina-se aos fabricantes de controladores de jogos compatíveis com o Windows 10 e não se aplica à maioria dos desenvolvedores.

O [namespace Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) permite que fornecedores independentes de hardware (IHVs) adicionem dados ao Registro do computador, permitindo que seus dispositivos apareçam como [Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad), [RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick) e [UINavigationControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller) conforme apropriado. Todos os IHVs devem adicionar esses dados aos seus controladores compatíveis. Ao fazer isso, todos os jogos UWP (e jogos da área de trabalho que usam a API do WinRT) poderão dar suporte ao seu controlador de jogo.

## <a name="mapping-scheme"></a>Esquema de mapeamento

Os mapeamentos para um dispositivo com ID de Fornecedor (VID) **VVVV**, ID do Produto (PID) **PPPP**, Página de Uso **UUUU** e ID de Uso **XXXX** serão lidas partir deste local no Registro:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

A tabela a seguir explica os valores esperados no local raiz do dispositivo:

<table>
    <tr>
        <th>Nome</th>
        <th>Tipo</th>
        <th>Necessário?</th>
        <th>Informações</th>
    </tr>
    <tr>
        <td>Desabilitado</td>
        <td>DWORD</td>
        <td>Não</td>
        <td>
            <p>Indica que esse dispositivo específico deve ser desabilitado.</p>
            <ul>
                <li><b>0</b>: o dispositivo não está desabilitado.</li>
                <li><b>1</b>: o dispositivo está desabilitado.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Descrição</td>
        <td>REG_SZ <td>Não</td>
        <td>Uma descrição breve do dispositivo.</td>
    </tr>
</table>

O instalador do seu dispositivo deve adicionar esses dados ao Registro (por meio de configuração ou de um [arquivo INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)).

As subchaves sob o local raiz do dispositivo são detalhadas nas seções a seguir.

### <a name="gamepad"></a>Gamepad

A tabela abaixo lista as subchaves necessárias e opcionais sob a subchave **Gamepad**:

<table>
    <tr>
        <th>Subchave</th>
        <th>Necessário?</th>
        <th>Informações</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>Sim</td>
        <td rowspan="18" style="vertical-align: middle;">Consulte <a href="#button-mapping">Mapeamento de botões</a></td>
    </tr>
    <tr>
        <td>View</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>A</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>B</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>X</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Y</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>Sim</td>
        <td rowspan="6" style="vertical-align: middle;">Consulte <a href="#axis-mapping">Mapeamento de eixos</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>Sim</td>
    </tr>
</table>

> [!NOTE]
> Se você adicionar seu controlador de jogo como um **Gamepad** compatível, é altamente recomendável também adicioná-lo como um **UINavigationController** compatível.

### <a name="racingwheel"></a>RacingWheel

A tabela abaixo lista as subchaves necessárias e opcionais sob a subchave **RacingWheel**:

<table>
    <tr>
        <th>Subchave</th>
        <th>Necessário?</th>
        <th>Informações</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>Sim</td>
        <td rowspan="30" style="vertical-align: middle;">Consulte <a href="#button-mapping">Mapeamento de botões</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Wheel</td>
        <td>Sim</td>
        <td rowspan="5" style="vertical-align: middle;">Consulte <a href="#axis-mapping">Mapeamento de eixos</a></td>
    </tr>
    <tr>
        <td>Throttle</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Brake</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Clutch</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>Sim</td>
        <td>Consulte <a href="#properties-mapping">Mapeamento de propriedades</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

A tabela abaixo lista as subchaves necessárias e opcionais sob a subchave **ArcadeStick**:

<table>
    <tr>
        <th>Subchave</th>
        <th>Necessário?</th>
        <th>Informações</th>
    </tr>
    <tr>
        <td>Action 1</td>
        <td>Sim</td>
        <td rowspan="12" style="vertical-align: middle;">Consulte <a href="#button-mapping">Mapeamento de botões</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>Sim</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

A tabela abaixo lista as subchaves necessárias e opcionais sob a subchave **FlightStick**:

<table>
    <tr>
        <th>Subchave</th>
        <th>Necessário?</th>
        <th>Informações</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>Sim</td>
        <td rowspan="2" style="vertical-align: middle;">Consulte <a href="#button-mapping">Mapeamento de botões</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Roll</td>
        <td>Sim</td>
        <td rowspan="4" style="vertical-align: middle;">Consulte <a href="#axis-mapping">Mapeamento de eixos</a></td>
    </tr>
    <tr>
        <td>Pitch</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Yaw</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Throttle</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>Sim</td>
        <td>Consulte <a href="#switch-mapping">Mapeamento de comutadores</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

A tabela abaixo lista as subchaves necessárias e opcionais sob a subchave **UINavigation**:

<table>
    <tr>
        <th>Subchave</th>
        <th>Necessário?</th>
        <th>Informações</th>
    </tr>
    <tr>
        <td>Menu</td>
        <td>Sim</td>
        <td rowspan="24" style="vertical-align: middle;">Consulte <a href="#button-mapping">Mapeamento de botões</a></td>
    </tr>
    <tr>
        <td>View</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Accept</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Cancel</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>Sim</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>PageUp</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>PageDown</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>Não</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>Não</td>
    </tr>
</table>

Para saber mais sobre os controladores de navegação da interface do usuário e os comandos acima, consulte [Controlador de navegação da interface do usuário](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller).

## <a name="keys"></a>Chaves

As seções a seguir explicam o conteúdo de cada uma das subchaves sob as chaves **Gamepad**, **RacingWheel**, **ArcadeStick**, **FlightStick** e **UINavigation**.

### <a name="button-mapping"></a>Mapeamento de botões

A tabela a seguir lista os valores que são necessários para mapear um botão. Por exemplo, se pressionar **DPadUp** no controlador de jogo, o mapeamento para **DPadUp** deverá conter o valor **ButtonIndex** (a **Origem** é **Botão**). Se **DPadUp** precisar ser mapeado de uma posição do comutador, o mapeamento de **DPadUp** deverá conter os valores **SwitchIndex** e **SwitchPosition** (a **Origem** é **Comutador**).

<table>
    <tr>
        <th>Origem</th>
        <th>Nome do valor</th>
        <th>Tipo de valor</th>
        <th>Necessário?</th>
        <th>Informações do valor</th>
    </tr>
    <tr>
        <td>Botão</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>Índice na matriz de botões <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Eixo</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>Índice na matriz de eixos <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>Não</td>
        <td>Indica que o valor do eixo deverá ser invertido antes de os fatores <b>ThresholdPercent</b> e <b>DebouncePercent</b> serem aplicados.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>Indica a posição do eixo na qual o valor do botão mapeado faz a transição entre os estados pressionado e liberado. O intervalo de valores válidos é de 0 a 100. O botão é considerado pressionado se o valor do eixo for maior ou igual a esse valor.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>
            <p>Define o tamanho de uma janela em torno do valor <b>ThresholdPercent</b>, que é usado para enumerar o estado do botão relatado. O intervalo de valores válidos é de 0 a 100. As transições de estado do botão só podem ocorrer quando o valor do eixo ultrapassa o limite superior ou inferior da janela de enumeração. Por exemplo, um <b>ThresholdPercent</b> de 50 e <b>DebouncePercent</b> de 10 resultam nos limites de enumeração em 45% e 55% dos valores de eixo do intervalo inteiro. O botão não pode fazer a transição para o estado pressionado até o valor do eixo atingir 55% ou mais e não pode voltar ao estado liberado até o valor do eixo atingir 45% ou menos.</p>
            <p>Os limites da janela de enumeração computados são vinculados entre 0% e 100%. Por exemplo, um limite de 5% e uma janela de enumeração de 20% resultariam na queda dos limites da janela de enumeração para 0% e 15%. O estado do botão para os valores de eixo de 0% e 100% sempre é relatado como liberado e pressionado, respectivamente, independentemente dos valores de limite e enumeração.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Comutador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>Índice na matriz de comutadores <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>Sim</td>
        <td>
            <p>Indica a posição do comutador que fará com que o botão mapeado relate que está sendo pressionado. Os valores de posição podem ser uma destas cadeias de caracteres:</p>
            <ul>
                <li>Up</li> 
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Não</td>
        <td>Indica que as posições do comutador adjacentes também farão com que o botão mapeado relate que está sendo pressionado.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Mapeamento de eixos

A tabela a seguir lista os valores que são necessários para mapear um eixo:

<table>
    <tr>
        <th>Origem</th>
        <th>Nome do valor</th>
        <th>Tipo de valor</th>
        <th>Necessário?</th>
        <th>Informações do valor</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Botão</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>
            <p>Índice na matriz de botões <b>RawGameController</b> que é convertido no valor do eixo unidirecional mapeado.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>Não</td>
        <td>
            <p>Indica que o eixo mapeado é bidirecional. Os valores de <b>MaxButton</b> e <b>MinButton</b> são combinados em um eixo único bidirecional, conforme mostrado abaixo.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Eixo</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>Índice na matriz de eixos <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>Não</td>
        <td>Indica que o valor do eixo mapeado deverá ser invertido antes de ser retornado.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Comutador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Sim</td>
        <td>Índice na matriz de comutadores <b>RawGameController</b>.
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>Sim</td>
        <td>
            <p>Uma das seguintes cadeias de caracteres:</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>Isso indica a posição do comutador que faz com que o valor do eixo mapeado seja relatados como 1.0. A direção oposta de <b>MaxValueSwitchPosition</b> é tratado como 0.0. Por exemplo, se <b>MaxValueSwitchPosition</b> for <b>Up</b>, a conversão do valor do eixo é mostrada abaixo:</p>
            <table>
                <tr>
                    <th>Posição do comutador</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Não</td>
        <td>
            <p>Indica que as posições do comutador adjacentes também farão com que o eixo mapeado relate 1.0. No exemplo acima, se <b>IncludeAdjacent</b> for definido, a conversão do eixo será feita da seguinte maneira:</p>
            <table>
                <tr>
                    <th>Posição do comutador</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Mapeamento de comutadores

As posições do comutador podem ser mapeadas a partir de um conjunto de botões na matriz de botões do **RawGameController** ou de um índice na matriz de comutadores. As posições dos comutadores não podem ser mapeadas a partir dos eixos.

<table>
    <tr>
        <th>Origem</th>
        <th>Nome do valor</th>
        <th>Tipo de valor</th>
        <th>Informações do valor</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Botão</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 ou 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> ou <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Consulte <a href="#buttonindex-values">*Valores de ButtonIndex</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Eixo</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> ou <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> está sempre presente. <b>XAxisIndex</b> só estará presente quando <b>SwitchKind</b> for <b>FourWay</b> ou <b>EightWay</b>.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indica o tamanho da zona morta ao redor da posição central dos eixos.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Defina o tamanho das janelas ao redor dos limites superior e inferior da zona morta, que são usados para enumerar o estado relatado do comutador.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indica que os valores dos eixos correspondentes devem ser invertidos antes de os cálculos da zona morta e da janela de enumeração serem aplicados.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Comutador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Índice na matriz de comutadores <b>RawGameController</b>.
    </tr>
    <tr>
        <td>Invert</td>
        <td>DWORD</td>
        <td>Indica que o comutador relata suas posições na ordem no sentido anti-horário, em vez da ordem no sentido horário padrão.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>Muda o ponto de partida de como posições são relatadas pelo valor especificado. <b>PositionBias</b> é sempre contabilizado no sentido horário do ponto de partida original e é aplicado antes que a ordem dos valores seja invertida.</p>
            <p>Por exemplo, um comutador que relata posições a partir de <b>DownRight</b> no sentido anti-horário pode ser normalizado definindo o sinalizador <b>Invert</b> e especificando um <b>PositionBias</b> de 5:</p>
            <table>
                <tr>
                    <th>Posição</th>
                    <th>Valor relatado</th>
                    <th>Após os sinalizadores PositionBias e Invert</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>*Valores de ButtonIndex

Índice de valores de \*ButtonIndex na matriz de botões do **RawGameController**:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>Mapeamento de propriedades

Estes são os valores de mapeamento estático para diferentes tipos de mapeamento.

<table>
    <tr>
        <th>Mapeamento</th>
        <th>Nome do valor</th>
        <th>Tipo de valor</th>
        <th>Informações do valor</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>Indica o ângulo máximo da roda física aceito pela roda em uma única direção. Por exemplo, uma roda com uma rotação possível de -90 graus a 90 graus especificaria 90.</td>
    </tr>
</table>

## <a name="labels"></a>Rótulos

Rótulos devem estar presentes sob a chave **Labels** na raiz do dispositivo. **Labels** pode ter 3 subchaves: **Buttons**, **Axes** e **Switches**.

### <a name="button-labels"></a>Rótulos de botão

A chave **Buttons** mapeia cada uma das posições dos botões na matriz de botões do **RawGameController** para uma cadeia de caracteres. Cada cadeia de caracteres é mapeada internamente para o valor de enumeração [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) correspondente. Por exemplo, se um gamepad tiver dez botões, a ordem em que o **RawGameController** analisa os botões e os apresenta no relatório de botões será esta:

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

Os rótulos devem aparecer nesta ordem sob a chave **Buttons**:

<table>
    <tr>
        <th>Nome</th>
        <th>Valor (tipo: REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>Menu</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>View</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>Rótulos de eixo

A chave **Axes** mapeará cada uma das posições do eixo na matriz de eixos do **RawGameController** para um dos rótulos listados no [enumerador GameControllerButtonLabel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) assim como os rótulos de botão. Consulte o exemplo em [Rótulos de botão](#button-labels).

### <a name="switch-labels"></a>Rótulos de comutador

A chave **Switches** mapeia as posições do comutador para rótulos. Os valores seguem esta convenção de nomenclatura: para rotular uma posição de um comutador, cujo índice é *x* na matriz de comutadores do **RawGameController**, adicione estes valores na subchave **Switches**: 

* SwitchxUp 
* SwitchxUpRight 
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

A tabela a seguir mostra um conjunto de exemplo de rótulos para posições de um comutador de 4 direções que aparece no índice 0 no **RawGameController**: 

<table>
    <tr>
        <th>Nome</th>
        <th>Valor (tipo: REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>Exemplo de um arquivo do Registro

Para mostrar como todos esses mapeamentos e valores se encaixam, aqui está um exemplo de arquivo do Registro para uma **RacingWheel** genérica:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>Veja também

* [Namespace Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Namespace Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)
* [Arquivos INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)