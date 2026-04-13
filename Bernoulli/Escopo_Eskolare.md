# Planejamento de Micro Atividades e Alocação de Horas (Equipe Técnica TOTVS)
**Projeto:** Integração TOTVS RM x Eskolare
**Premissas:** 1 Dia Útil = 9 Horas. Escopo estritamente técnico TOTVS.
---
## Fase 1: Iniciação, Planejamento e Mapeamento (Semanas 1 e 2)
**Objetivo:** Entendimento técnico das duas APIs, mapeamento do dicionário de dados TOTVS e regras de negócio.
| Micro Atividade | Detalhamento | Responsável | Esforço (h) |
| :--- | :--- | :--- | :--- |
| Análise de APIs (Vendas e Fin.) | Estudo técnico de endpoints, paginação e JSONs distintos para Clientes/Pedidos e Lançamentos. | Especialista TOTVS | 12h |
| Workshop: Regras Comerciais | Condução de reunião para definir Tipo de Movimento, CFOP, Local de Estoque e Tributação. | Analista TOTVS | 6h |
| Workshop: Regras Financeiras | Condução de reunião para definir Tipo de Documento, Conta Caixa e Condições de Pgto. | Analista TOTVS | 6h |
| Documento De-Para | Escrita técnica do mapeamento JSON x RM abordando os dois fluxos (Comercial e Financeiro). | Analista TOTVS | 16h |
| **Subtotal Fase 1** | | | **40h** |
---
## Fase 2: Desenvolvimento da Ingestão de Dados - DEV (Semana 3)
**Objetivo:** Criar a *Staging Area* (Metadados) e construir as requisições HTTP segregadas para as duas APIs da Eskolare.
| Micro Atividade | Detalhamento | Responsável | Esforço (h) |
| :--- | :--- | :--- | :--- |
| Modelagem de Metadados | Criação das Tabelas Z no banco de dados (Clientes, Pedidos e Pagamentos) via RM. | Especialista TOTVS | 8h |
| Telas e Visões (Metadados) | Parametrização das telas de visualização e edição das tabelas Z para os usuários. | Especialista TOTVS | 4h |
| FV REST: Autenticação | Criação de Fórmula Visual para consumo do endpoint de Token/Login (comum a ambas). | Especialista TOTVS | 6h |
| **FV REST: API Vendas** | Criação das chamadas HTTP GET na FV específicas para buscar Clientes e Pedidos. | Especialista TOTVS | 12h |
| **CodeActivity: Parser Vendas**| Escrita de script C# embutido na FV para desserializar o JSON de Vendas e gravar nos Metadados. | Especialista TOTVS | 16h |
| **FV REST: API Financeira** | Criação das chamadas HTTP GET na FV específicas para buscar Pagamentos/Lançamentos. | Especialista TOTVS | 10h |
| **CodeActivity: Parser Fin.** | Escrita de script C# embutido na FV para desserializar o JSON Financeiro e gravar nos Metadados. | Especialista TOTVS | 14h |
| Lógica de Watermark/Retry | Implementação da gravação da última execução com sucesso para os dois fluxos independentes. | Especialista TOTVS | 8h |
| **Subtotal Fase 2** | | | **78h** |
---
## Fase 3: Desenvolvimento de Automações TOTVS e Relatórios - DEV (Semanas 4 e 5)
**Objetivo:** Geração oficial de Movimentos e Lançamentos, além da construção dos painéis de auditoria.
| Micro Atividade | Detalhamento | Responsável | Esforço (h) |
| :--- | :--- | :--- | :--- |
| FV/Conceito Comercial | Construção da rotina que lê Metadados e salva TMOV/TITMMOV (Nucleus). | Especialista TOTVS | 24h |
| FV/Conceito Comercial (Erros)| Lógica de Catch para gravar erros fiscais/cadastrais de volta nos Metadados. | Especialista TOTVS | 8h |
| FV/Conceito Financeiro | Construção da rotina segregada que lê Metadados e salva FLAN/FXCX (Fluxus). | Especialista TOTVS | 20h |
| FV/Conceito Financeiro (Erros)| Lógica de Catch para gravar erros financeiros de volta nos Metadados. | Especialista TOTVS | 8h |
| RM Report: Log Auditoria | Desenho de relatório listando requisições, status HTTP e volume de dados ingeridos. | Analista TOTVS | 8h |
| RM Report: Conciliação Ven. | Desenho de relatório cruzando ID Eskolare vs ID Movimento TOTVS e listagem de erros. | Analista TOTVS | 8h |
| RM Report: Conciliação Fin. | Desenho de relatório cruzando Pagamento Eskolare vs Lançamento Fluxus e status de baixa. | Analista TOTVS | 8h |
| Testes Unitários DEV | Execuções e depuração no RM para validar a gravação de ponta a ponta na base DEV. | Especialista TOTVS | 16h |
| **Subtotal Fase 3** | | | **100h** |
---
## Fase 4: Homologação e Testes (UAT) - HOM (Semana 6)
**Objetivo:** Apoio técnico durante a validação integrada em ambiente simulando produção.
| Micro Atividade | Detalhamento | Responsável | Esforço (h) |
| :--- | :--- | :--- | :--- |
| Migração DEV -> HOM | Exportação e importação técnica de Metadados, Fórmulas Visuais, Conceitos e RM Reports. | Especialista TOTVS | 6h |
| Configuração Amb. HOM | Apontamentos para API Sandbox (Vendas e Fin.) e configuração de Job Server. | Especialista TOTVS | 4h |
| Suporte aos Testes (UAT) | Acompanhamento técnico durante as simulações de negócio realizadas pelo cliente. | Analista TOTVS | 12h |
| Bug Fixing (Correções) | Ajustes em regras fiscais/financeiras e correções dos dois fluxos de integração mapeados. | Especialista TOTVS | 20h |
| **Subtotal Fase 4** | | | **42h** |
---
## Fase 5: Go-Live e Operação Assistida - PRD (Semanas 7 e 8)
**Objetivo:** Implantação produtiva, acompanhamento pós-virada e entrega de documentação.
| Micro Atividade | Detalhamento | Responsável | Esforço (h) |
| :--- | :--- | :--- | :--- |
| Deploy PRD (Migração) | Exportação de HOM e importação definitiva na base de Produção. | Especialista TOTVS | 6h |
| Go-Live (Setup PRD) | Inserção de Tokens Produtivos, apontamento URLs Oficiais e ativação dos Jobs. | Especialista TOTVS | 4h |
| **Acompanhamento (Hypercare)**| Monitoramento de execuções das duas APIs (Vendas/Fin.) por **2 dias úteis (18h)**. | Especialista TOTVS | **18h** |
| **Documentação Técnica** | Desenvolvimento de doc. detalhada sobre entregáveis, configurações e regras (Vendas e Fin.).| Analista TOTVS | **22,5h** |
| Treinamento Sustentação | Sessão técnica de repasse de conhecimento (Como reprocessar, ler logs e reiniciar rotinas). | Especialista TOTVS | 4h |
| **Subtotal Fase 5** | | | **54,5h** |
---
## Resumo de Alocação de Esforço (Equipe TOTVS)
Abaixo está a consolidação exclusiva do esforço técnico da equipe TOTVS, acrescida da margem de segurança (risco de execução) solicitada de 20%.
| Perfil Profissional | Horas Estimadas |
| :--- | :--- |
| **Especialista TOTVS** (Desenvolvimento / Deploy / Hypercare das duas APIs) | 228,0h |
| **Analista TOTVS** (Mapeamento / Reports / Documentação) | 86,5h |
| **[ A ] Total de Horas Base da Equipe Técnica** | **314,5h** |
| **[ B ] Margem de Risco de Execução (20%)** | **62,9h** |
| **[ A + B ] ESFORÇO TOTAL ESTIMADO** | **377,4h** |
