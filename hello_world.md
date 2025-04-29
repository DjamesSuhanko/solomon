# Definições da placa
Essa parte da documentação deverá ser incrementada constantemente, conforme a placa for estudada. É fundamental compreender os conceitos utilizados pela Renesas, pois não há uma API para fazer abstração dos recursos de hardware,
tal qual há para placas que comportam a API do Arduino ou a API da Espressif (para ESP32, por exemplo).

## Renesas RH850
As MCUs dessa série são voltadas para aplicações industriais/automotivas, de modo que o controle deve estar absolutamente sob o controle do desenvolvedor. Nesse caso, é fundamental **estudar o datasheet para
compreender os recursos e suas configurações**. Inicialmente, podemos fazer um blink, que é o famoso "Hello World" da eletrônica digital. Para isso, vamos compreender primeiramente como configurar um pino
como GPIO.

### Registradores
Os pinos são controlados por múltiplos registradores em qualquer MCU, porém (como já citado), algumas delas oferecem uma API de alto nível que abstrai as configurações.
Os registradores principais que utilizaremos para o blink serão o de **nível lógico, modo e direção**.

