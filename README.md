# Solomon
Documentação incremental (e corretiva) da implementação do protocolo LIN usando a placa de estudo Renesas RH850 (RL78).

----------------
INDEX
----------------

1 - [Configuração de switches e jumpers (startup)](01-jumper_config.md)

## 1.0 - Jumping
A placa precisa ser previamente ajustada, conforme descrito na sequência a seguir. **NÃO LIGUE A PLACA ANTES DE CONFIGURAR OS JUMPERS E SWITCHES**.
Nesse vídeo **TODO: FAZER VIDEO E COLOCAR O LINK** os detalhes são claros.
**TODO: IMAGEM AQUI**

### 1.1 - LIN - CN15
Essa comunicação está localizada no canto superior esquerdo, olhando a placa frontalmente.
Os conectores correspondentes à comunicação LIN devem ser fechados. Repare na imagem que à direita estão descritos **RESET** e **LIN**. Coloque os jumpers na posição horizontal; um deles fechando P4_12 e o outro fechando P4_13.

### 1.2 - USB/USART - CN20
Essa comunicação está localizada à esquerda, próxima ao conector 12V.
Do mesmo modo que em LIN, feche o circuito dos pinos correspondentes à P5_5 e P5_4.

### 1.3 - CAN0 - CN21
Essa comunicação está localizada na base da placa, próximo ao centro da placa, entre **DEBUG** e **CAN1**.
Feche ambos os circuitos (P4_3 e P4_4). Repare no esquema impresso sobre a placa que eles estão na posição VERTICAL, portanto, tome o devido cuidado ao conectar esses dois jumpers.

### 1.4 CAN1 - CN22
Essa comunicação está localizada no canto inferior esquerdo da placa. As portas de **CAN0** e **CAN1** apontam para baixo. À esquerda de **CAN1** há um switch do watchdog, que se refere ao P4_1; e um pouco acima do switch do watchdog está o botão **SW2**, para reset, contendo um LED indicativo de estado.
Os jumpers de **CAN1** também devem ser fechados verticalmente.

### 1.5 - Watchdog
O switch de watchdog está localizado à esquerda de **CAN1**. Na configuração de jumpers está descrito apenas como "position 2-3". Seu estado inicial é **OFF** (TODO: verificar a implementação no código)

### 1.6 - Debug - SW3
Quando em 1 e 2, considere desligado. Olhando a placa frontalmente, 1 e 2 ficam para cima. Uma pequena parte branca do switch estará aparecendo na parte de baixo quando a posição for 1 e 2. Essa parte branca aparecerá em cima, quando os switches forem trocados para **MD** e **FL**.

### 1.7 - RSLV 0 - SW4
O SW4 está posicionado no canto superior direito da placa, na impressão de **RSLV 0**. Todos os switches devem ficar na posição dos números, assim como **CN6** não recebe nenhum jumper.

### 1.8 - SW6
O switch SW6 está localizado à esquerda do logo da Renesas, impresso na placa.
Na especificação da configuração o SW6 está descrito como "1,2:+" e "3,4:-". Porém os switches em estado inicial estão ao centro; sem indicação de "polaridade" e indicando **P5_0, P5_1, P5_2 e P5_**. Considerando a direção da impressão da porta e supondo que a relação "1,2,3,4" estejam diretamente apontando para "P5_0,...P5_3", a configuração desse switch fica com **P5_0 e P5_1** para baixo e **P5_2 e P5_3** para cima.

### 1.9 - SW7 e SW8
Esse switch fica ao lado dos jumpers do **CAN0**. O pinout no livreto descreve como "1,2:ON", porém **ON** e **CTS** estão gravados do lado oposto dos números. O mesmo para **SW8**, referente ao CAN1, cujo barramento está posicionado à esquerda de **CAN0**. Os switches estão posicionados em **ON e CTS** para ambos os barramentos CAN.

### 1.10 - SW10
Esse switch está acima dos jumpers da porta USB/USART. A configuração sugerida está com todos os pinos em ON. 
- O primeiro switch é para SPI.
- O segundo é para LED do usuário.
- O terceiro e quarto são interrupções, nos pinos P2_7 e P0_10, respectivamente. Ao lado esquerdo do switch há um botão de estado **SW9**, que gera a interrupção, colocando os pinos em GND.

### 1.11 - SW11
Esse é o maior switch, localizado juntamente à porta **LIN**. 
- A chave 5 não está conectada a nada.
- A chave 7 está relacionada ao **SENT**, que está *sem jumpers*, portanto, pode-se deixar desligado. A chave de **SENT** ligada requererá jumper em **P3_7** e **P3_8**, ao lado do switches.
- A chave 8 é **EVCC**.

### 1.12 - SW12
Sem maior descrição a configuração mínima sugere todos os switches de **SW12** em *OFF*, de modo que todos os pinos devem estar posicionados à esquerda.

