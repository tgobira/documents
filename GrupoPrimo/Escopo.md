# Documento de Escopo de Projeto

**Projeto:** Plataforma Integrada de Pagamento a Fornecedores com IA, Portal Público, TOTVS Fluig e Protheus.

---

## 1. Premissas do Projeto

Para garantir a viabilidade, aderência aos prazos e a correta execução do projeto, as seguintes premissas são estabelecidas:

### 1.1 Premissas Obrigatórias e de Responsabilidade

- **Responsabilidade de Serviços de Terceiros (APIs e IA):** É de inteira responsabilidade do cliente a contratação, licenciamento, manutenção, disponibilidade e custos relacionados aos Agentes de Inteligência Artificial (utilizados para OCR/leitura de Notas Fiscais) e às APIs de terceiros (consulta de CPF/CNPJ).
- **Infraestrutura e Acessos:** O cliente deverá disponibilizar, logo no início do projeto, todos os acessos necessários (VPN, senhas, acessos aos servidores Dev/QA/Prod, banco de dados e APIs de serviços contratados).
- **Licenciamento:** O cliente é responsável por manter o licenciamento do TOTVS Fluig, TOTVS Protheus e da hospedagem necessária para o Portal Público.

### 1.2 Premissas de Desenvolvimento (BPMN integrado ao ERP)

- **Ambientes Homologados:** O desenvolvimento e testes ocorrerão obrigatoriamente em ambientes de Qualidade/Homologação (Protheus e Fluig) espelhados da produção.
- **Padrão TOTVS Protheus:** Os endpoints construídos no ERP (AdvPL REST) utilizarão, preferencialmente, as rotinas padrão (`ExecAuto`/`MSExecAuto`) para Cadastro de Fornecedores (`MATA020`) e Pedidos de Compra/Títulos a Pagar (`MATA120`/`FINA050`), garantindo a integridade do ERP.
- **Portal Público:** O portal construído externamente irá consumir as APIs públicas do TOTVS Fluig (iniciar solicitações) utilizando um usuário integrador/genérico de sistema, eximindo a necessidade de licenciamento nominal do Fluig para os fornecedores externos.

---

## 2. Estrutura Analítica do Escopo e Volumetria (WBS)

> Considerando dias úteis compostos por **9 horas de trabalho**.

| Item | Descrição da Atividade |
|:----:|------------------------|
| 1 | **Validações de acesso iniciais:** Reunião de kickoff, testes de conexão (VPN, fechamento de infraestrutura). |
| 2 | **Overview de regras de negócios:** Mapeamento detalhado do processo de pagamento a fornecedores, fluxos de alçadas de aprovação, exceções e regras de validação fiscal/financeira. |
| 3 | **Desenvolvimento no TOTVS Fluig (BPMN):** Modelagem do fluxo de processo, criação de formulários dinâmicos, eventos de formulário (`displayFields`, `validateForm`) e eventos de processo. |
| 4 | **Integração com IA (Interpretação de NF):** Desenvolvimento de script para enviar o anexo da Nota Fiscal à IA, tratar o retorno (JSON estruturado) e popular o formulário do Fluig automaticamente. |
| 5 | **Integração com API de Dados (CPF/CNPJ):** Construção da chamada via Fluig para serviço terceiro da Receita Federal/SinTegra para validação e auto-preenchimento dos dados do fornecedor. |
| 6 | **Integração do Fluig com ERP (Protheus):** Desenvolvimento dos scripts (datasets e eventos) no Fluig para realizar chamadas REST de envio e consulta de dados no Protheus. |
| 7 | **Desenvolvimento de Endpoints no ERP (Protheus):** Criação de APIs RESTful customizadas em AdvPL para receber o payload do Fluig e executar a inclusão de Fornecedores e gravação de Pedidos/Títulos a pagar. |
| 8 | **Portal Público Externo (Frontend + Auth):** Desenvolvimento de página web pública para fornecedores (React/Angular ou similar), com gestão de usuários própria, autenticação e formulário integrado ao Fluig via API. |
| 9 | **Validação Pontual (Dev):** Testes unitários, testes de integração, simulação de erros de API e validação técnica interna pela equipe de desenvolvimento. |
| 10 | **Validação Conjunta (UAT):** Testes de Aceitação do Usuário (Homologação). Apresentação do fluxo fim a fim para as áreas de negócio do cliente e ajustes finos. |
| 11 | **Go Live:** Deploy em ambiente de produção (Fluig, Protheus, Portal), Handover para equipe técnica do cliente e capacitação dos usuários chave. |
| 12 | **Operação Assistida:** Acompanhamento pós-Go Live para resolução imediata de dúvidas, monitoramento de logs das integrações (IA e ERP) e correções emergenciais. Documentação técnica das APIs do Protheus, diagramas de arquitetura e manual operacional. |
|  | **TOTAL ESTIMADO** |

---

## 3. Cronograma Macro de Execução

As atividades serão divididas em fases ou Sprints lógicas, permitindo paralelismo em algumas tarefas (ex: desenvolvimento do Protheus e do Portal Público simultaneamente, caso haja mais de um recurso).

> Considerando execução sequencial padrão com **1 Squad focado**:

### FASE 1: Descoberta, Planejamento e Infraestrutura *(Semana 1)*

- **Atividades:** 1 e 2.
- **Entregáveis:**
  - Documento de Especificação de Regras de Negócio assinado
  - Arquitetura de Integração validada
  - Ambientes acessíveis

### FASE 2: Backend e Integrações Core *(Semanas 2 e 3)*

- **Atividades:** 7, 5 e 4.
- **Entregáveis:**
  - APIs REST do Protheus publicadas e testadas via Postman/Insomnia
  - Lógica de comunicação com IA e API de CPF/CNPJ estruturada e pronta para ser consumida

### FASE 3: Orquestração BPMN e Camada Visual *(Semanas 4, 5 e 6)*

- **Atividades:** 3, 6 e 8.
- **Entregáveis:**
  - Fluxo BPMN funcional no Fluig se comunicando com o Protheus (gravação de dados)
  - Portal Público operante com sistema de login próprio e integração funcional (abertura de processo via API do Fluig bypassando licenciamento)

### FASE 4: Qualidade e Homologação *(Semana 7)*

- **Atividades:** 9 e 10.
- **Entregáveis:**
  - Bateria de testes técnicos executados e relatório de UAT (User Acceptance Testing) validado junto ao cliente
  - Termo de aceite da fase de homologação

### FASE 5: Entrega, Lançamento e Transição *(Semana 8)*

- **Atividades:** 11, 13 e Início da 12.
- **Entregáveis:**
  - Sistema em Produção
  - Base de conhecimento entregue (Manuais)
  - Usuários treinados

### FASE 6: Pós-Implantação *(Semana 9)*

- **Atividades:** 12 (Conclusão da Operação Assistida).
- **Entregáveis:**
  - Termo de Encerramento do Projeto assinado, atestando a estabilidade da solução e a entrega total do escopo
