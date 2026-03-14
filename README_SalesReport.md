# 📈 Sales Report — Relatório de Vendas | Power BI

> Desafio prático — Criação de Relatório de Vendas Completo | Bootcamp DIO

---

## 📌 Sobre o Projeto

Projeto desenvolvido como parte do bootcamp **Power BI Analyst** da [DIO](https://www.dio.me/). O objetivo foi construir um **Sales Report** (Relatório de Vendas) completo e interativo, com foco em análise de performance comercial, acompanhamento de metas e distribuição geográfica de vendas.

---

## 🎯 Objetivos do Desafio

- Criar um relatório de vendas visualmente elaborado e profissional
- Desenvolver KPIs relevantes para análise comercial
- Implementar navegação intuitiva entre páginas
- Utilizar visuais adequados para cada tipo de análise
- Publicar o relatório no Power BI Service

---

## 🗂️ Estrutura do Repositório

```
📁 sales-report-power-bi/
├── 📄 README.md
├── 📊 sales_report.pbix              ← Arquivo Power BI
├── 📂 data/
│   └── financial_sample.xlsx         ← Base de dados utilizada
└── 📂 screenshots/
    ├── pagina_1_visao_geral.png
    ├── pagina_2_por_produto.png
    └── pagina_3_mapa_vendas.png
```

---

## 📄 Páginas do Relatório

### Página 1 — Visão Geral de Vendas

Painel executivo com os principais indicadores do negócio.

| Visual | Campos | Finalidade |
|--------|--------|------------|
| 🃏 Cartão KPI | `Total Vendas` | Receita total do período |
| 🃏 Cartão KPI | `Total Lucro` | Lucro líquido |
| 🃏 Cartão KPI | `Total Unidades` | Unidades vendidas |
| 🃏 Cartão KPI | `Margem Lucro %` | Percentual de margem |
| 📊 Barras clusterizadas | Vendas × Lucro por Produto | Comparativo de performance |
| 📈 Linha | Evolução mensal de Vendas | Tendência ao longo do tempo |
| 🥧 Pizza | Vendas por Segmento | Participação de cada segmento |
| 🔘 Segmentador | Ano / País | Filtros globais da página |

---

### Página 2 — Análise por Produto e Segmento

Detalhamento da performance comercial por categoria.

| Visual | Campos | Finalidade |
|--------|--------|------------|
| 📊 Barras horizontais | Lucro por Produto | Ranking de produtos mais lucrativos |
| 📉 Dispersão (Scatter) | Vendas × Lucro × Unidades | Correlação entre métricas |
| 📋 Matriz | Produto × Segmento × Vendas | Cruzamento detalhado |
| 📊 Colunas empilhadas | Vendas por Segmento/Mês | Evolução por segmento |
| 🔘 Segmentador | Segmento / Faixa de Desconto | Filtros contextuais |
| 🔀 Botões + Indicadores | Alternar entre visão Vendas e Lucro | Experiência interativa |

---

### Página 3 — Distribuição Geográfica

Análise espacial das vendas por país.

| Visual | Campos | Finalidade |
|--------|--------|------------|
| 🗺️ Mapa preenchido | País × Total Vendas | Intensidade de vendas por região |
| 🗺️ Mapa de bolhas | País × Lucro | Volume de lucro por localização |
| 📊 Barras | Vendas por País | Ranking geográfico |
| 🃏 Cartão | País com maior venda | Destaque dinâmico |
| 🔘 Segmentador | Ano / Segmento | Filtros contextuais |

---

## 📐 Medidas DAX Criadas

### Medidas Base

```dax
Total Vendas =
SUM(financials[Sales])

Total Lucro =
SUM(financials[Profit])

Total Unidades =
SUM(financials[Units Sold])

Total COGS =
SUM(financials[COGS])

Total Descontos =
SUM(financials[Discounts])
```

### Medidas de Performance

```dax
Margem Lucro % =
DIVIDE([Total Lucro], [Total Vendas], 0)

Ticket Médio =
DIVIDE([Total Vendas], COUNTROWS(financials), 0)

Taxa Desconto % =
DIVIDE([Total Descontos], SUM(financials[Gross Sales]), 0)

% Participação Vendas =
DIVIDE(
    [Total Vendas],
    CALCULATE([Total Vendas], ALL(financials)),
    0
)
```

### Medidas de Inteligência de Tempo

```dax
Vendas YTD =
TOTALYTD([Total Vendas], financials[Date])

Lucro YTD =
TOTALYTD([Total Lucro], financials[Date])

Vendas Ano Anterior =
CALCULATE(
    [Total Vendas],
    SAMEPERIODLASTYEAR(financials[Date])
)

Crescimento YoY % =
VAR _Atual    = [Total Vendas]
VAR _Anterior = [Vendas Ano Anterior]
RETURN
DIVIDE(_Atual - _Anterior, _Anterior, 0)
```

---

## ✨ Recursos de UX Implementados

### 🖱️ Botões de Navegação
- Botões estilizados para transitar entre as 3 páginas
- Estados de hover configurados para feedback visual
- Ícones consistentes com o tema do relatório

### 🔀 Indicadores (Bookmarks)
Dois bookmarks configurados na Página 2:
```
[Botão "Vendas"]  → Exibe gráfico de barras de Vendas
[Botão "Lucro"]   → Exibe gráfico de barras de Lucro
```

### 🎨 Design e Layout
- Fundo personalizado com formas geométricas
- Paleta de cores: tons de azul escuro e cinza (#1E3A5F, #2E86AB, #F5F5F5)
- Tipografia: Segoe UI em todos os títulos e rótulos
- Seções visuais claramente delimitadas

### 🔘 Segmentadores Sincronizados
- Filtros de **Ano** e **País** sincronizados entre páginas 1 e 3
- Segmentador de **Segmento** presente nas páginas 2 e 3

---

## 📊 Principais Insights do Relatório

> *(Exemplos baseados no Financial Sample — adapte conforme seus dados)*

- 🏆 **Produto mais lucrativo:** Paseo lidera em volume de lucro
- 🌍 **País com maior receita:** Estados Unidos representa ~30% das vendas totais
- 📅 **Melhor período:** Q4 concentra o maior volume de vendas do ano
- 🎯 **Segmento líder:** Government responde pela maior fatia de vendas brutas
- 📉 **Maior desconto médio:** Segmento Channel Partners registra maior taxa de desconto

---

## ✅ Checklist do Desafio

- [x] Página 1 — Visão Geral com KPIs e gráficos de tendência
- [x] Página 2 — Análise por Produto e Segmento
- [x] Página 3 — Mapa geográfico de vendas
- [x] Medidas DAX criadas (base, performance, tempo)
- [x] Botões de navegação entre páginas
- [x] Indicadores (bookmarks) para alternar visuais
- [x] Segmentadores configurados e sincronizados
- [x] Layout e design profissional aplicados
- [x] Tooltips personalizados nos visuais principais
- [x] Relatório publicado no Power BI Service

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- DAX (Data Analysis Expressions)
- Power Query (M Language)
- Power BI Service

---

## 🚀 Como Visualizar

### Opção 1 — Power BI Desktop
```bash
# Clone o repositório
git clone https://github.com/seu-usuario/sales-report-power-bi.git

# Abra o arquivo
sales_report.pbix
```

### Opção 2 — Power BI Service
🔗 [Acesse o relatório publicado aqui](#) *(substitua pelo seu link)*

---

## 📸 Preview

### Página 1 — Visão Geral
![Visão Geral](screenshots/pagina_1_visao_geral.png)

### Página 2 — Por Produto
![Por Produto](screenshots/pagina_2_por_produto.png)

### Página 3 — Mapa de Vendas
![Mapa](screenshots/pagina_3_mapa_vendas.png)

---

## 🔗 Links

- 🔗 [Relatório no Power BI Service](#) *(substitua pelo seu link)*
- 🔗 [Repositório base do curso](https://github.com/julianazanelatto/power_bi_analyst)

---

## 👤 Autor

Desenvolvido como parte do portfólio de projetos de dados.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](#)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](#)
[![DIO](https://img.shields.io/badge/DIO-E94D5F?style=for-the-badge&logo=dio&logoColor=white)](https://www.dio.me/)

---

## 📚 Referências

- [Repositório base — julianazanelatto](https://github.com/julianazanelatto/power_bi_analyst)
- [Documentação Power BI](https://learn.microsoft.com/pt-br/power-bi/)
- [Referência DAX](https://learn.microsoft.com/pt-br/dax/)
- [Boas práticas de relatórios](https://learn.microsoft.com/pt-br/power-bi/guidance/report-design-tips)
