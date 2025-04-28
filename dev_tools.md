# SETUP DO AMBIENTE DE DESENVOLVIMENTO

## Download
Para as MCUs de 32 bits, o recomendado é utilizar a IDE **CS+**, que pode ser baixado direto do site da Renesas. É necessário registrar-se para fazer o download. Entre no site [renesas.com](https://www.renesas.com) para se cadastrar, faça a confirmação do cadastro que é enviada por email e então faça login em sua conta recém criada. Depois, [abra esse link](https://www.renesas.com/en/software-tool/cs). A versão a ser utilizada é a ***CC*** (*"CS+ for CC V8.13.00"*, na data de publicação dessa documentação).

## Setup
Feche todas as aplicações abertas no computador antes de prosseguir.
Tendo baixado o instalador, descomprima-o e execute-o. Instale o suporte à RH850, drivers e ferramentas. O processo será relativamente demorado, pois grande parte do conteúdo será baixado durante a instalação.
A instalação precisa ser assistida porque durante o processo será necessário confirmar alguns dos recursos que serão instalados.

Ao finalizar a instalação, será executado o _Update Manager_, se o mantiver marcado. Havendo update a fazer, siga conforme sugerido pela aplicação. Quando estiver concluído, abra a IDE **CS Plus**; vá em _File > New > New Project_. Para essa placa da qual foi gerada essa documentação, o modelo a selecionar é a **RH850/C1M-A2**. Desdobre o respectivo menu e selecione a MCU, conforme mostra a imagem a seguir.

TODO: colocar a imagem aqui

Nessa janela ainda se encontram informações como ***kind of project***, inicialmente marcado como **Boot Loader for Multi-core CC-RH)**. Escolha a opção **Application for Multi-core(CC-RH)**.

TODO: continua na ide; iniciar um teste de compilação; conectar debbuger/gravador; gravar um teste qualquer
