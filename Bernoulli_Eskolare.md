# Bernoulli

Maurício

=================================

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
