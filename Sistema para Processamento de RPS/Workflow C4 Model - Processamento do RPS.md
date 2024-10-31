
# Processamento do RPS

## Descrição Geral
Este projeto implementa o fluxo de processamento do RPS (Recibo Provisório de Serviços), dividindo-o em componentes principais para buscar, gerar, armazenar temporariamente e processar notas fiscais a partir de registros de RPS. Utilizamos um padrão de arquitetura baseado no modelo C4, com foco na modularidade e comunicação eficiente entre componentes.

## Visão Geral do Workflow
O diagrama de fluxo de processamento do RPS é composto pelos seguintes componentes:

### Componentes Principais

1. **Job**: 
   - **Responsabilidade**: Buscar o RPS na fila de entrada.
   - **Descrição**: Este componente é responsável por consultar os registros de RPS disponíveis e iniciá-los para o próximo passo no processamento.

2. **Declaração (Geração de Nota)**:
   - **Responsabilidade**: Gerar a nota fiscal a partir dos dados do RPS.
   - **Descrição**: Após receber os dados do componente Job, este componente gera uma nota fiscal correspondente com base nas informações obtidas.

3. **ServiceBus**:
   - **Responsabilidade**: Salvar a nota gerada na fila de lançamento.
   - **Descrição**: Atua como um middleware de comunicação, garantindo o armazenamento temporário e a entrega dos dados para os próximos passos do fluxo.

4. **Declaração (Processamento do Lançamento)**:
   - **Responsabilidade**: Processar o lançamento da nota fiscal.
   - **Descrição**: Utiliza os dados armazenados no ServiceBus para completar o lançamento da nota gerada anteriormente.

## Fluxo de Processamento

1. **Busca RPS**:
   - O componente `Job` busca os registros de RPS na fila inicial, sinalizando o início do processamento.

2. **Gera a Nota**:
   - O componente `Declaração` utiliza os registros obtidos para gerar uma nota fiscal correspondente.

3. **Salva Nota no ServiceBus**:
   - O componente `ServiceBus` recebe a nota gerada e a armazena em uma fila específica para lançamento posterior.

4. **Processa Lançamento**:
   - Um segundo componente `Declaração` utiliza os dados armazenados no `ServiceBus` para processar o lançamento da nota.

## Como Contribuir
Para contribuir com este projeto, siga os seguintes passos:

1. **Clone o repositório**:
   ```bash
   git clone https://github.com/seu-usuario/processamento-rps.git
   ```

2. **Crie uma branch para sua feature ou correção**:
   ```bash
   git checkout -b sua-branch
   ```

3. **Implemente sua modificação**.

4. **Realize um commit das alterações**:
   ```bash
   git commit -m "Descrição do que foi alterado"
   ```

5. **Envie as alterações para o repositório remoto**:
   ```bash
   git push origin sua-branch
   ```

6. **Abra um Pull Request** explicando suas mudanças.

## Estrutura do Projeto

```
├── src
│   ├── job/
│   ├── declaracao/
│   ├── servicebus/
│   └── utils/
├── docs/
│   ├── c4-model/
│   └── diagrams/
└── README.md
```

## Tecnologias Utilizadas
- **ServiceBus**: Para comunicação entre componentes de forma assíncrona.
- **Microservices**: Arquitetura baseada em serviços independentes e escaláveis.
- **C4 Model**: Utilizado para modelagem de arquitetura.

## Licença
Este projeto está licenciado sob a licença MIT - veja o arquivo [LICENSE](./LICENSE) para mais detalhes.
