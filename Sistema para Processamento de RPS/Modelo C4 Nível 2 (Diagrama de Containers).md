# C4 Model - Container Diagram (Level 2)

## Sistema: Processamento do RPS

### Visão Geral
O diagrama de contêineres detalha os componentes e suas interações dentro do sistema de **Processamento do RPS (Recibo Provisório de Serviço)**. Ele descreve os principais contêineres de software que compõem o sistema, incluindo serviços, filas e outros contêineres necessários para a comunicação.

### Objetivo
Fornecer uma visão clara de como os componentes do sistema interagem e quais são suas responsabilidades dentro do processo de geração e lançamento de notas.

### Contêineres Principais

1. **Job Scheduler**
   - **Descrição**: Contêiner responsável por agendar e iniciar o processo de busca de RPS na fila.
   - **Tecnologia**: Cron Job / Task Scheduler
   - **Responsabilidade**: Buscar os RPS na fila de entrada e acionar o contêiner de geração de notas.

2. **Service Declaração**
   - **Descrição**: Serviço responsável por receber o RPS e gerar as notas fiscais.
   - **Tecnologia**: Microservice / API RESTful
   - **Responsabilidade**: Processar os dados do RPS e gerar uma nota fiscal válida, além de enviar os dados de nota para o Service Bus.

3. **Service Bus**
   - **Descrição**: Contêiner que atua como uma fila para armazenar notas fiscais geradas antes do processamento final.
   - **Tecnologia**: Azure Service Bus / Apache Kafka / RabbitMQ
   - **Responsabilidade**: Armazenar temporariamente as notas fiscais geradas até serem processadas pelo serviço de lançamento.

4. **Service Lançamento**
   - **Descrição**: Serviço que consome as notas fiscais do Service Bus e realiza o processamento de lançamento.
   - **Tecnologia**: Microservice / API RESTful
   - **Responsabilidade**: Processar as notas fiscais armazenadas na fila e concluir o lançamento.

### Relacionamentos

1. **Job Scheduler ➡️ Service Declaração**
   - **Descrição**: O Job Scheduler agenda e dispara o processo, enviando um RPS para o serviço de Declaração para que a nota seja gerada.

2. **Service Declaração ➡️ Service Bus**
   - **Descrição**: O Serviço de Declaração gera a nota e armazena temporariamente no Service Bus.

3. **Service Bus ➡️ Service Lançamento**
   - **Descrição**: O Service Bus envia as notas armazenadas para o Serviço de Lançamento, que processa os dados e conclui o lançamento.

### Diagrama

```mermaid
graph LR
  JobScheduler["Job Scheduler"] --> ServiceDeclaracao["Service Declaração"]
  ServiceDeclaracao --> ServiceBus["Service Bus"]
  ServiceBus --> ServiceLancamento["Service Lançamento"]
