# C4 Model - Nível 3: Diagrama de Componentes

## Sistema: Consulta da Nota Gerada

O **Sistema de Consulta da Nota Gerada** é composto por vários componentes distribuídos dentro de contêineres, que são responsáveis por autenticação, processamento, validação de permissões e geração de relatórios. Neste nível, detalhamos cada componente dentro dos contêineres e suas funções específicas.

### Componentes por Contêiner

#### 1. **Nginx**
   - **Componente: Autenticador Mutual TLS**
     - **Descrição**: Responsável por validar a identidade do usuário usando certificados TLS antes de encaminhar a requisição para os próximos componentes.
     - **Tecnologia**: Implementação de autenticação mútua TLS com regras configuradas no Nginx.

#### 2. **WS (Web Service)**
   - **Componente: Controlador de Requisições**
     - **Descrição**: Gerencia a entrada de requisições autenticadas e realiza uma verificação inicial dos dados fornecidos pelo usuário.
     - **Tecnologia**: API REST que atua como ponto de recepção das requisições.
   - **Componente: Validador de Formato**
     - **Descrição**: Verifica se a requisição segue o formato esperado e se todos os campos obrigatórios estão presentes.

#### 3. **Proxy-Declaração**
   - **Componente: Verificador de Permissões**
     - **Descrição**: Realiza a validação das permissões de acesso da empresa e do usuário com base nas regras de negócio.
     - **Tecnologia**: Serviço de verificação baseado em políticas de acesso e assinaturas.
   - **Componente: Verificador de Assinatura Digital**
     - **Descrição**: Confirma a autenticidade das assinaturas digitais fornecidas na requisição.

#### 4. **Declaração-WS**
   - **Componente: Serviço de Consulta de Notas**
     - **Descrição**: Busca notas geradas com base nos parâmetros fornecidos na requisição validada.
     - **Tecnologia**: API para consultas SQL parametrizadas no banco de dados.

#### 5. **Relatório**
   - **Componente: Gerador de XML**
     - **Descrição**: Gera o XML das notas consultadas e formata o conteúdo de acordo com o padrão especificado.
     - **Tecnologia**: Serviço de geração de relatórios com saída em formato XML.

### Fluxo de Comunicação

1. O usuário realiza a autenticação no **Autenticador Mutual TLS** dentro do contêiner **Nginx**.
2. O **Controlador de Requisições** no contêiner **WS** recebe a requisição autenticada e realiza verificações de formato.
3. A requisição é enviada ao **Verificador de Permissões** e ao **Verificador de Assinatura Digital** dentro do contêiner **Proxy-Declaração** para garantir que apenas usuários e empresas autorizadas tenham acesso.
4. Se a validação for bem-sucedida, o **Serviço de Consulta de Notas** no contêiner **Declaração-WS** é acionado para buscar as notas solicitadas.
5. O **Gerador de XML** dentro do contêiner **Relatório** formata e retorna as notas em XML para o usuário.

### Segurança

- **Autenticação**: Implementada com mutual TLS no componente de **Autenticador Mutual TLS**.
- **Validação de Permissões e Assinaturas**: Realizadas por meio dos componentes de **Verificador de Permissões** e **Verificador de Assinatura Digital**, garantindo que apenas usuários autorizados possam acessar ou gerar relatórios.

### Resumo

Este diagrama de componentes em nível 3 fornece uma visão detalhada da arquitetura do **Sistema de Consulta da Nota Gerada**, descrevendo os componentes dentro de cada contêiner e suas funções específicas. Ele destaca como a segurança e a validação de permissões são garantidas por meio de componentes especializados.

---
