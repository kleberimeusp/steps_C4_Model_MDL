# C4 Model: Diagrama de Implantação

## Sistema: Consulta da Nota Gerada

O diagrama de implantação descreve como os contêineres e componentes do **Sistema de Consulta da Nota Gerada** são implantados em uma infraestrutura física e/ou virtual. Essa visão detalha os servidores, os contêineres e a conectividade entre eles.

### Infraestrutura de Implantação

#### Servidor 1: **Servidor Web**
- **Descrição**: Servidor dedicado ao gerenciamento de requisições externas e autenticação.
- **Componentes Implementados**:
  - **Nginx**
    - **Função**: Responsável por autenticar os usuários via mutual TLS e redirecionar as requisições para o Servidor de Aplicação.
- **Tecnologia**: Nginx configurado para mutual TLS.

#### Servidor 2: **Servidor de Aplicação**
- **Descrição**: Servidor onde residem os serviços de processamento e validação das requisições.
- **Componentes Implementados**:
  - **WS (Web Service)**
    - **Função**: Controlador de requisições e validador de formato.
  - **Proxy-Declaração**
    - **Função**: Validação de permissões e assinaturas digitais.
- **Tecnologia**: Servidor de aplicação configurado para rodar serviços RESTful.

#### Servidor 3: **Servidor de Banco de Dados**
- **Descrição**: Servidor dedicado ao armazenamento e consulta das notas geradas.
- **Componentes Implementados**:
  - **Banco de Dados**
    - **Função**: Armazenar as notas e possibilitar a execução de consultas parametrizadas.
- **Tecnologia**: Banco de Dados Relacional (ex: PostgreSQL, MySQL).

#### Servidor 4: **Servidor de Relatórios**
- **Descrição**: Servidor dedicado à geração de relatórios e formatação de notas em XML.
- **Componentes Implementados**:
  - **Relatório**
    - **Função**: Gerar o XML das notas consultadas.
- **Tecnologia**: Serviço configurado para a geração e exportação de dados em formato XML.

### Conectividade

1. **Comunicação entre o Servidor Web e o Servidor de Aplicação**:
   - **Protocolo**: HTTPS (mutual TLS) para garantir a segurança e a integridade dos dados.
   - **Função**: O Servidor Web autentica os usuários e redireciona as requisições para o Servidor de Aplicação após a autenticação.

2. **Comunicação entre o Servidor de Aplicação e o Servidor de Banco de Dados**:
   - **Protocolo**: Conexão segura com criptografia para proteger as consultas ao banco de dados.
   - **Função**: O Servidor de Aplicação consulta o banco de dados para buscar as notas geradas com base nos parâmetros fornecidos.

3. **Comunicação entre o Servidor de Aplicação e o Servidor de Relatórios**:
   - **Protocolo**: HTTP/HTTPS para o envio de dados das notas e a geração do XML.
   - **Função**: O Servidor de Aplicação solicita a geração do XML ao Servidor de Relatórios.

### Segurança

- **Mutual TLS**: Utilizado na comunicação entre o Servidor Web e o Servidor de Aplicação para autenticação mútua e proteção de dados.
- **Criptografia de Dados**: Aplicada em todas as conexões entre servidores para garantir a privacidade e a segurança das informações trafegadas.

### Resumo

O diagrama de implantação do **Sistema de Consulta da Nota Gerada** descreve como os contêineres e componentes são distribuídos entre diferentes servidores e como eles se comunicam de forma segura. Essa configuração garante a separação de responsabilidades, escalabilidade e proteção dos dados sensíveis.

---
