# SETUP DO AMBIENTE DE DESENVOLVIMENTO

## Download
Para as MCUs de 32 bits, o recomendado é utilizar a IDE **CS+**, que pode ser baixado direto do site da Renesas. É necessário registrar-se para fazer o download. Entre no site [renesas.com](https://www.renesas.com) para se cadastrar, faça a confirmação do cadastro que é enviada por email e então faça login em sua conta recém criada. Depois, [abra esse link](https://www.renesas.com/en/software-tool/cs). A versão a ser utilizada é a ***CC*** (*"CS+ for CC V8.13.00"*, na data de publicação dessa documentação).

## Setup
Feche todas as aplicações abertas no computador antes de prosseguir.
Tendo baixado o instalador, descomprima-o e execute-o. Instale o suporte à RH850, drivers e ferramentas. O processo será relativamente demorado, pois grande parte do conteúdo será baixado durante a instalação.
A instalação precisa ser assistida porque durante o processo será necessário confirmar alguns dos recursos que serão instalados.

Ao finalizar a instalação, será executado o _Update Manager_, se o mantiver marcado. Havendo update a fazer, siga conforme sugerido pela aplicação. Quando estiver concluído, abra a IDE **CS Plus**; vá em _File > New > New Project_. Para essa placa da qual foi gerada essa documentação, o modelo a selecionar é a **RH850/C1M-A2**. Desdobre o respectivo menu e selecione a MCU, conforme mostra a imagem a seguir.


![new_project-board_selection](https://github.com/user-attachments/assets/31eaa2ed-e5cb-4490-ad31-44c61d89abd5)

Nessa janela ainda se encontram informações como ***kind of project***, inicialmente marcado como **Boot Loader for Multi-core CC-RH)**. Escolha a opção **Application for Multi-core(CC-RH)**.

## Licença
A versão de avaliação do compilador é válido por 60 dias e certamente possui limitação no tamanho da aplicação compilada também.

## Teste inicial
Inclua o header _iodefine.h_ e clique em Build na IDE (fica ao lado direito do "100%" que aparece no menu superior). A compilação deve ocorrer sem erros e algo será exibido em _Output_, semelhante (ou igual) à imagem a seguir.
![purchase_compiler](https://github.com/user-attachments/assets/d5def4d4-7db8-455f-ba1f-53de7317565b)

Tendo chegado a esse ponto, siga para o item 4 da documentação.
