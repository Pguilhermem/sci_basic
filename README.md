Aqui está uma proposta de ficheiro `README.md` estruturado e detalhado para o seu projeto, escrito em Português:

---

# Projeto SCI Basic (Echo de Porta Série)

## Descrição do Projeto

Este projeto é um exemplo básico de comunicação série (UART) utilizando o periférico SCI (Serial Communications Interface) para os microcontroladores da família C2000 da Texas Instruments. O código funciona em modo "Echo" (Eco) com base em interrupções: qualquer caractere recebido pela placa através da porta série é imediatamente retransmitido de volta para o emissor.

## Hardware Suportado

* **Placa de Desenvolvimento:** LAUNCHXL-F28379D
* **Microcontrolador:** Família F2837xD (especificamente a variante F2837xD_337ZWT para a CPU1)

## Ferramentas e Dependências

* **IDE:** Code Composer Studio (CCS)
* **SDK:** C2000WARE versão 26.00.00.00
* **Ferramenta de Configuração:** SysConfig (ferramenta automática utilizada para gerar o código de inicialização do hardware)

## Configurações de Hardware (SysConfig)

O projeto usa a ferramenta SysConfig (`sci.syscfg`) para configurar a interface SCI. As configurações geradas são as seguintes:

* **Módulo SCI Atribuído:** SCIA
* **Pinos Utilizados:**
* **RX (Recepção):** GPIO43
* **TX (Transmissão):** GPIO42


* **Interrupções:** A interrupção de recepção FIFO (RXFF) está habilitada. O nível da FIFO de recepção (RX) está configurado para disparar a interrupção ao receber dados (`SCI_FIFO_RX1`).

## Estrutura do Código

O ficheiro principal `main.c` é responsável pelo fluxo do programa:

1. **Inicialização:** O código inicializa o dispositivo (`Device_init()`), o módulo de interrupções PIE (`Interrupt_initModule()`), a tabela de vetores de interrupção (`Interrupt_initVectorTable()`) e a placa com as definições do SysConfig (`Board_init()`).
2. **Ciclo Principal:** As interrupções globais e de tempo real são ativadas e o processador entra num ciclo infinito (`while(1)`), aguardando pela ocorrência de eventos.
3. **Rotina de Interrupção (ISR):** A função `INT_SCI0_RX_ISR` é ativada sempre que um dado chega à porta série. A rotina bloqueia até ler o caractere do FIFO (`SCI_readCharBlockingFIFO`) e envia esse mesmo caractere de volta para o FIFO de transmissão (`SCI_writeCharBlockingFIFO`). No final, limpa o estado da interrupção para estar pronta a receber o próximo caractere.

## Como Testar

1. Conecte a placa LAUNCHXL-F28379D ao computador via cabo USB.
2. Compile e grave o programa no microcontrolador usando o Code Composer Studio.
3. Abra um terminal de comunicação serial, como o terminal integrado do CCS (View > Console > Serial Console).
4. Configure o terminal para a porta COM correta criada pela placa (geralmente descrita como *XDS100v2 Class USB Serial Port*). Certifique-se de usar a Baud Rate configurada (por padrão costuma ser 115200 bps em projetos LaunchPad F28379D, com 8 bits de dados, sem paridade, 1 stop bit).
5. Escreva no terminal: os caracteres digitados deverão aparecer no ecrã como resposta enviada pelo microcontrolador.
