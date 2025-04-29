# Definições da placa
Essa parte da documentação deverá ser incrementada constantemente, conforme a placa for estudada. É fundamental compreender os conceitos utilizados pela Renesas, pois não há uma API para fazer abstração dos recursos de hardware,
tal qual há para placas que comportam a API do Arduino ou a API da Espressif (para ESP32, por exemplo).

[Documento de desenvolvimento](https://github.com/user-attachments/files/19950791/RH850.pdf)

Outros documentos podem ser [baixados diretamente do site da Renesas](https://www.renesas.com/en/software-tool/cs#documents), após criar uma conta.



## Renesas RH850
As MCUs dessa série são voltadas para aplicações industriais/automotivas, de modo que o controle deve estar absolutamente sob o controle do desenvolvedor. Nesse caso, é fundamental **estudar o datasheet para compreender os recursos e suas configurações**. Inicialmente, podemos fazer um blink, que é o famoso "Hello World" da eletrônica digital. Para isso, vamos compreender primeiramente como configurar um pino como GPIO.

### Registradores
Os pinos são controlados por múltiplos registradores em qualquer MCU, porém (como já citado), algumas delas oferecem uma API de alto nível que abstrai as configurações.
Os registradores principais que utilizaremos para o blink serão o de **nível lógico, modo e direção**.

As notações estão definidas no arquivo _iodefine.h_, gerador pelo compilador **CC-RH** para a **RH850/C1M-A2** e reflete a estrutura de hardware do microcontrolador. Essas definições abstraem um pouco
a estrutura de hardware, mas a contrapartida é que se torna complexo para iniciantes.

#### 3 aspectos gerais a serem configurados
O que se deve ter em mente é um conceito básico de qualquer MCU, bastando compreender a forma de expressar o conjunto de configurações para um pino. Lembre-se:
* ***Modo do pino*** - pode ser usado como GPIO ou função alternativa (para algum barramento serial, por exemplo (UART, CAN, SPI, I2C etc).
* ***Direção do pino*** - pode ser entrada ou saída (e nem sempre é óbvio, mas essa explicação fica para quando houver a necessidade de entendimento).
* Nível lógico - Normalmente "pullup" ou "pulldown". Usaremos para o controle do LED.

Na MCU em questão, esses aspectos são configurados por registradores específicos: **P0, PMC0 e PM0**, por exemplo.
Por sorte, o starter kit contém a descrição dos pinos impressos no acrílico, de modo a facilitar o entendimento da relação que faremos aqui. **Sempre** que desejar utilizar algum pino, observe primeiramente sobre a placa se o pino desejado não está dedicado a algum dos recursos já existentes. Claro que se o recurso não estiver em uso, o pino poderá ser reconfigurado para outro propósito.

#### Exemplo com o pino P0_0
Considerando o uso do pino **P0_0** para fazermos o blink, teremos então:
* P0.BIT.P0_0 bit do registrador de saída de dados **P0**, que controlará o nível lógico do pino **P0_0**; registrador **P0** no bit referente ao pino **P0_0**. 0 é **LOW** e 1 é **HIGH**.
Ligar ou desligar o LED dependerá da forma de alimentação. Adequadamente, catodo no GPIO e VCC no anodo, para os conservadores que não desejam drenar corrente do GPIO. Se usando o GPIO para alimentar o LED,
teríamos:
```
P0.BIT.P0_0 = 1; // Define P0_0 como alto (liga o LED, se anodo no GPIO)
P0.BIT.P0_0 = 0; // Define P0_0 como baixo (desliga o LED, se anodo no GPIO)
```

Em comparação às APIs de alto nível - por exemplo, a API do Arduino, a relação seria algo como:
| a | b |
|---|---|
| 1 | 2 |
