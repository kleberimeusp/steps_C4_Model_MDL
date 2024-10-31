# Modelo C4: Diagrama de Containers para Recepção do RPS

## Visão Geral

Este diagrama descreve a arquitetura do sistema para recepção de RPS (Recibo Provisório de Serviços) com mais detalhes, apresentando os containers responsáveis por diferentes etapas do fluxo. O sistema é composto por quatro containers principais: Nginx, WS, Proxy-Declaração e Declaração-WS, cada um com uma função específica.

## Containers

1. **Nginx**:
   - **Propósito**: Atuar como ponto de entrada seguro para o sistema, realizando autenticação via TLS.
   - **Tecnologia**: Nginx.
   - **Responsabilidade**: Autenticar a solicitação do usuário e encaminhá-la para o WS, caso a autenticação seja bem-sucedida.
   - **Relacionamento**: Recebe solicitações dos usuários e encaminha as solicitações autenticadas para o WS.

2. **WS (Serviço Web)**:
   - **Propósito**: Receber requisições autenticadas de Nginx.
   - **Tecnologia**: Web Service desenvolvido com tecnologias de microserviços (ex: Spring Boot, Express, etc.).
   - **Responsabilidade**: Processar e validar a requisição do usuário antes de encaminhá-la para o Proxy-Declaração.
   - **Relacionamento**: Recebe solicitações autenticadas do Nginx e encaminha para o Proxy-Declaração.

3. **Proxy-Declaração**:
   - **Propósito**: Validação das permissões da empresa solicitante e verificação da assinatura digital.
   - **Tecnologia**: API intermediária que aplica políticas de validação (pode usar middleware e lógica de negócio).
   - **Responsabilidade**: Realizar validações de permissões de empresa e assinatura digital dos dados.
   - **Relacionamento**: Recebe requisições do WS e, após validação, encaminha para o Declaração-WS.

4. **Declaração-WS**:
   - **Propósito**: Persistir o RPS validado em uma fila para processamento ou armazenamento.
   - **Tecnologia**: Serviço de persistência (por exemplo, AWS SQS, RabbitMQ, ou outro serviço de fila).
   - **Responsabilidade**: Salvar o RPS validado na fila.
   - **Relacionamento**: Recebe requisições validadas do Proxy-Declaração e salva os dados na fila de processamento.

## Fluxo de Trabalho Detalhado

1. O **usuário** faz uma solicitação enviando um RPS para o sistema.
2. O **Nginx** recebe a solicitação e realiza a autenticação utilizando TLS.
3. Após a autenticação, a solicitação é encaminhada para o **WS**.
4. O **WS** processa a solicitação e a encaminha para o **Proxy-Declaração** para validação de permissões e assinatura digital.
5. O **Proxy-Declaração** valida a permissão e a assinatura, e se tudo estiver correto, encaminha o RPS para o **Declaração-WS**.
6. O **Declaração-WS** salva o RPS validado em uma fila para processamento posterior.

## Tecnologias Usadas

- **Nginx** para autenticação segura via TLS.
- **Web Service (WS)** para gerenciar as requisições dos usuários.
- **Proxy-Declaração** para aplicar políticas de validação de permissões e assinatura.
- **Declaração-WS** para persistir os dados validados em uma fila.

## Detalhes do Container

Cada container foi projetado para ser modular, permitindo um escalonamento independente e uma melhor gestão de cada serviço de acordo com sua responsabilidade no fluxo.

