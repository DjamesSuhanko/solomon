# Smart Configurator for RH850
O Smart Configurator é uma ferramenta para gerar código de clock e funções. Ele pode ser usado para configurar projetos existentes ou novos projetos.
No site da Renesas estão disponíveis vídeos de treinamento do Smart Configurator, que você encontra [nesse link](https://www.renesas.com/en/software-tool/rh850-smart-configurator#videos_training).

## Configuração
Após fazer o download e instalação ([referenciado aqui](https://github.com/DjamesSuhanko/solomon/blob/main/hello_world.md#:~:text=Alternativa%20%C3%A0%20API%20do%20Arduino)), abra o programa e crie uma nova configuração, escolhendo a placa correspondente - no caso, a RH850/C1M-A2. A toolchain utilizada deve ser a **CCRH850**, que estaremos usando na IDE **CS+**.

![smartconf-01](https://github.com/user-attachments/assets/a64cdcb2-2e71-4895-9cb6-90cc0b6c70f3)


Dê um nome para a aplicação e clique em ***Finish*** (porque em ***Next*** tem apenas a opção de seleção de RTOS, mas que não é compatível com **CS+**).

