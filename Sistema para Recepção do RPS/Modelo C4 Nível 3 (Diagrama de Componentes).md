# Modelo C4: Diagrama de Componentes para Recepção do RPS

## Visão Geral

Este diagrama de componentes detalha a arquitetura interna de cada container envolvido na recepção de RPS. Cada container possui componentes internos responsáveis por funções específicas, que colaboram para garantir o processamento seguro e correto dos RPS enviados ao sistema.

## Componentes Internos dos Containers

### Container: **Nginx**

1. **Componente: Autenticador TLS**:
   - **Descrição**: Componente que realiza a autenticação dos usuários via TLS.
   - **Responsabilidade**: Validar a identidade do usuário com certificados TLS e encaminhar a solicitação autenticada para o WS.

### Container: **WS (Serviço Web)**

1. **Componente: Controlador de Requisições**:
   - **Descrição**: Componente que gerencia e controla as requisições recebidas.
   - **Responsabilidade**: Receber as requisições autenticadas do Nginx, validar a estrutura dos dados e repassá-los ao componente de Proxy-Declaração.

2. **Componente: Logger de Auditoria**:
   - **Descrição**: Componente que registra todas as ações e transações realizadas.
   - **Responsabilidade**: Garantir que todas as ações e transações sejam registradas para auditoria e rastreamento.

### Container: **Proxy-Declaração**

1. **Componente: Validador de Permissões**:
   - **Descrição**: Componente responsável por verificar se a empresa possui as permissões necessárias para realizar a ação.
   - **Responsabilidade**: Validar as permissões da empresa com base nas políticas configuradas.

2. **Componente: Verificador de Assinatura Digital**:
   - **Descrição**: Componente que verifica a assinatura digital do RPS enviado.
   - **Responsabilidade**: Validar se a assinatura digital é autêntica e corresponde aos dados da empresa.

### Container: **Declaração-WS**

1. **Componente: Gerenciador de Fila**:
   - **Descrição**: Componente responsável por gerenciar a fila de mensagens para armazenamento e processamento posterior.
   - **Responsabilidade**: Persistir o RPS validado em uma fila segura para processamento posterior.

2. **Componente: Integrador de Mensagens**:
   - **Descrição**: Componente que se integra com serviços de fila (ex: AWS SQS, RabbitMQ, etc.).
   - **Responsabilidade**: Garantir que o RPS validado seja enviado corretamente para a fila de mensagens.

## Fluxo de Trabalho Detalhado

1. O **usuário** envia uma solicitação contendo um RPS.
2. O **Autenticador TLS** do container **Nginx** realiza a autenticação usando certificados TLS.
3. A requisição autenticada é encaminhada para o **Controlador de Requisições** no container **WS**.
4. O **Controlador de Requisições** valida a estrutura da requisição e registra a ação com o **Logger de Auditoria**.
5. A requisição validada é encaminhada para o **Validador de Permissões** no container **Proxy-Declaração**.
6. O **Validador de Permissões** verifica se a empresa possui as permissões necessárias.
7. O **Verificador de Assinatura Digital** valida a autenticidade da assinatura digital da empresa.
8. Após validação, a requisição é encaminhada para o **Gerenciador de Fila** no container **Declaração-WS**.
9. O **Gerenciador de Fila** persiste o RPS na fila de mensagens utilizando o **Integrador de Mensagens**.

## Tecnologias e Componentes Internos

Cada container foi projetado para conter componentes internos específicos, os quais desempenham funções modulares e colaborativas para garantir um fluxo seguro e eficiente.

- **Nginx**: Autenticador TLS.
- **WS**: Controlador de Requisições e Logger de Auditoria.
- **Proxy-Declaração**: Validador de Permissões e Verificador de Assinatura Digital.
- **Declaração-WS**: Gerenciador de Fila e Integrador de Mensagens.

## Detalhes de Componentes

Os componentes internos de cada container foram projetados para realizar funções específicas que suportam a responsabilidade principal de cada container, garantindo uma separação clara de preocupações e uma maior escalabilidade.

