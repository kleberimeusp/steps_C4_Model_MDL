# C4 Model - Code Diagram (Level 4)

## Sistema: Processamento do RPS

### Visão Geral
O diagrama de código fornece uma visão detalhada do design de classes, métodos e estruturas de código dentro de componentes específicos do sistema de **Processamento do RPS**. Neste nível, a modelagem de código foca em um componente chave para fornecer um exemplo mais granular.

### Objetivo
Fornecer uma visão detalhada do design e das interações dentro dos componentes de software, destacando classes, métodos e interações entre eles.

### Componente Selecionado: **Nota Generator** (Service Declaração)

### Classes Principais

1. **RpsService**
   - **Descrição**: Classe responsável por gerenciar a lógica de negócio relacionada ao processamento dos RPS.
   - **Métodos**:
     - `processarRps(rps: Rps)`: Processa o RPS fornecido e retorna uma `NotaFiscal`.
     - `validarRps(rps: Rps)`: Valida os dados do RPS.
   - **Dependências**:
     - `NotaFiscalRepository`: Repositório para persistência de notas fiscais.
     - `RpsValidator`: Classe auxiliar para validação de RPS.

2. **RpsValidator**
   - **Descrição**: Classe auxiliar responsável por validar a integridade dos dados do RPS.
   - **Métodos**:
     - `validarFormato(rps: Rps)`: Verifica se o RPS segue o formato correto.
     - `verificarCamposObrigatorios(rps: Rps)`: Verifica se todos os campos obrigatórios estão presentes.

3. **NotaFiscal**
   - **Descrição**: Classe que representa uma nota fiscal gerada.
   - **Propriedades**:
     - `id: Long`
     - `valor: BigDecimal`
     - `dataEmissao: LocalDateTime`
     - `descricao: String`

4. **NotaFiscalRepository**
   - **Descrição**: Classe responsável por persistir e recuperar notas fiscais do banco de dados.
   - **Métodos**:
     - `salvarNota(notaFiscal: NotaFiscal)`: Salva uma nota fiscal no banco de dados.
     - `buscarNotaPorId(id: Long)`: Busca uma nota fiscal pelo seu ID.

### Relacionamentos entre Classes

- **RpsService** usa **RpsValidator** para validar os RPS.
- **RpsService** cria instâncias de **NotaFiscal** após processar e validar um RPS.
- **RpsService** persiste instâncias de **NotaFiscal** utilizando **NotaFiscalRepository**.

### Diagrama de Classes

```mermaid
classDiagram
  class RpsService {
    +NotaFiscal processarRps(Rps rps)
    +boolean validarRps(Rps rps)
    -NotaFiscalRepository notaFiscalRepository
    -RpsValidator rpsValidator
  }

  class RpsValidator {
    +boolean validarFormato(Rps rps)
    +boolean verificarCamposObrigatorios(Rps rps)
  }

  class NotaFiscal {
    +Long id
    +BigDecimal valor
    +LocalDateTime dataEmissao
    +String descricao
  }

  class NotaFiscalRepository {
    +void salvarNota(NotaFiscal notaFiscal)
    +NotaFiscal buscarNotaPorId(Long id)
  }

  RpsService --> RpsValidator : usa
  RpsService --> NotaFiscal : cria
  RpsService --> NotaFiscalRepository : persiste
