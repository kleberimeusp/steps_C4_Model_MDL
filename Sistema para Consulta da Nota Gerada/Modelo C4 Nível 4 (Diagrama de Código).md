# C4 Model - Nível 4: Diagrama de Código

## Sistema: Consulta da Nota Gerada

No nível 4 do modelo C4, detalhamos as classes e métodos principais que compõem o **Sistema de Consulta da Nota Gerada**. Esta visão fornece um entendimento granular de como os componentes são implementados e como eles se comunicam internamente para atender às requisições.

### Estrutura de Classes e Métodos

#### 1. **Classe: AutenticadorMutualTLS**
   - **Método: autenticarCertificado()**
     - **Descrição**: Método responsável por validar o certificado TLS do usuário. Ele verifica a autenticidade do certificado e a correspondência com o certificado da autoridade confiável.
     - **Retorno**: Booleano indicando se a autenticação foi bem-sucedida.

#### 2. **Classe: ControladorRequisicoes**
   - **Método: receberRequisicao()**
     - **Descrição**: Método responsável por receber a requisição do usuário e encaminhá-la para a validação de formato.
   - **Método: validarFormatoRequisicao()**
     - **Descrição**: Valida se a requisição contém todos os parâmetros obrigatórios e se o formato dos dados está correto.
     - **Retorno**: Booleano indicando se a validação do formato foi bem-sucedida.

#### 3. **Classe: VerificadorPermissoes**
   - **Método: validarPermissoesUsuario()**
     - **Descrição**: Verifica se o usuário tem as permissões necessárias para acessar ou consultar as notas geradas.
     - **Retorno**: Booleano indicando se a validação de permissões foi bem-sucedida.
   - **Método: validarAssinaturaDigital()**
     - **Descrição**: Confirma a autenticidade da assinatura digital fornecida na requisição.
     - **Retorno**: Booleano indicando se a assinatura é válida.

#### 4. **Classe: ServicoConsultaNotas**
   - **Método: buscarNotas()**
     - **Descrição**: Executa a consulta das notas no banco de dados com base nos parâmetros validados. 
     - **Parâmetros**: Objeto de parâmetros da consulta.
     - **Retorno**: Lista de notas geradas.

#### 5. **Classe: GeradorXML**
   - **Método: gerarXMLNota()**
     - **Descrição**: Gera o XML das notas consultadas de acordo com o padrão especificado.
     - **Parâmetros**: Lista de notas.
     - **Retorno**: String contendo o XML gerado.

### Fluxo de Execução

1. O método **autenticarCertificado()** é chamado na **Classe AutenticadorMutualTLS** para validar o certificado TLS do usuário.
2. O método **receberRequisicao()** na **Classe ControladorRequisicoes** gerencia a recepção da requisição e valida seu formato por meio do método **validarFormatoRequisicao()**.
3. Após a validação de formato, os métodos **validarPermissoesUsuario()** e **validarAssinaturaDigital()** são chamados na **Classe VerificadorPermissoes** para confirmar as permissões e a assinatura digital do usuário.
4. Se as validações forem bem-sucedidas, o método **buscarNotas()** na **Classe ServicoConsultaNotas** é executado para realizar a consulta das notas geradas.
5. As notas são então processadas pelo método **gerarXMLNota()** na **Classe GeradorXML**, que formata as notas no padrão XML especificado e retorna o resultado para o usuário.

### Segurança e Validações

- **Autenticação**: Realizada pelo método **autenticarCertificado()** para garantir a identidade do usuário.
- **Validação de Permissões**: Realizada pelos métodos **validarPermissoesUsuario()** e **validarAssinaturaDigital()** para garantir que apenas usuários autorizados tenham acesso aos dados.

### Resumo

Este diagrama de código em nível 4 fornece uma visão detalhada de como as classes e métodos interagem para compor o **Sistema de Consulta da Nota Gerada**. Ele descreve o fluxo completo de validação, consulta e geração de notas, garantindo segurança e consistência no processamento das requisições.

---
