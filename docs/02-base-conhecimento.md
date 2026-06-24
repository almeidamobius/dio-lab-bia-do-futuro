# Base de Conhecimento

## Dados Utilizados

Para este projeto, estamos utilizando dados financeiros reais do **Yahoo Finance Dataset**, disponível no Hugging Face. O foco inicial é a análise da empresa **Bradesco (BBDC4)**.

| Dataset | Formato | Descrição | Utilização no Agente |
|---------|---------|-----------|---------------------|
| `stock_prices` | Parquet | Preços históricos (open, high, low, close, volume) | Análise de tendências, gráficos de preços, volatilidade |
| `stock_profile` | Parquet | Perfil da empresa (endereço, setor, funcionários) | Contexto da empresa, resumo executivo |
| `stock_statement` | Parquet | Demonstrações financeiras (DRE, balanço, fluxo de caixa) | Análise financeira, indicadores, valuation |
| `stock_officers` | Parquet | Executivos (nome, cargo, remuneração) | Governança, perfil de liderança |
| `stock_news` | Parquet | Notícias e artigos sobre a empresa | Sentimento de mercado, eventos relevantes |
| `stock_dividend_events` | Parquet | Histórico de pagamento de dividendos | Análise de dividend yield, consistência |

### Fonte dos Dados

Todos os dados são provenientes do dataset público:
- **Repositório:** [defeatbeta/yahoo-finance-data](https://huggingface.co/datasets/defeatbeta/yahoo-finance-data)
- **Fonte Original:** Yahoo! Finance, Nasdaq!, U.S. Department of the Treasury
- **Formato:** Parquet
- **Atualização:** Regular, com registro em `spec.json`

### Estrutura Local dos Dados


---

## Adaptações nos Dados

### Mudanças em relação ao projeto original

| Aspecto | Original | Adaptação |
|---------|----------|-----------|
| **Fonte de dados** | Dados mockados (fictícios) | Dados reais do Yahoo Finance |
| **Formato** | CSV e JSON | Parquet (formato otimizado) |
| **Escopo** | Dados do cliente (gastos, perfil) | Dados de mercado (ações, finanças) |
| **Atualização** | Estático | Dinâmico (atualizado via script) |

### Por que essas mudanças?

1. **Dados Reais:** O projeto ganha complexidade e relevância ao usar dados reais de mercado
2. **Formato Parquet:** Mais eficiente para grandes volumes de dados
3. **Escalabilidade:** Possibilidade de adicionar mais empresas e indicadores
4. **Aprendizado:** Uso de ferramentas reais (DuckDB, Hugging Face)

---

## Estratégia de Integração

### Como os dados são carregados?

Os dados são carregados usando **DuckDB** para consultas SQL diretamente nos arquivos Parquet:

```python
# Exemplo de carregamento
import duckdb

conn = duckdb.connect()
query = """
    SELECT * FROM 'data/raw/yahoo_finance/stock_prices_BBDC4.parquet'
    ORDER BY report_date DESC
    LIMIT 10
"""
df = conn.execute(query).df()
