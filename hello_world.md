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

Em comparação às APIs de alto nível - por exemplo, a API do Arduino, a relação de _MODO_ e _DIREÇÃO_ seria algo como:
| Função | Arduino | Renesas |
|:-------|:-------:|--------:|
| Modo   | pinMode(pin,OUTPUT) | PMC0.BIT.PMC0_0 = 0 |
| Estado | digitalWrite(pin, HIGH) | P0.BIT.P0_0 = 1 |

Agora vamos a um exemplo de blink usando como exemplo o pino **P0_0**:

```
#include <iodefine.h> // Definições de registradores do RH850/C1M-A2

void delay_ms(unsigned int ms) {
    unsigned int i;
    for (i = 0; i < ms * 1000; i++) {} // Delay aproximado
}

int main(void) {
    // Configuração do pino P0_0 (equivalente a pinMode)
    PMC0.BIT.PMC0_0 = 0; // Modo GPIO
    PM0.BIT.PM0_0 = 0;   // Saída (OUTPUT)
    P0.BIT.P0_0 = 0;     // Inicialmente baixo (LOW)

    while (1) {
        P0.BIT.P0_0 = 1; // Liga LED (equivalente a digitalWrite HIGH)
        delay_ms(500);
        P0.BIT.P0_0 = 0; // Desliga LED (equivalente a digitalWrite LOW)
        delay_ms(500);
    }
    return 0;
}
```
Basicamente, usamos o **registrador de controle do modo de porta PMC (Port Mode Control)**; usamos o **registrador de direção dos pinos PM ("Pin Mode", para analogia com Arduino)**;  usamos o **registrador P0 (Port) para escrever o valor digital no pino (LOW ou HIGH).

| Registrador | Definição | Valor |
|:-------|:-------:|--------:|
| PMC   | Port Mode Control - =0 para GPIO ou 1 para uso com algum protocolo de comunicação  | PMC0.BIT.PMC0_0 = 0 |
| PM | Port/Pin Mode =0 para OUTPUT e =1 para INPUT | PM0.BIT.PM0_0 = 0 |
| P | Port/Pin - =0 para LOW e =1 para HIGH | PM0.BIT.PM0_0 = 0 |

A relação com Arduino, para quem tiver algum conceito básico:
![tabela-pins](https://github.com/user-attachments/assets/bdf14761-62c7-41bb-aaa5-01c717bbc5ed)

## Alternativa à API do Arduino
Como pôde ser visto, é mais fácil usar a API do Arduino do que ajustar os registradores de cada pino individualmente, como é padrão nas APIs da Renesas. Porém, ela própria oferece uma ferramenta para facilitar essa configuração, de modo semelhante à API do Arduino. Para a **RH850**, faça o download do [Smart Configurator nesse link](https://www.renesas.com/en/software-tool/rh850-smart-configurator#downloads).

O Smart Configurator funciona como plugin ou standalone. Gerar o código necessário é simples, mas requer algum estudo para a implementação. Dependendo dos critérios, pode
ser mais adequado criar manualmente as funções para manipulação dos pinos, definidos em _iodefine.h_. 

## Fóruns
Certamente será necessário atuar juntamente à comunidade para solução de percalços, adaptações, aprendizado e auxílio na resolução de problemas.

