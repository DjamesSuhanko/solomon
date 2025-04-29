# Código para comunicação LIN
Não basta configurar os pinos e a comunicação; diversos outros fatores precisam ser configurados na placa como watchdog, timers, clock, interrupções etc. De fato, uma configuração
adequada demanda um período variável de aprendizado e especialização, especificamente sobre a arquitetura que for utilizada.

## Comunicação direta 
Para fazer a comunicação LIN é necessário compreender o protocolo. Um exemplo (não funcional, pois não foi experimentado) seria algo como:
```
#include <iodefine.h> // Definições de registradores do RH850/C1M-A2

// Função de delay simples (calibre para o clock do sistema)
void delay_ms(unsigned int ms) {
    unsigned int i;
    for (i = 0; i < ms * 1000; i++) {}
}

// Configura os pinos P4_12 (LIN30RX) e P4_13 (LIN30TX)
void configure_lin_pins(void) {
    // Configura P4_12 (LIN30RX) e P4_13 (LIN30TX) como função alternativa (LIN)
    PMC4.BIT.PMC4_12 = 1; // P4_12 como função alternativa (LIN30RX)
    PMC4.BIT.PMC4_13 = 1; // P4_13 como função alternativa (LIN30TX)
    
    // Configura direção: RX como entrada, TX como saída
    PM4.BIT.PM4_12 = 1;   // P4_12 como entrada (RX)
    PM4.BIT.PM4_13 = 0;   // P4_13 como saída (TX)
}

// Configura o módulo RLIN30 como mestre LIN (19200 bps)
void configure_rlin30_master(void) {
    // 1. Habilitar clock para RLIN30
    CPGSTBCR5.BIT.MSTP50 = 0; // Remove RLIN3 do estado de parada (ativa clock)

    // 2. Configurar modo LIN (mestre, UART mode para LIN)
    RLIN30.LWBR.BYTE = 0x00; // Desabilitar temporariamente para configuração
    RLIN30.LBRP0 = 0x00;     // Prescaler temporário
    RLIN30.LBRP1 = 0x00;     // Prescaler temporário

    // 3. Configurar baud rate (19200 bps, assumindo clock de 80 MHz)
    // Fórmula: Baud rate = PCLK / (Prescaler * Baud Rate Divisor)
    // Para 80 MHz, 19200 bps: Prescaler = 16, Divisor = 260
    RLIN30.LWBR.BIT.LPRS = 4; // Prescaler = 2^4 = 16
    RLIN30.LBRP0 = 260 & 0xFF; // Divisor baixo
    RLIN30.LBRP1 = (260 >> 8) & 0xFF; // Divisor alto
    RLIN30.LWBR.BIT.NSPB = 1; // 1 amostra por bit

    // 4. Configurar modo mestre LIN
    RLIN30.LMSTS.BYTE = 0x00; // Limpar status
    RLIN30.LCUC.BYTE = 0x01;  // Modo LIN (UART)
    RLIN30.LMD.BIT.LMD = 0x01; // Modo mestre

    // 5. Configurar formato do frame
    RLIN30.LDFC.BIT.LFMT = 0; // Checksum clássico
    RLIN30.LDFC.BIT.LBFC = 0; // 8 bytes de dados
    RLIN30.LDFC.BIT.LIDB = 0; // ID enviado pelo software

    // 6. Habilitar transmissão e recepção
    RLIN30.LWBR.BIT.LWBR0 = 1; // Habilitar LIN
    RLIN30.LUOR1.BIT.UTOE = 1; // Habilitar transmissão
    RLIN30.LUOR1.BIT.UROE = 1; // Habilitar recepção
}

// Envia um frame LIN como mestre (ID e dados)
void send_lin_frame(unsigned char id, unsigned char *data, unsigned char length) {
    // 1. Aguardar RLIN30 pronto
    while (RLIN30.LST.BIT.TRMSTS) {} // Espera transmissão anterior concluir

    // 2. Configurar ID
    RLIN30.LIDB.BYTE = id; // Define ID do frame (ex.: 0x01)

    // 3. Carregar dados (até 8 bytes)
    for (int i = 0; i < length; i++) {
        RLIN30.LDBR[i].BYTE = data[i]; // Carrega dados no buffer
    }

    // 4. Iniciar transmissão do header
    RLIN30.LTRC.BIT.TRS = 1; // Inicia transmissão do header

    // 5. Aguardar header enviado
    while (!RLIN30.LST.BIT.HTRSTS) {} // Espera header completo

    // 6. Iniciar transmissão dos dados
    RLIN30.LTRC.BIT.TRS = 1; // Inicia transmissão dos dados

    // 7. Aguardar frame completo
    while (!RLIN30.LST.BIT.FTCSTS) {} // Espera frame concluído
    RLIN30.LST.BIT.FTCSTS = 0; // Limpa flag
}

// Recebe resposta de um frame LIN
void receive_lin_response(unsigned char *data, unsigned char *length) {
    // 1. Aguardar recepção completa
    while (!RLIN30.LST.BIT.FRCSTS) {} // Espera recepção do frame

    // 2. Ler dados recebidos
    *length = 8; // Assume 8 bytes (conforme configuração)
    for (int i = 0; i < *length; i++) {
        data[i] = RLIN30.LDBR[i].BYTE; // Lê dados do buffer
    }

    // 3. Limpar flag de recepção
    RLIN30.LST.BIT.FRCSTS = 0;
}

int main(void) {
    // Dados de exemplo para enviar
    unsigned char tx_data[8] = {0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08};
    unsigned char rx_data[8];
    unsigned char rx_length;

    // 1. Configurar pinos P4_12 e P4_13
    configure_lin_pins();

    // 2. Configurar RLIN30 como mestre
    configure_rlin30_master();

    // 3. Loop principal
    while (1) {
        // Enviar frame LIN (ID 0x01, 8 bytes)
        send_lin_frame(0x01, tx_data, 8);

        // Aguardar e receber resposta (se houver)
        receive_lin_response(rx_data, &rx_length);

        // Pequeno delay para evitar sobrecarga
        delay_ms(100);
    }

    return 0;
}
```

Nesse código temos o tratamento inicial, relacionado à configuração dos pinos (sendo os pinos reais, impressos no chassi do starter kit); TX e RX no PMC4 (veja a respeito da configuração de pinos [aqui](hello_world.md)).
Definimos as direções (RX e TX, como entrada e saída, respectivamente). Após a configuração dos pinos, a ativação do módulo RLIN, com características específicas, como baud rate baseado no clock de 80MHz.
O módulo precisa ser configurado como Master ou Slave; além disso, checksum e habilitação da comunicação. Configuramos também o envio e recepção de resposta, envio de frame e o loop de comunicação com os slaves.

Essa é a comunicação mais simples possível para exemplificar, sem a adição de criptografia.

## Biblioteca
Pode haver mais de uma opção, mas esse é um estudo inicial. Considerando isso, existe uma opção que é a **RPDL** - ou **Renesas Peripheral Driver Library**. Trata-se de uma biblioteca oficial da Renesas projetada para 
facilitar o desenvolvimento em microcontroladores como RH850. Ela inclui funções prontas para os periféricos e de fato, poupa **MUITO TEMPO** em desenvolvimento, uma vez que as rotinas que haveriam de ser escritas
manualmente serão gerenciadas pela biblioteca. Para ter uma ideia, a configuração e execução ficaria em torno de `R_LIN_Init, R_LIN_Transmit` e `R_LIN_Receive`.

A contrapartida é que essa biblioteca é apenas bare-metal (ou seja, não funciona com RTOS). Além disso, ela não recebe mais atualização desde 2014, pois foi dado lugar ao [Smart Configurator](https://www.renesas.com/en/software-tool/cs?downloads-title-filter=Peripheral+Driver+Library&documents-title-filter=smart+configurator#documents). O link aponta para a documentação de migração do **e2** para o **CS+**, que é um PDF.

Há um treinamento da Renesas para o Smart Configurator em vídeo, [nesse link](https://www.renesas.com/en/software-tool/rh850-smart-configurator#tools_support).

[Clique para saber mais sobre o **Smart Configurator**](smartconfigurator.md)

TODO: continua com código gerado com Smart Configurator no CS+


