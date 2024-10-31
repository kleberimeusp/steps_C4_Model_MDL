# Diagrama C4 Model - Processamento de RPS

Este documento descreve o modelo C4 para o sistema de **Processamento de RPS**, detalhando sua arquitetura em diferentes níveis de abstração.

## 1. Nível de Contexto (System Context Diagram)

No nível de contexto, representamos o sistema "Processamento de RPS" como um todo, junto com os sistemas externos que interagem com ele.

- **Sistema:** Processamento de RPS
- **Interações Externas:** Nenhuma interação explícita é mostrada na imagem fornecida, mas pode haver interação com sistemas de fila ou APIs externas, dependendo do contexto.

## 2. Nível de Contêiner (Container Diagram)

No nível de contêiner, detalhamos os componentes do sistema. Com base na imagem, a representação pode ser:

- **Contêineres:**
  - **Job:** Componente responsável por buscar RPS na fila.
  - **Declaração:** Módulo que gera e processa as notas fiscais.
  - **Service Bus:** Contêiner de mensageria responsável por salvar notas em uma fila para processamento posterior.

## 3. Nível de Componente (Component Diagram)

No nível de componente, especificamos a funcionalidade de cada parte interna do contêiner.

- **Componentes dentro de "Declaração":**
  - **Geração de Nota:** Módulo que cria a nota a partir dos dados do RPS.
  - **Processamento do Lançamento:** Componente que consome a mensagem da fila de lançamento e realiza o processamento.

## 4. Nível de Código (Code Diagram)

O nível de código mostra a implementação em detalhe, como classes, métodos e detalhes de código. Este nível seria desenvolvido com base em detalhes específicos dos métodos e classes que compõem cada módulo do sistema.

## Resumo do Workflow em Nível C4

- **Contexto:** O sistema "Processamento de RPS" processa RPS da fila para gerar notas fiscais.
- **Contêineres:**
  - **Job**: Busca RPS na fila.
  - **Declaração**: Gera e processa notas fiscais.
  - **Service Bus**: Armazena as notas em uma fila para processamento posterior.
- **Componentes principais:** Dentro de cada contêiner "Declaração" temos a geração de notas e o processamento das mensagens do "Service Bus".

---

Este modelo segue as diretrizes do **C4 Model** para descrever de forma clara e organizada a arquitetura do sistema de Processamento de RPS.
