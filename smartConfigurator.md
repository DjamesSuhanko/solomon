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

#### EVDD
O EVDD (que é a base de operação) por exemplo, deve ser configurado conforme o projeto de hardware. Se já há um projeto, ele servirá como base para reprodução.

![evdd](https://github.com/user-attachments/assets/5fff99e5-05a5-47cc-8321-3784964b02ed)

#### System Clocks
Os clocks de sistema são o próximo passo, divididos em **clock principal de sistema** e ** **clock de subsistema**. Ao configurar oscilador e frequência, configura-se então os clocks dos multiplexadores. 

#### Components settings

Nessa aba se seleciona os componentes periféricos a habilitar. Normalmente as configurações podem ser mantidas como padrão, MAS, a UART (por exemplo) precisa ser configurada em duas etapas, mantendo os padrões como o nome, mas criando os componentes de **recepção (UART1, por exemplo)** e **transmissão (UART0, por exemplo)**. 

Esse descritivo é apenas para que se entenda que há um cuidado especial para cada recurso, que deve ser planejado e implementado conforme as especificações de projeto.

![components-settings](https://github.com/user-attachments/assets/7483dfad-8989-4928-9c3e-86e9e93d3add)

Após configurar o recurso, opções adicionais aparecerão na árvore de recurso. No caso da UART, todas as configuraç~eso como stop bit, paridade e baud rate estarão acessíveis na janela, facilitando a configuração do
recurso sem precisar escrever código manual para setup.


### Components
Para configurar os periféricos, como DMA, ADC, PWM, timers, UART etc. Detalhes da UART foram explicados no texto precedente.

### Pins
Configuração dos pinos e funções.
Tem mais de um modo de selecionar o pino. Nessa imagem, a seleção pode ser feita pelo recurso, então o respectivo pino recebe um quadrado vermelho na volta. A seleção do pino pode ser através da lista ou clicando com o botão direito sobre o pino. Repare que nesse caso se trata da seleção do número do pino, que é feita após selecionar o recurso:


![smartconf-05](https://github.com/user-attachments/assets/80a74665-6399-4c38-9eba-8fc606a74ca5)

A seleção pode ser feita também pela aba ***Pin Function***. Repare que (propositalmente) a seleção de RX está conflitante, pois ambos **TX** e **RX** estão apontando para o pino **4P** (não confunda com o registrador P4), que é o **pino 12 do registrador 4**. Aparece a mensagem de erro ao lado do título "Function", assim como no respectivo pino, ao lado direito da IDE do Smart Configurator. Esse é um excelente indicativo para recursos conflitantes! Para resolver, troque o pino de um dos dois. Na placa, o barramento LIN está fazendo uso de **P4_12** e **P4_13**. Buscando **P4_13** pelo filtro, a referência é o **Pin Number** designado como **3P**. Em suma:
* **4P** = P4_12
* **3P** = P4_13
Não havendo conflito, ainda faltará a configuração de software para o pino, como pode ser visto em ***Configuration Problems***:

![smartconf-07](https://github.com/user-attachments/assets/d08273c0-d227-4c88-b622-082353a2bdee)


Nesse modelo estão sendo mostrados 3 barramentos LIN, sendo 0 30, 31 e 31. As configurações foram feitas para o **LIN30**, como pode-se ver na imagem a seguir:

![smartconf-08](https://github.com/user-attachments/assets/ae087fd0-5fc0-4f13-b4df-b1095ecebffd)

## Assistente de desenvolvimento
Há uma árvore que auxilia na configuração e certamente será útil durante o desenvolvimento.


![dev-assist](https://github.com/user-attachments/assets/dd2c2be0-5feb-44a2-964a-03a8919b5513)


Arrastando o exemplo para o palco, o código do recurso (uma chamada de função, por exemplo) será inserida na posição em que foi arrastado. Em **Usage Example**, um exemplo completo de implementação do recurso será exibido,
incluindo comentários do recurso. 


![code_example](https://github.com/user-attachments/assets/9a64d186-a876-473d-a5ee-2c956984a2b4)

> O Smart Configurator pode ser acessado através de plugin, instalado no CS+. Ele aparecerá na árvore do projeto, porém abrirá uma janela à parte.

Do mesmo modo que na interface da IDE **CS+**, também vemos o _Smart Configurator_ no **e2studio**. Ambos apresentam semelhança na apresentação.

A interface apresenta um bom wizard, contendo uma aba chamada de "Smart Browser", com diversas orientações. Ao lado, uma aba de problemas de configuração. 
No console de saída são geradas as mensagens do compilador. Todas essas abas tem funções importantes e serão exploradas ao longo do estudo.

![ide-estudio-guiada](https://github.com/user-attachments/assets/e4e7c3eb-bd46-40fe-ac94-aa8d50c01760)

...continua...
