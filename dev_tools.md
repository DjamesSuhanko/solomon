# SETUP DO AMBIENTE DE DESENVOLVIMENTO

## Download do CS+
Para as MCUs de 32 bits, uma das opções é utilizar a IDE **CS+**, que pode ser baixado direto do site da Renesas. É necessário registrar-se para fazer o download. Entre no site [renesas.com](https://www.renesas.com) para se cadastrar, faça a confirmação do cadastro que é enviada por email e então faça login em sua conta recém criada. Depois, [abra esse link](https://www.renesas.com/en/software-tool/cs). A versão a ser utilizada é a ***CC*** (*"CS+ for CC V8.13.00"*, na data de publicação dessa documentação).

No [FAQ do CS Plus](https://en-support.renesas.com/knowledgeBase/category/31407) tem referências importantes que serão usadas à posteriori.


## Setup
Feche todas as aplicações abertas no computador antes de prosseguir.
Tendo baixado o instalador, descomprima-o e execute-o. Instale o suporte à RH850, drivers e ferramentas. O processo será relativamente demorado, pois grande parte do conteúdo será baixado durante a instalação.
A instalação precisa ser assistida porque durante o processo será necessário confirmar alguns dos recursos que serão instalados.

Ao finalizar a instalação, será executado o _Update Manager_, se o mantiver marcado. Havendo update a fazer, siga conforme sugerido pela aplicação. Quando estiver concluído, abra a IDE **CS Plus**; vá em _File > New > New Project_. Para essa placa da qual foi gerada essa documentação, o modelo a selecionar é a **RH850/C1M-A2**. Desdobre o respectivo menu e selecione a MCU, conforme mostra a imagem a seguir.


![new_project-board_selection](https://github.com/user-attachments/assets/31eaa2ed-e5cb-4490-ad31-44c61d89abd5)

Nessa janela ainda se encontram informações como ***kind of project***, inicialmente marcado como **Boot Loader for Multi-core CC-RH)**. Escolha a opção **Application for Multi-core(CC-RH)**.

## Licença
A versão de avaliação do compilador é válido por 60 dias após o primeiro build. Todos os recusos estarão disponíveis durante o período de avaliação.
Após 60 dias, haverá uma limitação no tamanho do objeto para 256KB e as funções profissionais não estarão disponíveis.
Algumas otimizações estarão limitadas em modo de avaliação.

## Teste inicial
Inclua o header _iodefine.h_ e clique em Build na IDE (fica ao lado direito do "100%" que aparece no menu superior). A compilação deve ocorrer sem erros e algo será exibido em _Output_, semelhante (ou igual) à imagem a seguir.
![purchase_compiler](https://github.com/user-attachments/assets/d5def4d4-7db8-455f-ba1f-53de7317565b)

Tendo chegado a esse ponto, siga para o item 4 da documentação, caso tenha optado por utilizar o CS+.

## Download do e2studio
É necessário estar logado para fazer o donwload do e2studio, que pode ser encontrado [nesse link](https://www.renesas.com/en/software-tool/e2studio-information-rh850-family#downloads). Após baixar, descomprima o arquivo e execute o instalador. Ele possui versão para Windows e Linux.

O instalador sugere o modo **Lite**. A instalação foi feita no modo sugerido, para esse estudo.

O **e2studio** usa o **Eclipse** como editor.

Ambas as IDEs utilizam o toolchain **CC-RH**, permitindo apenas projetos em C, não em C++.

Ao iniciar a IDE, é necessário escolher a criação de um executável para _RH-850_ e escolher adequadamente a placa, como mostra a imagem a seguir.

![board-selection-e2studio](https://github.com/user-attachments/assets/3ed7685a-54e8-42f7-b0d8-b8826f05aab3)

Ambas as IDEs apresentam a mesma aparência, porém o **e2studio** evidencia mais o _Smart Configurator_.






