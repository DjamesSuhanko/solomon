# Smart Configurator for RH850
O Smart Configurator é uma ferramenta para gerar código de clock e funções. Ele pode ser usado para configurar projetos existentes ou novos projetos.
No site da Renesas estão disponíveis vídeos de treinamento do Smart Configurator, que você encontra [nesse link](https://www.renesas.com/en/software-tool/rh850-smart-configurator#videos_training).

## Configuração
Após fazer o download e instalação ([referenciado aqui](https://github.com/DjamesSuhanko/solomon/blob/main/hello_world.md#:~:text=Alternativa%20%C3%A0%20API%20do%20Arduino)), abra o programa e crie uma nova configuração, escolhendo a placa correspondente - no caso, a RH850/C1M-A2. A toolchain utilizada deve ser a **CCRH850**, que estaremos usando na IDE **CS+**.

![smartconf-01](https://github.com/user-attachments/assets/a64cdcb2-2e71-4895-9cb6-90cc0b6c70f3)


Dê um nome para a aplicação e clique em ***Finish*** (porque em ***Next*** tem apenas a opção de seleção de RTOS, mas que não é compatível com **CS+**).

Ao fazer isso, a janela principal do Smart Configurator ficará com essa aparência:

![smartconf-02](https://github.com/user-attachments/assets/a77f9c0f-c525-48c8-bbaa-2f3e16189c76)


Se a placa escolhida estiver errada, basta ir à aba ***Board** (próximo ao canto inferior esquerdo). Clicando em "**...**" será possível fazer a seleção do modelo correto.


![smartconf-03](https://github.com/user-attachments/assets/9a71b2a9-624a-413c-aebe-d25957235dd9)


O Smart Configurator tem algumas abas fundamentais para diversas configurações. Dentre elas:

### Overview
Essa é a aba inicial ao criar um novo projeto. Nela podemos notar que diversas camadas podem ser incluídas, olhando para o conjunto **Software Components**.
Essa aba inclui um overview das características, link para vídeos de introdução, link para verificar releases, documentação e FAQ. Dúvidas posteriores devem sempre ser consultadas na documentação.

### Board
Para mudar a seleção da placa ou modelo.

### Clocks
A configuração dos diversos clocks está disponível nessa aba. É necessário ler o datasheet e documentação do Starter Kit para saber como os clocks devem ser configurados.

### Components
Para configurar os periféricos, como DMA, ADC, PWM, timers, UART etc.

### Pins
Configuração dos pinos e funções.
Tem mais de um modo de selecionar o pino. Nessa imagem, a seleção pode ser feita pelo recurso, então o respectivo pino recebe um quadrado vermelho na volta. A seleção do pino pode ser através da lista ou clicando com o botão direito sobre o pino. Repare que nesse caso se trata da seleção do número do pino, que é feita após selecionar o recurso:


![smartconf-05](https://github.com/user-attachments/assets/80a74665-6399-4c38-9eba-8fc606a74ca5)

A seleção pode ser feita também pela aba ***Pin Funciont***. Repare que (propositalmente) a seleção de RX está conflitante, pois ambos **TX** e **RX** estão apontando para o pino **4P** (não confunda com o registrador P4), que é o **pino 12 do registrador 4**. Aparece a mensagem de erro ao lado do título "Function", assim como no respectivo pino, ao lado direito da IDE do Smart Configurator. Esse é um excelente indicativo para recursos conflitantes! Para resolver, troque o pino de um dos dois. Na placa, o barramento LIN está fazendo uso de **P4_12** e **P4_13**.
