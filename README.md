# solomon
Documentação incremental (e corretiva) da implementação do protocolo LIN usando a placa de estudo Renesas RH850 (RL78).

## 1.0 - Jumping
A placa precisa ser previamente ajustada, conforme descrito na sequência a seguir. **NÃO LIGUE A PLACA ANTES DE CONFIGURAR OS JUMPERS E SWITCHES**.
Nesse vídeo **FAZER VIDEO E COLOCAR O LINK** os detalhes são claros.
**IMAGEM AQUI**

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
