# C4 Model - Nível 2: Diagrama de Contêiner

## Sistema: Consulta da Nota Gerada

O **Sistema de Consulta da Nota Gerada** é composto por vários contêineres que interagem para fornecer funcionalidades de autenticação, validação e consulta de notas geradas. A seguir, detalhamos os principais contêineres e suas interações.

### Componentes do Sistema

#### 1. **Nginx**
   - **Descrição**: Contêiner responsável pela autenticação dos usuários utilizando mutual TLS. Atua como ponto de entrada inicial para todas as requisições.
   - **Tecnologia**: Nginx com suporte a mutual TLS.

#### 2. **WS (Web Service)**
   - **Descrição**: Contêiner que recebe as requisições autenticadas e faz o gerenciamento inicial das solicitações dos usuários. Valida o formato e os dados das requisições antes de enviá-las para o próximo componente.
   - **Tecnologia**: Serviço Web hospedado em um servidor de aplicação.

#### 3. **Proxy-Declaração**
   - **Descrição**: Contêiner que faz a validação das permissões e da assinatura digital dos usuários. É responsável por verificar se a empresa e o usuário possuem as autorizações necessárias para consultar ou gerar uma nota.
   - **Tecnologia**: Serviço de proxy configurado com regras de validação e autorização.

#### 4. **Declaração-WS**
   - **Descrição**: Contêiner responsável por executar a busca de notas geradas com base nos parâmetros fornecidos pela requisição validada.
   - **Tecnologia**: Serviço Web configurado para consultas parametrizadas em banco de dados.

#### 5. **Relatório**
   - **Descrição**: Contêiner responsável por gerar relatórios no formato XML das notas encontradas e fornecê-los aos usuários.
   - **Tecnologia**: Serviço configurado para geração de relatórios e exportação de dados em XML.

### Fluxo de Comunicação

1. O usuário se conecta ao sistema e se autentica através do contêiner **Nginx**, utilizando mutual TLS.
2. O **Nginx** redireciona a requisição autenticada para o contêiner **WS**, que realiza a recepção e validação inicial da solicitação.
3. O **WS** encaminha a requisição para o contêiner **Proxy-Declaração**.
4. O **Proxy-Declaração** valida as permissões e a assinatura digital da empresa e do usuário:
   - Caso a validação seja bem-sucedida, o **Proxy-Declaração** interage com os seguintes contêineres:
     - **Declaração-WS**: Realiza a consulta de notas com base nos parâmetros recebidos.
     - **Relatório**: Gera um XML contendo as informações das notas consultadas.

### Segurança

- **Autenticação**: Utiliza-se mutual TLS no contêiner **Nginx** para garantir uma comunicação segura entre o usuário e o sistema.
- **Permissões**: O **Proxy-Declaração** valida as permissões de acesso para garantir que apenas usuários e empresas autorizadas possam consultar ou gerar relatórios.

### Resumo

Este diagrama de contêiner em nível 2 descreve a arquitetura do **Sistema de Consulta da Nota Gerada**, incluindo os principais contêineres, suas responsabilidades e as interações entre eles. Ele destaca a importância da autenticação e validação de permissões para garantir a segurança e a integridade dos dados consultados.

---

