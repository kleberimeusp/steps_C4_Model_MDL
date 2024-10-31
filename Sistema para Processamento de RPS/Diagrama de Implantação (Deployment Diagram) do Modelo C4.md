# C4 Model - Deployment Diagram

## Sistema: Processamento do RPS

### Visão Geral
O diagrama de implantação fornece uma visão detalhada de como os contêineres de software são implantados na infraestrutura, mostrando servidores, nós de contêineres, instâncias, e como eles se comunicam entre si.

### Objetivo
Fornecer uma visão clara da arquitetura de implantação do sistema de **Processamento do RPS**, destacando servidores, instâncias de contêineres, serviços em nuvem e filas de mensagens.

### Nós de Implantação

1. **Servidor de Aplicação (Node A)**
   - **Descrição**: Servidor principal onde os serviços de processamento e validação de RPS estão implantados.
   - **Tecnologia**: Kubernetes Cluster (Azure AKS) / Docker
   - **Contêineres**:
     - **Job Scheduler**
       - **Descrição**: Contêiner responsável por agendar e iniciar tarefas para buscar RPS.
       - **Tecnologia**: CronJob
     - **Service Declaração**
       - **Descrição**: Serviço responsável por gerar notas fiscais a partir dos dados de RPS.
       - **Tecnologia**: Docker Container (Spring Boot)

2. **Servidor de Mensagens (Node B)**
   - **Descrição**: Servidor que hospeda a fila de mensagens responsável por armazenar notas geradas temporariamente.
   - **Tecnologia**: Azure Service Bus / Kafka Cluster
   - **Contêineres**:
     - **Message Queue**
       - **Descrição**: Fila de mensagens para troca de informações entre serviços.
       - **Tecnologia**: Azure Service Bus Queue

3. **Servidor de Processamento (Node C)**
   - **Descrição**: Servidor onde os serviços de processamento final de lançamento estão implantados.
   - **Tecnologia**: Kubernetes Cluster (AWS EKS) / Docker
   - **Contêineres**:
     - **Service Lançamento**
       - **Descrição**: Serviço responsável por processar e validar o lançamento de notas.
       - **Tecnologia**: Docker Container (Spring Boot)

### Relacionamentos entre Nós

1. **Node A ➡️ Node B**: O **Job Scheduler** e o **Service Declaração** no **Node A** interagem com a fila de mensagens (**Message Queue**) no **Node B** para salvar as notas geradas.
2. **Node B ➡️ Node C**: O **Message Queue** no **Node B** envia as notas para o **Service Lançamento** no **Node C** para o processamento final.

### Diagrama

```mermaid
flowchart LR
  subgraph NodeA["Servidor de Aplicação"]
    JobScheduler["Job Scheduler"]
    ServiceDeclaracao["Service Declaração"]
  end

  subgraph NodeB["Servidor de Mensagens"]
    MessageQueue["Message Queue"]
  end

  subgraph NodeC["Servidor de Processamento"]
    ServiceLancamento["Service Lançamento"]
  end

  JobScheduler --> ServiceDeclaracao
  ServiceDeclaracao --> MessageQueue
  MessageQueue --> ServiceLancamento
