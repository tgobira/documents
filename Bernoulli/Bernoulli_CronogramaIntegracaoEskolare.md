# Cronograma de Atividades: Integração TOTVS RM x Eskolare
O planejamento contempla um ciclo de vida completo de engenharia de software e implantação sistêmica, estimado em **8 semanas**, garantindo todas as validações, segregação de rotinas e transição segura entre os ambientes (Desenvolvimento, Homologação e Produção).
## Fase 1: Iniciação, Planejamento e Mapeamento (Semanas 1 e 2)
*Foco: Alinhamento de expectativas, garantia de infraestrutura, estudo técnico e definição de regras de negócio.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Kick-off do Projeto** | Reunião de abertura com todos os *stakeholders* para apresentação do escopo, premissas, cronograma e time do projeto. | Gerente de Projetos | Semana 1 |
| **Validação de Acessos e Infraestrutura** | Liberação de acessos ao TOTVS (Ambiente DEV), credenciais da API Eskolare (Sandbox) e liberação de regras de Firewall (Outbound). | Infraestrutura / TI | Semana 1 |
| **Estudo de APIs Eskolare** | Análise técnica do Swagger/Postman Collection da Eskolare, mapeando *endpoints*, paginação e retornos (JSON) de Clientes e Pedidos. | Desenvolvedor Integração | Semana 1 |
| **Refinamento de Regras de Negócio** | Workshops com áreas Comercial, Fiscal e Financeira para definir parametrizações (Tipo de Movimento, CFOP, Impostos, Conta Caixa, Condição de Pagamento). | Analista TOTVS / Key Users | Semana 2 |
| **Documento de Mapeamento (De-Para)** | Elaboração do documento técnico com a correlação de campos entre o JSON da Eskolare e as tabelas/campos do TOTVS RM. | Analista TOTVS | Semana 2 |
| **Aprovação do Mapeamento** | Validação e aprovação formal (Sign-off) do documento de regras de negócio e De-Para pelas áreas envolvidas. | Key Users / Patrocinador | Semana 2 |
## Fase 2: Desenvolvimento da Ingestão de Dados - Ambiente DEV (Semana 3)
*Foco: Construção da camada de recepção dos dados (Staging) e comunicação HTTP com a plataforma.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Criação de Cadastros Personalizados** | Modelagem e criação das tabelas customizadas (Metadados/Tabelas Z) no banco de dados do TOTVS para servir como *Staging Area*. | Desenvolvedor TOTVS | Semana 3 |
| **Rotina de HTTP Request / Autenticação** | Desenvolvimento da automação TOTVS para realizar o *login/token* e as requisições `GET` na API da Eskolare. | Desenvolvedor Integração | Semana 3 |
| **Parser JSON e Gravação (Staging)** | Desenvolvimento da lógica de leitura do JSON (Clientes, Pedidos, Pagamentos) e rotina de `INSERT/UPDATE` nos cadastros personalizados. | Desenvolvedor Integração | Semana 3 |
| **Mecanismo de Controle (Watermark/Retry)**| Implementação da gravação da Data/Hora da última sincronização para controle de fila e tratamento de falhas de comunicação. | Desenvolvedor Integração | Semana 3 |
## Fase 3: Desenvolvimento de Automações TOTVS e Relatórios - Ambiente DEV (Semanas 4 e 5)
*Foco: Processamento interno segregado e criação das ferramentas de visibilidade/auditoria.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Automação Comercial (RM Nucleus)** | Criação da rotina que lê a tabela de *Staging* e gera o **Pedido de Venda**, aplicando as regras fiscais, itens e clientes. | Desenvolvedor TOTVS | Semana 4 |
| **Automação Financeira (RM Fluxus)** | Criação da rotina **segregada e independente** que lê a tabela de *Staging* e gera o **Lançamento Financeiro** / Baixa. | Desenvolvedor TOTVS | Semana 4 |
| **Tratamento de Exceções Nativas** | Implementação de retorno de erros nativos do TOTVS (ex: produto sem preço) para atualização do *Status* no cadastro personalizado. | Desenvolvedor TOTVS | Semana 4 |
| **Desenvolvimento de Relatórios (Reports)** | Construção dos 3 relatórios gerenciais: 1. Auditoria/Log; 2. Conferência de Pedidos; 3. Conciliação Financeira. | Analista TOTVS | Semana 5 |
| **Testes Unitários e Integrados (DEV)** | Execução do fluxo completo de ponta a ponta no ambiente de Desenvolvimento pela equipe técnica. | Equipe Técnica | Semana 5 |
## Fase 4: Homologação e Testes (UAT) - Ambiente HOM (Semana 6)
*Foco: Validação da solução pelos usuários finais (Key Users) em ambiente controlado simulando a produção.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Preparação do Ambiente HOM** | Migração das customizações (DEV -> HOM) e configuração das credenciais Eskolare Sandbox no ambiente de Homologação. | Equipe Técnica / TI | Semana 6 |
| **Execução de Testes (UAT)** | Key Users realizam massa de testes: simulação de vendas na Eskolare e conferência da geração de Pedidos e Lançamentos no TOTVS. | Key Users | Semana 6 |
| **Validação dos Relatórios Gerenciais** | Extração e conferência dos dados gerados nos relatórios de auditoria e conciliação construídos na Fase 3. | Key Users | Semana 6 |
| **Ajustes e Correções (Bug Fixing)** | Correção de possíveis desvios de regras de negócio, falhas fiscais ou mapeamentos incorretos identificados durante o UAT. | Equipe Técnica | Semana 6 |
| **Aprovação para Produção (Sign-off)** | Assinatura formal de aceite técnico e de negócios atestando que a integração está pronta para o Go-Live. | Key Users / Patrocinador | Semana 6 |
## Fase 5: Go-Live e Operação Assistida - Ambiente PRD (Semanas 7 e 8)
*Foco: Implantação produtiva, ativação definitiva e suporte intenso pós-implantação.*
| Atividade | Descrição Detalhada | Responsável(eis) | Previsão |
| :--- | :--- | :--- | :--- |
| **Janela de Mudança (GMUD)** | Planejamento da janela de deploy, garantindo backup e plano de *rollback* do ambiente de Produção. | TI / Gerente de Projetos | Semana 7 |
| **Migração para Produção** | Exportação/Importação dos recursos (Metadados, Fórmulas, RM Reports) de Homologação para Produção. | Equipe Técnica / TI | Semana 7 |
| **Configuração e Ativação (Go-Live)** | Configuração do Token Eskolare Produtivo e ativação do Agendador (Job Server) no TOTVS para execução periódica. | Equipe Técnica | Semana 7 |
| **Operação Assistida (Hypercare)** | Acompanhamento diário das primeiras integrações reais. Monitoramento ativo dos logs e relatórios para ação imediata em caso de erros. | Equipe Técnica / TI | Semanas 7 e 8 |
| **Passagem de Conhecimento (Handover)** | Treinamento e entrega de documentação técnica para a equipe de Sustentação/Suporte Interno que assumirá o monitoramento contínuo. | Equipe Técnica | Semana 8 |
| **Encerramento do Projeto** | Reunião de fechamento, apresentação de resultados e transição oficial do projeto para a rotina de sustentação. | Gerente de Projetos | Semana 8 |
