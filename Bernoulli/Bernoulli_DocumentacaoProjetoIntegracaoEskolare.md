# Documentação de Execução de Projeto: Integração TOTVS RM x Eskolare
## 1. Visão Geral do Projeto
Este projeto visa estabelecer uma integração automatizada, ativa e de ponta a ponta entre a plataforma de vendas **Eskolare** e o ERP **TOTVS RM**. O objetivo é garantir que os dados de Clientes, Pedidos de Venda e Pagamentos gerados na Eskolare sejam consumidos e processados pelo TOTVS RM.
**Premissa Arquitetônica Fundamental:** O ERP TOTVS RM não permite a construção modular de entregáveis externos para este cenário. Portanto, **todo o desenvolvimento será realizado estritamente através de funcionalidades internas e nativas do TOTVS RM**. O projeto será conduzido por Especialistas TOTVS com domínio em backoffice, automações internas e integrações de consumo de APIs externas (REST).
## 2. Arquitetura Técnica e Ferramentas Nativas
A integração utilizará a arquitetura de *Polling* ativo, onde o TOTVS RM atua como cliente (Client) realizando chamadas periódicas à API Eskolare. As ferramentas nativas utilizadas serão:
* **Metadados (Tabelas Z):** Para a criação de cadastros personalizados que atuarão como área de transição (*Staging Area*) dentro do banco de dados do RM.
* **Fórmulas Visuais (Activities REST / CodeActivity C#):** Para realizar a autenticação (Token), consumo HTTP (GET/POST), desserialização do JSON da Eskolare e gravação na *Staging Area*.
* **Fórmulas Visuais / Conceito:** Para a leitura dos dados em *Staging* e criação nativa e validada dos Movimentos e Lançamentos.
* **Gerador de Relatórios (RM Reports):** Para a criação dos painéis de conciliação e auditoria.
* **Job Server (Agendador de Tarefas):** Para orquestrar e agendar a execução periódica e em *background* das Fórmulas Visuais.
## 3. Fluxo de Dados (End-to-End) e Segregação
O processamento ocorrerá em etapas estritamente divididas, garantindo resiliência e independência entre as áreas de negócio:
1. **Ingestão de Dados (Inbound):** Uma Fórmula Visual agendada dispara uma requisição `GET` para a Eskolare. O JSON de retorno é lido e gravado nos Metadados (Cadastros Personalizados de Clientes, Pedidos e Pagamentos).
2. **Processamento Segregado (Automações Internas):**
   * **Automação A (Comercial - RM Nucleus):** Uma rotina (Fórmula Visual/Conceito) lê a tabela de Metadados de Pedidos e, aplicando regras de negócio de faturamento, gera o Pedido de Venda oficial no TOTVS (TMOV/TITMMOV).
   * **Automação B (Financeira - RM Fluxus):** Uma rotina **totalmente independente** lê a tabela de Metadados de Pagamentos e gera os Lançamentos Financeiros (Contas a Receber/Baixas) no TOTVS (FLAN/FXCX). *Um erro na Automação A jamais impactará a execução da Automação B, e vice-versa.*
## 4. Escopo de Entidades e Mapeamento Técnico
Durante a fase inicial, haverá um intenso trabalho de refinamento de regras de negócio com os *Key Users* e estudo dos serviços de API da Eskolare (Swagger) para compor o **Documento de Mapeamento (De-Para)**.
* **Clientes (FCFO):** Consumo de compradores (Alunos/Responsáveis). Mapeamento de CPF/CNPJ, Nome, Contatos e Endereço.
* **Pedidos de Venda (Comercial/Estoque):** Parametrização de Filial, Local de Estoque, Tipo de Movimento, Natureza de Operação (CFOP) e Regras de Tributação por SKU.
* **Lançamentos Financeiros (Caixa/Bancos):** Parametrização de Tipo de Documento, Meio de Pagamento, Conta Caixa, Condição de Pagamento e Datas (Vencimento/Emissão).
## 5. Ciclo de Ambientes e Implantação
Toda implementação respeitará o ciclo de vida seguro de desenvolvimento de software, não havendo desenvolvimentos diretos em produção.
1. **Ambiente de Desenvolvimento (DEV):** Criação dos Metadados, Fórmulas Visuais e RM Reports. Realização de testes unitários técnicos mockando chamadas à API Sandbox da Eskolare.
2. **Ambiente de Homologação (HOM / UAT):** Exportação dos recursos de DEV e importação em HOM. Realização de Testes Integrados (UAT - User Acceptance Testing) simulando operações reais da plataforma Eskolare (Sandbox) até a ponta (Nucleus/Fluxus). Somente após o *Sign-off* das áreas o projeto avança.
3. **Ambiente de Produção (PRD / Go-Live):** Planejamento de Janela de Mudança (GMUD). Migração dos objetos sistêmicos nativos (Metadados, Fórmulas, Reports) para a Produção. Configuração das credenciais oficiais da Eskolare, ativação no Job Server e início da Operação Assistida (*Hypercare*).
## 6. Relatórios Gerenciais e Auditoria (RM Reports)
Para garantir transparência operacional e facilitar a conferência por parte dos usuários, serão desenvolvidos relatórios gerenciais nativos:
1. **Painel de Auditoria e Log de Integração:** Relatório técnico que exibe o histórico de execuções do Job Server, volume de registros importados (JSON -> Metadados) e alertas de falhas de comunicação (HTTP Status, Timeout, Token Expirado).
2. **Relatório de Efetivação Comercial (Conciliação de Pedidos):** Lista todos os pedidos consumidos da Eskolare contra os gerados no TOTVS. Exibe: *ID Eskolare | Cliente | Valor Total | Status Staging | ID Movimento TOTVS (TMOV) | Mensagens de Erro de Regra de Negócio*.
3. **Relatório de Conciliação Financeira:** Lista os dados financeiros integrados para conferência de caixa pela Tesouraria. Exibe: *ID Pedido Eskolare | Meio de Pagamento | Valor | ID Lançamento Fluxus (FLAN) | Status (Aberto/Baixado) | Mensagens de Erro Financeiro*.
## 7. Tratamento de Erros e Resiliência
A integração será construída considerando alta disponibilidade e rastreabilidade:
* **Controle de Ponto de Parada (Watermark):** A Fórmula Visual gravará a Data/Hora da última sincronização com sucesso. Se o TOTVS ficar fora do ar, ao retornar, ele buscará os registros retroativos a partir desta data, não perdendo dados da Eskolare.
* **Mecanismo de Retry (Inbound):** Indisponibilidades na API Eskolare (erro 500, timeout) não travarão a integração. O erro será persistido no Log, e a Fórmula Visual tentará o consumo na próxima janela de agendamento.
* **Captura de Exceções Nativas:** Falhas de regra de negócio do próprio TOTVS (ex: *Produto inativo*, *Cliente sem endereço preenchido*) levantadas durante a criação do Movimento/Lançamento não travarão o lote. A rotina fará um *Catch* da exceção, atualizará o status na tabela de Metadados para "Erro" e continuará o processamento, deixando o alerta no RM Report.
---
## 8. Cronograma de Atividades
O planejamento contempla um ciclo de implantação estimado em **8 semanas**, conduzido por Especialistas TOTVS.
### Fase 1: Iniciação, Planejamento e Mapeamento (Semanas 1 e 2)
*Foco: Alinhamento, estudo das APIs e de-para técnico focado no dicionário de dados do TOTVS.*
| Atividade | Descrição Detalhada | Responsável | Previsão |
| :--- | :--- | :--- | :--- |
| **Kick-off do Projeto** | Reunião de abertura para apresentação do escopo, limitações técnicas do ERP, cronograma e time. | Gerente de Projetos | Sem. 1 |
| **Validação de Infraestrutura** | Liberação de acessos ao TOTVS (DEV) e regras de Firewall no App Server para chamadas Outbound. | Infra/TI | Sem. 1 |
| **Estudo de APIs Eskolare** | Análise técnica do Swagger da Eskolare para entender o formato JSON e paginação. | Especialista TOTVS | Sem. 1 |
| **Refinamento de Regras** | Workshops com áreas Comercial e Financeira para definir parametrizações padrão do RM. | Analista TOTVS | Sem. 2 |
| **Mapeamento (De-Para)** | Mapeamento detalhando de qual nó do JSON a informação sairá e em qual Tabela/Campo do RM será salva. | Analista TOTVS | Sem. 2 |
| **Aprovação do Mapeamento** | Validação e aprovação formal (Sign-off) do documento de regras de negócio. | Key Users | Sem. 2 |
### Fase 2: Desenvolvimento da Ingestão de Dados - Ambiente DEV (Semana 3)
*Foco: Criação do Staging Area e consumo da API Eskolare utilizando Metadados e Fórmulas Visuais.*
| Atividade | Descrição Detalhada | Responsável | Previsão |
| :--- | :--- | :--- | :--- |
| **Cadastros via Metadados** | Parametrização das tabelas customizadas (Tabelas Z) no TOTVS RM para atuar como Staging. | Especialista TOTVS | Sem. 3 |
| **Consumo da API (REST)** | Construção de Fórmula Visual (Activities REST) para consumir a API Eskolare e gerenciar o Token. | Especialista TOTVS | Sem. 3 |
| **Parser JSON** | Desenvolvimento da lógica de desserialização (CodeActivity C#) e inclusão nos Metadados. | Especialista TOTVS | Sem. 3 |
| **Controle de Execução** | Lógica de Watermark (Data/Hora) e parametrização no Job Server do RM. | Especialista TOTVS | Sem. 3 |
### Fase 3: Desenvolvimento de Automações TOTVS e Relatórios - Ambiente DEV (Semanas 4 e 5)
*Foco: Processamento segregado para Nucleus e Fluxus e criação de relatórios.*
| Atividade | Descrição Detalhada | Responsável | Previsão |
| :--- | :--- | :--- | :--- |
| **Automação Comercial** | Fórmula Visual/Conceito que varre os Metadados e cria o Movimento de Venda (Nucleus). | Especialista TOTVS | Sem. 4 |
| **Automação Financeira** | Rotina segregada que lê Metadados e cria faturas/baixas financeiras (Fluxus). | Especialista TOTVS | Sem. 4 |
| **Tratamento de Exceções** | Captura de exceções nativas do RM e gravação das mensagens nos Metadados. | Especialista TOTVS | Sem. 4 |
| **Relatórios (RM Reports)** | Criação dos 3 relatórios gerenciais (Log, Comercial e Financeiro). | Especialista TOTVS | Sem. 5 |
| **Testes Unitários (DEV)** | Execução manual das rotinas em DEV para garantir a gravação de ponta a ponta. | Equipe Técnica | Sem. 5 |
### Fase 4: Homologação e Testes (UAT) - Ambiente HOM (Semana 6)
*Foco: Validação da solução pelos usuários finais simulando cenários reais.*
| Atividade | Descrição Detalhada | Responsável | Previsão |
| :--- | :--- | :--- | :--- |
| **Preparação do Amb. HOM** | Exportação dos processos (DEV) e importação em Homologação. | Equipe Técnica | Sem. 6 |
| **Testes Integrados (UAT)** | Geração de pedidos na Eskolare (Sandbox) e acompanhamento do processamento no TOTVS. | Key Users | Sem. 6 |
| **Validação de Relatórios** | Extração e conferência dos RM Reports, cruzando com o painel da Eskolare. | Key Users | Sem. 6 |
| **Bug Fixing** | Correção de desvios fiscais ou falhas nas Fórmulas Visuais. | Equipe Técnica | Sem. 6 |
| **Aprovação (Sign-off)** | Assinatura formal atestando que as automações atendem às regras de negócio. | Key Users | Sem. 6 |
### Fase 5: Go-Live e Operação Assistida - Ambiente PRD (Semanas 7 e 8)
*Foco: Migração definitiva, apontamento para ambiente real e suporte.*
| Atividade | Descrição Detalhada | Responsável | Previsão |
| :--- | :--- | :--- | :--- |
| **Janela de GMUD** | Planejamento da exportação/importação para Produção e backup do banco PRD. | TI / Projetos | Sem. 7 |
| **Go-Live** | Apontamento para URL Produtiva, inserção do Token Oficial e ativação no Job Server. | Especialista TOTVS | Sem. 7 |
| **Hypercare** | Monitoramento diário das rotinas produtivas e logs do Job Server. | Equipe / TI | Sem. 7 e 8 |
| **Handover Sistêmico** | Treinamento técnico da equipe de sustentação interna sobre fluxos e tratamento de erros. | Especialista TOTVS | Sem. 8 |
| **Encerramento** | Reunião formal de fechamento e transição para o suporte padrão. | Gerente de Projetos | Sem. 8 |
