# C4 Model - Nível 1: Diagrama de Contexto

## Sistema: Consulta da Nota Gerada

O **Sistema de Consulta da Nota Gerada** é responsável por fornecer aos usuários uma maneira segura de consultar notas geradas. Através da autenticação mútua (mutual TLS) e da validação de permissões, o sistema garante que apenas usuários autorizados possam acessar ou gerar relatórios.

### Componentes do Sistema

- **Nginx**
  - **Descrição**: Responsável por autenticar os usuários utilizando mutual TLS antes de redirecionar a requisição para o serviço interno.
  
- **WS**
  - **Descrição**: Serviço que recepciona a requisição do usuário autenticado através do Nginx e encaminha para o próximo componente do sistema.
  
- **Proxy-Declaração**
  - **Descrição**: Realiza a validação da permissão da empresa e da assinatura digital, encaminhando a requisição validada para os serviços adequados.
  
- **Declaração-WS**
  - **Descrição**: Responsável por buscar as notas geradas com base nos parâmetros recebidos na requisição.
  
- **Relatório**
  - **Descrição**: Gera o XML da nota conforme solicitado, retornando o resultado ao usuário.

### Fluxo de Informação

1. O usuário se autentica por meio do **Nginx** utilizando mutual TLS.
2. A requisição autenticada é direcionada ao **WS** para processamento inicial.
3. O **WS** envia a requisição para o **Proxy-Declaração**.
4. O **Proxy-Declaração** valida a permissão da empresa e a assinatura do usuário.
5. Após a validação:
   - O **Proxy-Declaração** acessa o **Declaração-WS** para buscar notas com base nos parâmetros fornecidos.
   - O **Proxy-Declaração** pode gerar um relatório no formato XML através do **Relatório**.

### Segurança e Validações

- **Autenticação**: Utiliza-se mutual TLS para garantir uma comunicação segura entre o usuário e o sistema.
- **Permissões**: O **Proxy-Declaração** valida as permissões e as assinaturas para assegurar que apenas usuários autorizados tenham acesso.

### Resumo

Este diagrama de contexto em nível 1 ilustra a interação dos componentes principais no **Sistema de Consulta da Nota Gerada**, destacando as etapas de autenticação, validação de permissões e geração de relatórios.

---

