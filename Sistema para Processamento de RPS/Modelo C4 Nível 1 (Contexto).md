# C4 Model - Context Diagram (Level 1)

## Sistema: Processamento do RPS

### Visão Geral
O diagrama de contexto abaixo descreve os principais componentes envolvidos no processo de **Processamento do RPS (Recibo Provisório de Serviço)**. Ele mostra como as diferentes partes do sistema interagem entre si.

### Objetivo
O objetivo deste modelo é fornecer uma visão de alto nível do fluxo de processamento do RPS, destacando os componentes e a comunicação entre eles.

### Componentes Principais

1. **Job**
   - **Descrição**: Responsável por buscar os RPS na fila e iniciar o processamento.
   - **Tecnologia**: Scheduler

2. **Declaração**
   - **Descrição**: Responsável por gerar a nota a partir dos dados do RPS e também por processar os lançamentos.
   - **Tecnologia**: Service

3. **Service Bus**
   - **Descrição**: Armazena as notas geradas na fila de lançamento para que possam ser processadas posteriormente.
   - **Tecnologia**: Message Queue

### Relacionamentos

1. **Job ➡️ Declaração**
   - **Descrição**: O Job inicia o processo e envia o RPS para a Declaração, para que a nota possa ser gerada.

2. **Declaração ➡️ Service Bus**
   - **Descrição**: A Declaração salva a nota gerada na fila de lançamento do Service Bus.

3. **Service Bus ➡️ Declaração**
   - **Descrição**: O Service Bus envia os lançamentos armazenados de volta para a Declaração, que processa os lançamentos do Service Bus.

### Diagrama

```mermaid
graph LR
  Job["Job"] --> Declaração["Declaração"]
  Declaração --> ServiceBus["Service Bus"]
  ServiceBus --> Declaração
