# Bernoulli

Maurício

---

## Integração com plataforma Eskolare

### Já possuem
1. TOTVS RM implantado para execução de vendas de livros
2. Queries para consumo dos dados cadastrados no TOTVS
3. Plataforma Eskolare já executa chamadas ao TOTVS RM para retroalimentação

### Atenção
1. Alta volumetria de requisições
2. Financeiro é gerado previamente ao faturamento

### Propostas
1. Tabela intermediária para receber requisições
    - Pedidos - Movimentação de PEDIDO DE VENDA (2.1.63)
    - Pagamentos - Faturamento automático do 
    - Entrega de materiais - 
2. Automação para leitura da tabela intermediária com lógica para execução de cada uma das atividades
3. Tabela intermediária com LOG de execução da tentativa de integração

---

## Reunião - 2026.04.10

Talita / Thierny / Yuri / Svizzero / Maurício

*Atualmente possuem 2 dores*
1. Funcionamento Core
2. Melhorias operacionais para equipe responsável

### Estrutura
- Eskolare = plataforma
- TOTVS = ERP

### AS IS
- Eskolare = plataforma de vendas utilizada pelo Bernoulli
- Venda de material didático para aluno de escolas parceiras
- Não é manual o processo
    - Integração desenvolvida pela Worknow
    - Ponto de falha: tempo de resposta inconsistente
- 2 tipos de usuários
    - Interno: usuários da operação de backoffice
    - Externo: atendido pela plataforma da Eskolare
- Integração atual faz:
    - Carga de pedido
    - Carga de pagamento
    - Logística
- Operação financeira sobre a gestão de recebíveis de cartão de crédito executada através do Extrato de Caixa - compensação

### Objetivo Final
- Reestruturação da integração inicial desenvolvida pela WorkNow para atender a necessidade da Bernoulli
    - Retirada da tecnologia da WorkNow e implementação de nova funcionalidade para entrega da solução
- Oportunidade: melhoria na experiência do usuário (interno)
    - Painel de acompanhamento do processo de compras

### Sincronicidade/Integração
- Integração assíncrona
    - Rotina da WorkNow é agendada (JOB configurável)
- Eskolare disponibiliza as APIs para comunicação de dados. Ela não aciona o TOTVS
- Toda integração foi desenvolvida diretamente pela WorkNow, integrando o TOTVS com Eskolare
    - Existe publicado dentro do TOTVS área de parametrização base para integração do TOTVS com Eskolare
    - Não é executada integração de entidades educacionais
    - É executado somente integração do BackOffice (Pedidos)
    - Configuração realizada por Unidade de Ensino
    - 1 Lançamento por venda realizada
    - Integração identifica parcelamentos para tratamento e direcionamento através do Extrato Finaceiro
    - TOTVS envia informações de NF para o Eskolare (movimento 2.2.40)
        - Item obrigatório para controle do Bernoulli, pois possibilita o saque do valor da venda realizada através do Eskolare
    - Integração de dados financeiros p/ execução de conciliação de vendas realizadas com valor recebido
        - Saque realizado na plataforma é realizada consulta de dados com todas as transações financeiras executadas
        - Conciliação realizada gerando lançamentos no TOTVS para equalização
        - Extratificação de todo transacionamento financeiro para sensibilização do financeiro do TOTVS (recebimento - venda, pagamento - taxas e devoluções)
        - Lançamento financeiro gerado a partir do movimento 2.2.40 (Emissão de NF)
    - Estrutura de integração do pedido já contempla dados de CLIENTE. Essa informação é interpretada para tomada de decisão sobre a criação de novo cliente ou atualização de cliente existente
    - 
