# C4 Model - Component Diagram (Level 3)

## Sistema: Processamento do RPS

### Visão Geral
O diagrama de componentes fornece uma visão mais detalhada dos contêineres dentro do sistema de **Processamento do RPS (Recibo Provisório de Serviço)**, destacando os componentes internos de cada contêiner, suas responsabilidades e as interações entre eles.

### Objetivo
Fornecer uma visão clara dos componentes internos que constituem os principais contêineres, detalhando as funcionalidades específicas de cada um e suas responsabilidades.

### Contêiner: Job Scheduler

1. **Job Runner**
   - **Descrição**: Componente responsável por executar periodicamente tarefas agendadas.
   - **Tecnologia**: Cron Job
   - **Responsabilidade**: Agendar e executar tarefas de busca de RPS na fila.

2. **RPS Fetcher**
   - **Descrição**: Componente responsável por buscar os RPS na fila de entrada.
   - **Tecnologia**: Script Python / Serviço Java
   - **Responsabilidade**: Conectar-se à fila de RPS e recuperar os dados necessários.

### Contêiner: Service Declaração

1. **Nota Generator**
   - **Descrição**: Componente responsável por processar os dados do RPS e gerar uma nota fiscal.
   - **Tecnologia**: Spring Boot Service
   - **Responsabilidade**: Receber o RPS e criar uma nota fiscal válida.

2. **Nota Validator**
   - **Descrição**: Componente responsável por validar as notas fiscais geradas.
   - **Tecnologia**: Java Validator
   - **Responsabilidade**: Garantir que as notas fiscais geradas estejam em conformidade com as regras de negócio.

3. **Nota Sender**
   - **Descrição**: Componente responsável por enviar as notas geradas para o Service Bus.
   - **Tecnologia**: RESTful API
   - **Responsabilidade**: Armazenar as notas no Service Bus para processamento posterior.

### Contêiner: Service Bus

1. **Message Queue**
   - **Descrição**: Componente responsável por gerenciar a fila de mensagens.
   - **Tecnologia**: Azure Service Bus / Kafka / RabbitMQ
   - **Responsabilidade**: Armazenar e entregar mensagens entre os componentes.

### Contêiner: Service Lançamento

1. **Lançamento Processor**
   - **Descrição**: Componente responsável por processar o lançamento das notas fiscais a partir da fila.
   - **Tecnologia**: Spring Boot Service
   - **Responsabilidade**: Consumir mensagens do Service Bus e processar o lançamento.

2. **Lançamento Validator**
   - **Descrição**: Componente responsável por validar o processamento do lançamento.
   - **Tecnologia**: Java Validator
   - **Responsabilidade**: Validar se os lançamentos foram processados corretamente.

### Relacionamentos

1. **Job Runner ➡️ RPS Fetcher**
   - **Descrição**: O Job Runner aciona o RPS Fetcher para buscar RPS na fila de entrada.

2. **RPS Fetcher ➡️ Nota Generator**
   - **Descrição**: O RPS Fetcher envia os dados do RPS para o Nota Generator.

3. **Nota Generator ➡️ Nota Validator**
   - **Descrição**: O Nota Generator cria uma nota fiscal e a envia para o Nota Validator.

4. **Nota Validator ➡️ Nota Sender**
   - **Descrição**: O Nota Validator valida a nota gerada e, se estiver correta, envia para o Nota Sender.

5. **Nota Sender ➡️ Message Queue**
   - **Descrição**: O Nota Sender armazena a nota validada no Service Bus (Message Queue).

6. **Message Queue ➡️ Lançamento Processor**
   - **Descrição**: O Message Queue envia mensagens contendo notas fiscais para o Lançamento Processor.

7. **Lançamento Processor ➡️ Lançamento Validator**
   - **Descrição**: O Lançamento Processor processa o lançamento e envia para o Lançamento Validator para validação final.

### Diagrama

```mermaid
graph TD
  JobRunner["Job Runner"] --> RPSFetcher["RPS Fetcher"]
  RPSFetcher --> NotaGenerator["Nota Generator"]
  NotaGenerator --> NotaValidator["Nota Validator"]
  NotaValidator --> NotaSender["Nota Sender"]
  NotaSender --> MessageQueue["Message Queue"]
  MessageQueue --> LancamentoProcessor["Lançamento Processor"]
  LancamentoProcessor --> LancamentoValidator["Lançamento Validator"]
