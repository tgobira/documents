# Documento de Escopo de Projeto

**Projeto:** Plataforma Integrada de Pagamento a Fornecedores com IA, Portal Público, TOTVS Fluig e Protheus

**Data:** Abril/2026

**Versão:** 1.0

---

## 1. Premissas do Projeto

Para garantir a viabilidade, aderência aos prazos e a correta execução do projeto, as seguintes premissas são estabelecidas e devem ser formalmente aceitas pelas partes envolvidas.

### 1.1 Premissas Obrigatórias e de Responsabilidade do Cliente

| # | Premissa | Responsável |
|:-:|----------|:-----------:|
| P1 | **Serviços de Terceiros (APIs e IA):** É de inteira responsabilidade do cliente a contratação, licenciamento, manutenção, disponibilidade, custos e suporte relacionados aos Agentes de Inteligência Artificial (utilizados para OCR/leitura de Notas Fiscais) e às APIs de terceiros (consulta de CPF/CNPJ na Receita Federal/Sintegra). Quaisquer indisponibilidades ou limitações desses serviços que impactem o cronograma não serão de responsabilidade da equipe de desenvolvimento. | Cliente |
| P2 | **Infraestrutura e Acessos:** O cliente deverá disponibilizar, até a data de kickoff, todos os acessos necessários: VPN, credenciais de servidores (Dev/QA/Prod), banco de dados, APIs de serviços contratados e usuário administrador no Fluig e Protheus. Atrasos na liberação de acessos impactarão proporcionalmente o cronograma. | Cliente |
| P3 | **Licenciamento de Plataformas:** O cliente é responsável por manter ativo e vigente o licenciamento do TOTVS Fluig, TOTVS Protheus e da infraestrutura de hospedagem necessária para o Portal Público (servidor web, domínio, certificado SSL). | Cliente |
| P4 | **Disponibilidade de Interlocutores:** O cliente deverá designar ao menos 1 (um) ponto focal de negócios e 1 (um) ponto focal técnico com autonomia de decisão e disponibilidade para reuniões de alinhamento, validações e homologação dentro dos prazos estipulados. | Cliente |
| P5 | **Aprovação de Entregas:** Cada fase do projeto possui entregáveis definidos. O cliente terá até **3 dias úteis** para validar cada entrega. A ausência de manifestação dentro deste prazo será considerada como aceite tácito. | Cliente |

### 1.2 Premissas de Desenvolvimento (BPMN integrado ao ERP)

| # | Premissa |
|:-:|----------|
| D1 | **Ambientes Homologados:** O desenvolvimento e testes ocorrerão obrigatoriamente em ambientes de Qualidade/Homologação (Protheus e Fluig) espelhados da produção. Divergências entre ambientes que gerem retrabalho serão tratadas como escopo adicional. |
| D2 | **Padrão TOTVS Protheus:** Os endpoints construídos no ERP (AdvPL REST) utilizarão, preferencialmente, as rotinas padrão (`ExecAuto`/`MSExecAuto`) para Cadastro de Fornecedores (`MATA020`) e Pedidos de Compra/Títulos a Pagar (`MATA120`/`FINA050`), garantindo a integridade e rastreabilidade dentro do ERP. |
| D3 | **Portal Público — Modelo de Integração:** O portal externo consumirá as APIs públicas do TOTVS Fluig utilizando um usuário integrador/genérico de sistema, eximindo a necessidade de licenciamento nominal do Fluig para fornecedores externos. |
| D4 | **Escopo de IA:** O escopo contempla a integração com **1 (um) modelo de IA** já contratado pelo cliente. A interpretação se limita a Notas Fiscais Eletrônicas (NF-e/NFS-e) em formato PDF ou XML. Outros tipos de documentos fiscais requerem análise de escopo adicional. |
| D5 | **Fluxo de Aprovação:** Será implementado **1 (um) fluxo BPMN** de pagamento a fornecedores com até **3 (três) níveis de alçada de aprovação**. Fluxos adicionais ou alçadas extras constituem escopo adicional. |
| D6 | **Customizações do Protheus:** O escopo não contempla alterações em rotinas-padrão do Protheus além da criação dos endpoints REST específicos. Ajustes em pontos de entrada, gatilhos ou relatórios existentes são escopo adicional. |
| D7 | **Dados Mestres:** Presume-se que os cadastros base do Protheus (Empresas, Filiais, Condições de Pagamento, Naturezas, Plano de Contas) estejam corretamente configurados antes do início da Fase 2. |

### 1.3 Premissas de Infraestrutura do Portal Público

| # | Premissa |
|:-:|----------|
| I1 | O servidor de hospedagem do Portal Público deverá suportar aplicações Node.js/React (ou stack equivalente) com HTTPS habilitado. |
| I2 | O banco de dados para gestão de usuários do Portal será independente do banco do Protheus/Fluig. |
| I3 | O cliente é responsável pelo domínio, DNS e certificado SSL do Portal Público. |

---

## 2. Estrutura Analítica do Escopo e Volumetria (WBS)

> Considerando dias úteis compostos por **9 horas de trabalho**.

### GRUPO A — Mapeamento e Planejamento

| Item | Descrição da Atividade | Subatividades | Horas | Dias |
|:----:|------------------------|---------------|:-----:|:----:|
| 1 | **Validações de acesso iniciais** | | **18h** | **2** |
| 1.1 | | Reunião de kickoff e alinhamento de expectativas | 4h | 0,4 |
| 1.2 | | Teste de conexão VPN e acesso aos servidores (Dev/QA/Prod) | 4h | 0,4 |
| 1.3 | | Validação de acesso ao Fluig (admin + usuário integrador) | 3h | 0,3 |
| 1.4 | | Validação de acesso ao Protheus (ambiente REST, compilação AdvPL) | 3h | 0,3 |
| 1.5 | | Teste de conectividade com APIs externas (IA e CPF/CNPJ) | 2h | 0,2 |
| 1.6 | | Documentação do mapa de infraestrutura e acessos | 2h | 0,2 |
| 2 | **Overview de regras de negócios** | | **27h** | **3** |
| 2.1 | | Mapeamento detalhado do processo de pagamento a fornecedores (AS-IS) | 9h | 1,0 |
| 2.2 | | Definição do fluxo TO-BE com alçadas de aprovação e exceções | 9h | 1,0 |
| 2.3 | | Levantamento de regras de validação fiscal e financeira | 4h | 0,4 |
| 2.4 | | Documentação de Especificação de Regras de Negócio | 3h | 0,3 |
| 2.5 | | Validação e assinatura do documento de regras com o cliente | 2h | 0,2 |
| | | **Subtotal Grupo A** | **45h** | **5** |

### GRUPO B — Desenvolvimento

| Item | Descrição da Atividade | Subatividades | Horas | Dias |
|:----:|------------------------|---------------|:-----:|:----:|
| 3 | **Desenvolvimento no TOTVS Fluig (BPMN)** | | **63h** | **7** |
| 3.1 | | Modelagem do diagrama BPMN (fluxo, raias, atividades, gateways) | 9h | 1,0 |
| 3.2 | | Criação do formulário HTML dinâmico (campos, máscaras, validações visuais) | 14h | 1,6 |
| 3.3 | | Desenvolvimento de eventos de formulário (`displayFields`, `validateForm`, `enableFields`) | 14h | 1,6 |
| 3.4 | | Desenvolvimento de eventos de processo (antes/após atividades, condições de gateway) | 9h | 1,0 |
| 3.5 | | Mecanismo de anexo e visualização de documentos (NF) no formulário | 5h | 0,6 |
| 3.6 | | Configuração de alçadas de aprovação (até 3 níveis) e mecanismo de delegação | 7h | 0,8 |
| 3.7 | | Testes internos de navegação do fluxo ponta a ponta no Fluig | 5h | 0,6 |
| 4 | **Integração com IA (Interpretação de NF)** | | **36h** | **4** |
| 4.1 | | Análise da API do agente de IA contratado (autenticação, payloads, limites) | 5h | 0,6 |
| 4.2 | | Desenvolvimento de dataset/serviço no Fluig para envio do anexo (PDF/XML) à IA | 9h | 1,0 |
| 4.3 | | Tratamento e parsing do retorno JSON estruturado da IA | 9h | 1,0 |
| 4.4 | | Lógica de preenchimento automático dos campos do formulário com os dados da NF | 7h | 0,8 |
| 4.5 | | Tratamento de erros, timeout e fallback (preenchimento manual) | 6h | 0,7 |
| 5 | **Integração com API de Dados (CPF/CNPJ)** | | **27h** | **3** |
| 5.1 | | Análise da API terceira (Receita Federal/Sintegra): autenticação, rate limits, payloads | 4h | 0,4 |
| 5.2 | | Desenvolvimento de dataset no Fluig para chamada à API de consulta | 7h | 0,8 |
| 5.3 | | Parsing do retorno e mapeamento de campos (Razão Social, Endereço, CNAE, Situação Cadastral) | 5h | 0,6 |
| 5.4 | | Auto-preenchimento dos campos do formulário e/ou payload de cadastro de fornecedor | 5h | 0,6 |
| 5.5 | | Validações de consistência (CNPJ ativo, situação regular) e tratamento de exceções | 4h | 0,4 |
| 5.6 | | Testes com CNPJs de cenários diversos (ativo, baixado, inapto) | 2h | 0,2 |
| 6 | **Integração do Fluig com ERP (Protheus)** | | **36h** | **4** |
| 6.1 | | Desenvolvimento de datasets de consulta ao Protheus (fornecedores existentes, pedidos, títulos) | 9h | 1,0 |
| 6.2 | | Desenvolvimento de eventos de processo para envio de dados ao Protheus via REST | 9h | 1,0 |
| 6.3 | | Lógica de tratamento de respostas do Protheus (sucesso, erro de validação, timeout) | 7h | 0,8 |
| 6.4 | | Mecanismo de retry e log de auditoria das integrações | 5h | 0,6 |
| 6.5 | | Sincronização de status entre fluxo Fluig e registros no Protheus | 6h | 0,7 |
| 7 | **Endpoints REST no ERP (Protheus — AdvPL)** | | **45h** | **5** |
| 7.1 | | Criação de API REST para inclusão de Fornecedores (`MATA020` via `MsExecAuto`) | 14h | 1,6 |
| 7.2 | | Criação de API REST para gravação de Pedidos de Compra (`MATA120` via `MsExecAuto`) | 12h | 1,3 |
| 7.3 | | Criação de API REST para gravação de Títulos a Pagar (`FINA050` via `MsExecAuto`) | 10h | 1,1 |
| 7.4 | | Endpoints auxiliares de consulta (fornecedor existe? pedido duplicado?) | 5h | 0,6 |
| 7.5 | | Camada de autenticação e autorização dos endpoints (token/basic auth) | 4h | 0,4 |
| 8 | **Portal Público Externo (Frontend + Auth)** | | **63h** | **7** |
| 8.1 | | Setup do projeto frontend (React ou framework equivalente) e estrutura base | 5h | 0,6 |
| 8.2 | | Desenvolvimento do sistema de gestão cadastral de usuários (CRUD) | 12h | 1,3 |
| 8.3 | | Implementação de autenticação e autorização (registro, login, recuperação de senha, JWT) | 14h | 1,6 |
| 8.4 | | Desenvolvimento do formulário de solicitação de pagamento (espelhando dados do Fluig) | 12h | 1,3 |
| 8.5 | | Integração com API pública do Fluig (abertura de processo via usuário integrador) | 9h | 1,0 |
| 8.6 | | Tela de acompanhamento de solicitações (status do processo) | 5h | 0,6 |
| 8.7 | | Responsividade, UX e ajustes visuais finais | 4h | 0,4 |
| 8.8 | | Deploy em ambiente de homologação | 2h | 0,2 |
| | | **Subtotal Grupo B** | **270h** | **30** |

### GRUPO C — Validação e Qualidade

| Item | Descrição da Atividade | Subatividades | Horas | Dias |
|:----:|------------------------|---------------|:-----:|:----:|
| 9 | **Validação Pontual (Dev)** | | **27h** | **3** |
| 9.1 | | Testes unitários dos endpoints Protheus (payloads válidos e inválidos) | 7h | 0,8 |
| 9.2 | | Testes de integração Fluig → Protheus (fluxo completo de gravação) | 7h | 0,8 |
| 9.3 | | Testes de integração Portal → Fluig (abertura de processo externo) | 5h | 0,6 |
| 9.4 | | Simulação de cenários de erro (API IA indisponível, CNPJ inválido, Protheus offline) | 4h | 0,4 |
| 9.5 | | Correção de bugs identificados e reteste | 4h | 0,4 |
| 10 | **Validação Conjunta — UAT** | | **27h** | **3** |
| 10.1 | | Preparação de ambiente e massa de dados para homologação | 5h | 0,6 |
| 10.2 | | Apresentação do fluxo fim a fim para área de negócios do cliente | 4h | 0,4 |
| 10.3 | | Execução guiada de cenários pelo usuário-chave | 9h | 1,0 |
| 10.4 | | Registro e priorização de ajustes solicitados | 2h | 0,2 |
| 10.5 | | Implementação de ajustes finos pós-UAT | 5h | 0,6 |
| 10.6 | | Reteste e validação final com assinatura de aceite | 2h | 0,2 |
| | | **Subtotal Grupo C** | **54h** | **6** |

### GRUPO D — Implantação e Transição

| Item | Descrição da Atividade | Subatividades | Horas | Dias |
|:----:|------------------------|---------------|:-----:|:----:|
| 11 | **Go Live (Publicação, Handover, Capacitação)** | | **18h** | **2** |
| 11.1 | | Deploy do fluxo BPMN em produção (Fluig) | 3h | 0,3 |
| 11.2 | | Deploy dos endpoints REST em produção (Protheus) | 3h | 0,3 |
| 11.3 | | Deploy do Portal Público em produção (servidor web) | 2h | 0,2 |
| 11.4 | | Smoke test pós-deploy em ambiente produtivo | 3h | 0,3 |
| 11.5 | | Capacitação de usuários-chave (operação do fluxo e do portal) | 4h | 0,4 |
| 11.6 | | Handover técnico para equipe de sustentação do cliente | 3h | 0,3 |
| 12 | **Operação Assistida** | | **36h** | **4** |
| 12.1 | | Monitoramento de logs das integrações (IA, API CPF/CNPJ, Protheus) | 9h | 1,0 |
| 12.2 | | Suporte reativo a dúvidas operacionais dos usuários | 9h | 1,0 |
| 12.3 | | Correções emergenciais e ajustes de parametrização | 9h | 1,0 |
| 12.4 | | Estabilização e aceite final de operação assistida | 9h | 1,0 |
| 13 | **Documentação Técnica e Operacional** | | **27h** | **3** |
| 13.1 | | Documentação técnica das APIs REST do Protheus (endpoints, payloads, autenticação) | 7h | 0,8 |
| 13.2 | | Diagrama de arquitetura de integração (Fluig ↔ Protheus ↔ IA ↔ Portal) | 5h | 0,6 |
| 13.3 | | Manual operacional do fluxo BPMN (guia do aprovador) | 5h | 0,6 |
| 13.4 | | Manual do Portal Público (guia do fornecedor) | 5h | 0,6 |
| 13.5 | | Documento de encerramento do projeto | 5h | 0,6 |
| | | **Subtotal Grupo D** | **81h** | **9** |

---

### Resumo Consolidado de Volumetria

| Grupo | Descrição | Horas | Dias (9h) | % do Total |
|:-----:|-----------|:-----:|:---------:|:----------:|
| **A** | Mapeamento e Planejamento | 45h | 5 dias | 10,0% |
| **B** | Desenvolvimento | 270h | 30 dias | 60,0% |
| **C** | Validação e Qualidade | 54h | 6 dias | 12,0% |
| **D** | Implantação e Transição | 81h | 9 dias | 18,0% |
| | **TOTAL DO PROJETO** | **450h** | **50 dias** | **100%** |

> **Equivalência:** 50 dias úteis ≈ **10 semanas** ≈ **2,5 meses** (considerando 5 dias úteis/semana × 9h/dia).

---

## 3. Cronograma Macro de Execução

As atividades são divididas em 6 fases, permitindo paralelismo em tarefas independentes (ex: Protheus e Portal Público simultaneamente, caso haja mais de um recurso alocado).

> Considerando execução sequencial padrão com **1 Squad focado**.

```
Semana    1    2    3    4    5    6    7    8    9   10
        ├────┼────┼────┼────┼────┼────┼────┼────┼────┼────┤
FASE 1  ████                                              ← Mapeamento
FASE 2       █████████                                    ← Backend & Integrações
FASE 3                 ██████████████                     ← BPMN, Fluig & Portal
FASE 4                                ██████████          ← Validação & UAT
FASE 5                                          ████      ← Go Live & Docs
FASE 6                                               ████ ← Operação Assistida
```

### FASE 1 — Descoberta, Planejamento e Infraestrutura

| | Detalhes |
|--|---------|
| **Período** | Semana 1 |
| **Atividades** | 1 (Validações de acesso) e 2 (Overview de regras de negócio) |
| **Horas** | 45h — 5 dias |
| **Entregáveis** | ✅ Documento de Especificação de Regras de Negócio assinado |
| | ✅ Mapa de infraestrutura e arquitetura de integração validados |
| | ✅ Ambientes acessíveis e testados (Fluig, Protheus, APIs) |

### FASE 2 — Backend e Integrações Core

| | Detalhes |
|--|---------|
| **Período** | Semanas 2 e 3 |
| **Atividades** | 7 (Endpoints Protheus), 5 (API CPF/CNPJ) e 4 (Integração IA) |
| **Horas** | 108h — 12 dias |
| **Entregáveis** | ✅ APIs REST do Protheus publicadas e testadas (Postman/Insomnia) |
| | ✅ Integração com IA funcional e retornando JSON estruturado |
| | ✅ Integração com API de CPF/CNPJ funcional e validada |

### FASE 3 — Orquestração BPMN e Camada Visual

| | Detalhes |
|--|---------|
| **Período** | Semanas 4, 5 e 6 |
| **Atividades** | 3 (Fluig BPMN), 6 (Integração Fluig↔Protheus) e 8 (Portal Público) |
| **Horas** | 162h — 18 dias |
| **Entregáveis** | ✅ Fluxo BPMN completo no Fluig com formulários, eventos e alçadas |
| | ✅ Integração Fluig → Protheus operante (gravação de dados no ERP) |
| | ✅ Portal Público com login, cadastro de usuários e integração ao Fluig |

### FASE 4 — Qualidade e Homologação

| | Detalhes |
|--|---------|
| **Período** | Semanas 7 e 8 |
| **Atividades** | 9 (Validação Dev) e 10 (UAT com cliente) |
| **Horas** | 54h — 6 dias |
| **Entregáveis** | ✅ Relatório de testes técnicos executados (unitários e integração) |
| | ✅ Relatório de UAT validado e assinado pelo cliente |
| | ✅ Bugs críticos corrigidos e retestados |

### FASE 5 — Entrega, Lançamento e Documentação

| | Detalhes |
|--|---------|
| **Período** | Semana 9 |
| **Atividades** | 11 (Go Live), 13 (Documentação) e início da 12 (Operação Assistida) |
| **Horas** | 45h — 5 dias |
| **Entregáveis** | ✅ Sistema em produção (Fluig + Protheus + Portal) |
| | ✅ Documentação técnica e manuais operacionais entregues |
| | ✅ Usuários-chave capacitados |

### FASE 6 — Pós-Implantação

| | Detalhes |
|--|---------|
| **Período** | Semana 10 |
| **Atividades** | 12 (Conclusão da Operação Assistida) |
| **Horas** | 36h — 4 dias |
| **Entregáveis** | ✅ Logs de integração monitorados e estáveis |
| | ✅ Ajustes emergenciais concluídos |
| | ✅ **Termo de Encerramento do Projeto assinado** |

---

## 4. Matriz de Distribuição de Horas por Atividade

| Item | Atividade | Grupo | Horas | Dias |
|:----:|-----------|:-----:|:-----:|:----:|
| 1 | Validações de acesso iniciais | A | 18h | 2 |
| 2 | Overview de regras de negócios | A | 27h | 3 |
| 3 | Desenvolvimento BPMN (Fluig) | B | 63h | 7 |
| 4 | Integração com IA (NF) | B | 36h | 4 |
| 5 | Integração API CPF/CNPJ | B | 27h | 3 |
| 6 | Integração Fluig ↔ Protheus | B | 36h | 4 |
| 7 | Endpoints REST (Protheus/AdvPL) | B | 45h | 5 |
| 8 | Portal Público (Frontend + Auth) | B | 63h | 7 |
| 9 | Validação Pontual (Dev) | C | 27h | 3 |
| 10 | Validação Conjunta (UAT) | C | 27h | 3 |
| 11 | Go Live | D | 18h | 2 |
| 12 | Operação Assistida | D | 36h | 4 |
| 13 | Documentação | D | 27h | 3 |
| | **TOTAL** | | **450h** | **50 dias** |

---

## 5. Exclusões de Escopo

Para evitar ambiguidades, os seguintes itens **NÃO** estão contemplados neste escopo:

- Desenvolvimento ou treinamento de modelos de Inteligência Artificial (o modelo é fornecido e mantido pelo cliente).
- Customização de rotinas-padrão do Protheus além dos endpoints REST descritos.
- Migração de dados legados de outros sistemas.
- Desenvolvimento de relatórios ou dashboards gerenciais (BI).
- Integrações com sistemas além de Fluig, Protheus, IA e API de CPF/CNPJ.
- Mais de 1 (um) fluxo BPMN ou mais de 3 (três) níveis de alçada de aprovação.
- Aplicativo mobile nativo.
- Suporte pós-operação assistida (requer contrato de sustentação separado).
- Infraestrutura de servidores, cloud, domínio e certificados SSL.

---

## 6. Aceite

| | |
|--|--|
| **Aprovado por (Cliente):** | ________________________________________ |
| **Cargo:** | ________________________________________ |
| **Data:** | ____/____/________ |
| | |
| **Aprovado por (Fornecedor):** | ________________________________________ |
| **Cargo:** | ________________________________________ |
| **Data:** | ____/____/________ |

---

> *Este documento estabelece o escopo, volumetria e cronograma do projeto. Alterações de escopo deverão ser formalizadas via Change Request (CR) com análise de impacto em prazo e custo.*
