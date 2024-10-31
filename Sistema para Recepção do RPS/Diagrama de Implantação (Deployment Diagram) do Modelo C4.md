# Modelo C4: Diagrama de Implantação para Recepção do RPS

## Visão Geral

Este diagrama descreve como os containers e componentes do sistema são implantados em uma infraestrutura física ou virtual. O diagrama de implantação foca em representar os nós de hardware e software envolvidos, bem como os relacionamentos e conexões entre esses nós.

## Nós de Implantação

### 1. **Nó: Servidor de Front-End** 

- **Descrição**: Responsável por servir como ponto de entrada inicial para o sistema.
- **Componentes Implantados**: 
  - **Nginx**: Componente de autenticação TLS.
- **Tecnologia**: 
  - **Serviço**: Nginx
  - **Máquina**: Servidor Virtual na nuvem (por exemplo, AWS EC2, Azure VM, Google Compute Engine).

### 2. **Nó: Cluster de Serviços** 

- **Descrição**: Cluster de containers que executa os serviços principais de negócios.
- **Componentes Implantados**:
  - **WS**: Componente de Serviço Web para processamento de requisições.
  - **Proxy-Declaração**: Componente para validação de permissões e assinatura digital.
- **Tecnologia**:
  - **Orquestrador**: Kubernetes (ou Docker Swarm).
  - **Máquina**: Nós Kubernetes para containers (por exemplo, AWS EKS, Azure AKS, Google Kubernetes Engine).

### 3. **Nó: Servidor de Fila de Mensagens**

- **Descrição**: Infraestrutura de fila de mensagens para armazenamento confiável dos RPS processados.
- **Componentes Implantados**:
  - **Declaração-WS**: Componente que salva o RPS na fila.
- **Tecnologia**: 
  - **Serviço de Fila**: AWS SQS, RabbitMQ, ou outro serviço de mensageria.
  - **Máquina**: Instância gerenciada de fila (por exemplo, AWS SQS ou Instância RabbitMQ na nuvem).

### 4. **Nó: Servidor de Logs e Auditoria**

- **Descrição**: Responsável por gerenciar e armazenar logs de auditoria gerados pelo sistema.
- **Componentes Implantados**:
  - **AuditLogger**: Componente que registra logs de auditoria.
- **Tecnologia**: 
  - **Serviço**: ELK Stack (ElasticSearch, Logstash, Kibana) ou solução de logging na nuvem (ex: AWS CloudWatch, Azure Monitor).

## Descrição das Conexões

1. **Conexão Segura entre o Usuário e o Servidor de Front-End (Nginx)**:
   - **Protocolo**: HTTPS
   - **Descrição**: Canal seguro entre o usuário e o Nginx para autenticação via TLS.

2. **Conexão entre Nginx e o Cluster de Serviços**:
   - **Protocolo**: HTTP (em um ambiente seguro) ou HTTPS.
   - **Descrição**: Encaminha solicitações autenticadas para o container WS.

3. **Conexão Interna entre WS e Proxy-Declaração**:
   - **Protocolo**: HTTP/HTTPS
   - **Descrição**: Comunicação interna entre os containers do cluster para validação das requisições.

4. **Conexão entre Proxy-Declaração e Servidor de Fila de Mensagens**:
   - **Protocolo**: HTTP ou Protocolo de Mensageria (ex: AMQP para RabbitMQ).
   - **Descrição**: Envia RPS validados para o Declaração-WS.

5. **Conexão entre WS e Servidor de Logs e Auditoria**:
   - **Protocolo**: HTTP ou Protocolo de Logging (ex: Logstash).
   - **Descrição**: Envia logs de auditoria para o servidor de logs.

## Infraestrutura e Tecnologias de Suporte

- **Orquestração**: Kubernetes ou Docker Swarm para gerenciar o cluster de serviços.
- **Segurança**: TLS para autenticação e HTTPS para comunicação segura.
- **Mensageria**: AWS SQS, RabbitMQ, ou outro serviço de fila para armazenamento confiável.
- **Monitoramento e Auditoria**: Solução de logging centralizada (ex: ELK Stack, AWS CloudWatch).

## Visão Geral de Implantação

A implantação é distribuída em vários nós para garantir a escalabilidade, segurança e separação de responsabilidades. Os serviços críticos estão contidos em um cluster gerenciado por Kubernetes ou Docker Swarm, enquanto os logs e filas são gerenciados em infraestruturas dedicadas.

