# ALIMENTAÇÃO

## 2.0 - Fonte de alimentação 12V
O starter kit usa uma alimentação externa de 12Vque atende a todos os requisitos de tensão do circuito. Nenhuma configuração adicional é necessária.
Após conectar a energia externa, os LEDS de **VDD** e **VBAT** se acenderão e o LED de **RESET** começará a piscar. O LED de RESET atua a cada reset e pode ser também
um indicador de reset por modo de operação inadequado. Fique atento, pois esse reinicio não deve acontecer espontaneamente.

## 2.1 - Comunicação com o computador
*Somente após a configuração dos jumpers e alimentação da placa*, conecte o cabo de dados à porta USB/USART e então ao computador. Na primeira vez que for conectada a
placa ao computador, será detectada a porta serial e o driver de dispositivo deverá ser instalado. Esse driver é meramente protocolo de comunicação serial, que
corresponde à controladora que se comunicará pela USB. Após a instalação, a porta aparecerá no gerenciador de dispositivos como uma porta **COM**. Para facilitar a
identificação do dispositivo, deixe aberto previamente o gerenciador de dispositivos com a aba de **Ports (COM e LPT)** expandida, então conecte o cabo USB ao computador.



