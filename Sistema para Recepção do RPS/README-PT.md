# Modelo C4: Diagrama de Containers do Sistema para Recepção do RPS

## Containers

1. **Nginx**:
   - **Propósito**: Fornece uma entrada segura para o sistema por meio de autenticação TLS.
   - **Função**: Valida e encaminha solicitações autenticadas para o container WS.

2. **WS (Serviço Web)**:
   - **Propósito**: Receber solicitações do Nginx após a autenticação bem-sucedida.
   - **Função**: Atua como ponto de entrada principal para processar e encaminhar as solicitações para validações adicionais.

3. **Proxy-Declaração**:
   - **Propósito**: Validar a permissão da empresa e a assinatura digital para processar o RPS.
   - **Função**: Servir como uma camada intermediária para verificações de segurança adicionais e integridade dos dados.

4. **Declaração-WS**:
   - **Propósito**: Salvar o RPS validado em uma fila para processamento ou armazenamento posterior.
   - **Função**: Garantir que o RPS seja persistido de forma confiável para ações futuras.

## Passos do Fluxo

1. A solicitação do usuário é recebida pelo **Nginx**, que realiza a **autenticação utilizando TLS**.
2. Caso autenticada, a solicitação é encaminhada para o **WS**, que **recebe e processa a solicitação do usuário**.
3. O WS envia a solicitação para o **Proxy-Declaração**, onde ocorre a **validação das permissões da empresa e da assinatura**.
4. Após a validação, o **RPS é salvo na fila** pelo serviço **Declaração-WS**.
