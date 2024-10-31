# Modelo C4: Diagrama de Contexto para Recepção do RPS

## Descrição

Este diagrama de contexto descreve a arquitetura de um sistema para recepção de RPS (Recibo Provisório de Serviços). O fluxo aborda a autenticação, recepção, validação e armazenamento do RPS. O sistema é composto por diferentes serviços que cooperam para garantir a integridade e segurança das operações.

## Componentes e Descrição dos Relacionamentos

1. **Usuário**: O usuário (cliente ou sistema externo) inicia o processo ao enviar uma solicitação para o sistema com um RPS para ser processado.

2. **Nginx (Autenticação com TLS)**:
   - **Descrição**: Um servidor Nginx que lida com a autenticação inicial utilizando TLS (Transport Layer Security) para garantir uma conexão segura.
   - **Relacionamento**: Recebe as solicitações dos usuários e encaminha apenas as autenticadas para o serviço de recepção.

3. **WS (Serviço de Recepção)**:
   - **Descrição**: Serviço responsável por receber as requisições autenticadas do Nginx e processá-las para validação.
   - **Relacionamento**: Recebe requisições autenticadas do Nginx e as direciona para o serviço de validação.

4. **Proxy-Declaração**:
   - **Descrição**: Componente intermediário que valida a permissão da empresa solicitante e verifica a assinatura digital dos dados.
   - **Relacionamento**: Recebe requisições do WS para validação de permissão e assinatura.

5. **Declaração-WS**:
   - **Descrição**: Serviço responsável por salvar o RPS validado em uma fila para processamento posterior ou armazenamento.
   - **Relacionamento**: Recebe o RPS validado pelo Proxy-Declaração e o salva em uma fila.

## Fluxo de Trabalho Resumido

1. O **usuário** faz uma solicitação de envio de um RPS.
2. O **Nginx** autentica a solicitação utilizando TLS.
3. A requisição autenticada é direcionada para o **WS**, que a processa.
4. O **Proxy-Declaração** valida a permissão da empresa e verifica a assinatura.
5. O **Declaração-WS** salva o RPS validado em uma fila.

## Visão Geral

O sistema é projetado para garantir a segurança e a integridade dos RPS recebidos por meio de autenticação TLS e validação de permissões, além de proporcionar um mecanismo confiável de armazenamento para processamento posterior.
