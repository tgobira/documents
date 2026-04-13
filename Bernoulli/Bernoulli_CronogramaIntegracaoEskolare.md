# Cronograma de Atividades: Integração TOTVS RM x Eskolare (Tools Nativas)
O planejamento contempla um ciclo de implantação estimado em **8 semanas**, focado no uso exclusivo de ferramentas internas do TOTVS RM (Metadados, Fórmulas Visuais, RM Reports e Conceito), visto que o ERP não trabalha com entregáveis modulares externos. O trabalho será conduzido por Especialistas TOTVS com domínio em integrações e backoffice.
## Fase 1: Iniciação, Planejamento e Mapeamento (Semanas 1 e 2)
*Foco: Alinhamento, estudo das APIs e de-para técnico focado no dicionário de dados do TOTVS.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Kick-off do Projeto** | Reunião de abertura para apresentação do escopo, limitações técnicas do ERP, cronograma e time. | Gerente de Projetos | Semana 1 |
| **Validação de Infraestrutura e Rede** | Liberação de acessos ao TOTVS (Ambiente DEV) e liberação de regras de Firewall no App Server para chamadas Outbound à API Eskolare. | Infraestrutura / TI | Semana 1 |
| **Estudo de APIs Eskolare** | Análise técnica do Swagger da Eskolare (Clientes e Pedidos) para entender o formato do JSON e a paginação a ser tratada nas Fórmulas Visuais. | Especialista TOTVS (Integração) | Semana 1 |
| **Refinamento de Regras de Negócio** | Workshops com áreas Comercial e Financeira para definir parametrizações padrão do RM (Tipo de Movimento, CFOP, Tabelas de Preço, Conta Caixa, Condições de Pgto). | Analista TOTVS / Key Users | Semana 2 |
| **Documento de Mapeamento (De-Para)** | Mapeamento técnico detalhando de qual nó do JSON da Eskolare a informação sairá e em qual Tabela/Campo do TOTVS RM (incluindo campos complementares) ela será salva. | Analista TOTVS | Semana 2 |
| **Aprovação do Mapeamento** | Validação e aprovação formal (Sign-off) do documento de regras de negócio e De-Para. | Key Users / Patrocinador | Semana 2 |
## Fase 2: Desenvolvimento da Ingestão de Dados - Ambiente DEV (Semana 3)
*Foco: Criação do Staging Area e consumo da API Eskolare utilizando Metadados e Fórmulas Visuais.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Criação de Cadastros via Metadados** | Parametrização e publicação das tabelas customizadas (Tabelas Z) no TOTVS RM, incluindo telas de visão e edição via funcionalidade de Metadados. | Especialista TOTVS (Backoffice) | Semana 3 |
| **Consumo da API (Fórmula Visual - REST)** | Construção de Fórmula Visual utilizando *Activities* nativas de requisição HTTP (REST) para consumir a API da Eskolare e gerenciar o Token de autenticação. | Especialista TOTVS (Integração) | Semana 3 |
| **Parser JSON (CodeActivity C# / Transformação)** | Desenvolvimento da lógica de deserialização do JSON dentro da Fórmula Visual (usando CodeActivity/C# embarcado se necessário) para realizar a inclusão dos dados nos Metadados. | Especialista TOTVS (Integração) | Semana 3 |
| **Controle de Execução e Job Server** | Implementação de lógica na Fórmula Visual para armazenar a Data/Hora da última execução bem sucedida e parametrização no Agendador de Tarefas do RM. | Especialista TOTVS (Integração) | Semana 3 |
## Fase 3: Desenvolvimento de Automações TOTVS e Relatórios - Ambiente DEV (Semanas 4 e 5)
*Foco: Processamento segregado para Nucleus e Fluxus utilizando Fórmulas Visuais/Conceito e criação de relatórios.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Automação Comercial (RM Nucleus)** | Construção de Fórmula Visual e/ou *Conceito* que varre a tabela de Metadados (Pedidos) e aciona as rotinas internas do RM para criar o Movimento de Venda (TMOV/TITMMOV). | Especialista TOTVS (Backoffice) | Semana 4 |
| **Automação Financeira (RM Fluxus)** | Construção de rotina segregada (Fórmula Visual) que lê a tabela de Metadados (Pagamentos) e cria as faturas/baixas financeiras no Fluxus (FLAN/FXCX). | Especialista TOTVS (Backoffice) | Semana 4 |
| **Mapeamento e Tratamento de Exceções** | Captura de exceções nativas do RM (ex: erro de tributação, produto inativo) e gravação destas mensagens de volta na tabela de Metadados para auditoria. | Especialista TOTVS (Integração) | Semana 4 |
| **Desenvolvimento de Relatórios (RM Reports)** | Criação de relatórios gerenciais nativos: 1. Painel de Auditoria/Log; 2. Conciliação Comercial (Eskolare x Nucleus); 3. Conciliação Financeira (Eskolare x Fluxus). | Especialista TOTVS (Backoffice) | Semana 5 |
| **Testes Unitários no TOTVS** | Execução manual das Fórmulas Visuais no ambiente de DEV para garantir a correta gravação do banco de dados de ponta a ponta. | Equipe Técnica | Semana 5 |
## Fase 4: Homologação e Testes (UAT) - Ambiente HOM (Semana 6)
*Foco: Validação da solução pelos usuários finais simulando cenários reais da plataforma.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Preparação do Ambiente HOM** | Exportação dos processos construídos em DEV (Metadados, Fórmulas Visuais, Conceitos e RM Reports) e importação no ambiente de Homologação. | Equipe Técnica | Semana 6 |
| **Execução de Testes Integrados (UAT)** | Key Users realizam geração de pedidos/pagamentos na Eskolare (Sandbox) e acompanham o processamento dos Jobs no TOTVS RM. | Key Users | Semana 6 |
| **Validação dos RM Reports** | Extração e conferência dos relatórios gerenciais construídos, cruzando as informações com o painel web da plataforma Eskolare. | Key Users | Semana 6 |
| **Bug Fixing de Parametrizações** | Correção de desvios fiscais, falhas na Fórmulas Visuais ou ajustes nos mapeamentos identificados durante o UAT. | Equipe Técnica | Semana 6 |
| **Aprovação para Produção (Sign-off)** | Assinatura formal atestando que a parametrização e as automações construídas no TOTVS atendem às regras de negócio. | Key Users / Patrocinador | Semana 6 |
## Fase 5: Go-Live e Operação Assistida - Ambiente PRD (Semanas 7 e 8)
*Foco: Migração definitiva dos recursos nativos, apontamento para ambiente real e suporte.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Janela de GMUD e Deploy** | Planejamento da exportação/importação (Metadados, Fórmulas Visuais, Reports) de Homologação para Produção, garantindo backup do banco PRD. | TI / Gerente de Projetos | Semana 7 |
| **Configuração Final (Go-Live)** | Apontamento da Fórmula Visual para a URL Produtiva da Eskolare, inserção do Token Oficial e ativação dos processos no Agendador do RM (Job Server). | Especialista TOTVS (Integração) | Semana 7 |
| **Operação Assistida (Hypercare)** | Monitoramento diário (via RM Reports e Log do Job Server) da execução das Fórmulas Visuais produtivas, garantindo o processamento correto da fila. | Equipe Técnica / TI | Semanas 7 e 8 |
| **Handover Sistêmico** | Treinamento técnico da equipe de sustentação interna, demonstrando onde reprocessar fórmulas visuais, verificar metadados e analisar logs de erro nativos. | Especialista TOTVS | Semana 8 |
| **Encerramento do Projeto** | Reunião formal de fechamento com as áreas de negócio e transição da integração para o suporte padrão (Sustentação). | Gerente de Projetos | Semana 8 |
