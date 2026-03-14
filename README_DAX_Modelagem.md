# 📐 Modelagem e Transformação de Dados com DAX — Power BI

> Desafio prático — Funções DAX, Tabelas Calculadas e Medidas | Bootcamp DIO

---

## 📌 Sobre o Projeto

Projeto desenvolvido como parte do bootcamp **Power BI Analyst** da [DIO](https://www.dio.me/). O foco deste desafio foi a criação de um **esquema em Star Schema** a partir da tabela `Financial Sample`, utilizando **DAX (Data Analysis Expressions)** para criar tabelas calculadas, colunas derivadas e medidas, aprofundando o entendimento sobre modelagem dimensional no Power BI.

---

## 🎯 Objetivos do Desafio

- Criar tabelas dimensão e fato utilizando **DAX**
- Utilizar funções DAX para transformação e derivação de dados
- Construir medidas (measures) para KPIs de negócio
- Organizar o modelo Star Schema com relacionamentos corretos
- Compreender o uso de `RELATED`, `CALCULATE`, `FILTER` e funções de tempo

---

## 🗂️ Estrutura do Repositório

```
📁 dax-modelagem-power-bi/
├── 📄 README.md
├── 📊 dax_financial_model.pbix       ← Arquivo Power BI
├── 📂 data/
│   └── financial_sample.xlsx         ← Fonte de dados original
├── 📂 dax/
│   ├── tabelas_calculadas.dax        ← Scripts das tabelas DAX
│   └── medidas.dax                   ← Scripts das medidas
└── 📂 screenshots/
    ├── modelo_star_schema.png
    ├── tabelas_dax.png
    └── medidas_dax.png
```

---

## ⭐ Modelo Star Schema

```
                   ┌──────────────────┐
                   │  D_Calendario    │
                   │──────────────────│
                   │ DateKey (PK)     │
                   │ Date             │
                   │ Ano              │
                   │ Trimestre        │
                   │ Mês / NomeMes    │
                   │ Dia / DiaSemana  │
                   └────────┬─────────┘
                            │
┌───────────────┐  ┌────────▼─────────┐  ┌────────────────┐
│  D_Produto    │  │   F_Vendas       │  │  D_Descontos   │
│───────────────│  │──────────────────│  │────────────────│
│ ProductKey(PK)│◄─│ ProductKey (FK)  │  │ DiscountKey(PK)│
│ Produto       │  │ DiscountKey (FK) │─►│ FaixaDesconto  │
│ FaixaPreco    │  │ DateKey (FK)     │  │ BandaDesconto  │
└───────────────┘  │ SegmentKey (FK)  │  └────────────────┘
                   │ CountryKey (FK)  │
┌───────────────┐  │──────────────────│  ┌────────────────┐
│  D_Segmento   │◄─│ Unidades_Vend.   │  │   D_Pais       │
│───────────────│  │ Vendas_Brutas    │  │────────────────│
│ SegmentKey(PK)│  │ Descontos        │  │ CountryKey(PK) │
│ Segmento      │  │ Vendas_Liquidas  │  │ País           │
└───────────────┘  │ COGS             │  └────────────────┘
                   │ Lucro            │
                   └──────────────────┘
```

---

## 🧮 Tabelas Criadas com DAX

### F_Vendas — Tabela Fato

```dax
F_Vendas =
SELECTCOLUMNS(
    financials,
    "ProductKey",     RELATED(D_Produto[ProductKey]),
    "SegmentKey",     RELATED(D_Segmento[SegmentKey]),
    "CountryKey",     RELATED(D_Pais[CountryKey]),
    "DiscountKey",    RELATED(D_Descontos[DiscountKey]),
    "DateKey",        FORMAT(financials[Date], "YYYYMMDD"),
    "Unidades_Vendidas",  financials[Units Sold],
    "Vendas_Brutas",      financials[Gross Sales],
    "Descontos",          financials[Discounts],
    "Vendas_Liquidas",    financials[Sales],
    "COGS",               financials[COGS],
    "Lucro",              financials[Profit]
)
```

---

### D_Produto — Dimensão Produto

```dax
D_Produto =
ADDCOLUMNS(
    SUMMARIZE(
        financials,
        financials[Product],
        financials[Discount Band]
    ),
    "ProductKey", RANKX(
        ALL(financials[Product]),
        financials[Product],
        ,
        ASC,
        Dense
    ),
    "FaixaPreco",
    SWITCH(
        TRUE(),
        AVERAGE(financials[Sale Price]) >= 300, "Premium",
        AVERAGE(financials[Sale Price]) >= 150, "Intermediário",
        "Econômico"
    )
)
```

---

### D_Descontos — Dimensão Descontos

```dax
D_Descontos =
ADDCOLUMNS(
    SUMMARIZE(
        financials,
        financials[Discount Band]
    ),
    "DiscountKey", RANKX(
        ALL(financials[Discount Band]),
        financials[Discount Band],
        ,
        ASC,
        Dense
    ),
    "BandaDesconto",
    SWITCH(
        financials[Discount Band],
        "High",   "Alto (>30%)",
        "Medium", "Médio (15–30%)",
        "Low",    "Baixo (<15%)",
        "None",   "Sem Desconto",
        "Não Informado"
    )
)
```

---

### D_Segmento — Dimensão Segmento

```dax
D_Segmento =
ADDCOLUMNS(
    SUMMARIZE(financials, financials[Segment]),
    "SegmentKey", RANKX(
        ALL(financials[Segment]),
        financials[Segment],
        ,
        ASC,
        Dense
    )
)
```

---

### D_Pais — Dimensão País

```dax
D_Pais =
ADDCOLUMNS(
    SUMMARIZE(financials, financials[Country]),
    "CountryKey", RANKX(
        ALL(financials[Country]),
        financials[Country],
        ,
        ASC,
        Dense
    )
)
```

---

### D_Calendario — Dimensão Tempo

```dax
D_Calendario =
VAR _Start = MIN(financials[Date])
VAR _End   = MAX(financials[Date])
RETURN
ADDCOLUMNS(
    CALENDAR(_Start, _End),
    "DateKey",        FORMAT([Date], "YYYYMMDD") + 0,
    "Ano",            YEAR([Date]),
    "Trimestre",      QUARTER([Date]),
    "NomeTrimestre",  "T" & QUARTER([Date]) & "/" & YEAR([Date]),
    "Mês",            MONTH([Date]),
    "NomeMes",        FORMAT([Date], "MMMM"),
    "MesAbrev",       FORMAT([Date], "MMM"),
    "Dia",            DAY([Date]),
    "NomeDiaSemana",  FORMAT([Date], "DDDD"),
    "NumDiaSemana",   WEEKDAY([Date], 2),
    "É_FimSemana",    IF(WEEKDAY([Date], 2) >= 6, "Sim", "Não"),
    "Semestre",       IF(MONTH([Date]) <= 6, "S1", "S2"),
    "AnoMes",         YEAR([Date]) * 100 + MONTH([Date])
)
```

---

## 📏 Medidas DAX (Measures)

### Medidas Base

```dax
-- Total de Vendas
Total Vendas =
SUM(F_Vendas[Vendas_Liquidas])

-- Total de Lucro
Total Lucro =
SUM(F_Vendas[Lucro])

-- Total de Unidades Vendidas
Total Unidades =
SUM(F_Vendas[Unidades_Vendidas])

-- Total de COGS
Total COGS =
SUM(F_Vendas[COGS])

-- Total de Descontos
Total Descontos =
SUM(F_Vendas[Descontos])
```

---

### Medidas Calculadas

```dax
-- Margem de Lucro (%)
Margem Lucro % =
DIVIDE(
    [Total Lucro],
    [Total Vendas],
    0
)

-- Ticket Médio por Transação
Ticket Médio =
DIVIDE(
    [Total Vendas],
    COUNTROWS(F_Vendas),
    0
)

-- Taxa de Desconto (%)
Taxa Desconto % =
DIVIDE(
    [Total Descontos],
    SUM(F_Vendas[Vendas_Brutas]),
    0
)

-- Lucro por Unidade
Lucro por Unidade =
DIVIDE(
    [Total Lucro],
    [Total Unidades],
    0
)
```

---

### Medidas de Inteligência de Tempo

```dax
-- Vendas Ano Anterior (YoY)
Vendas Ano Anterior =
CALCULATE(
    [Total Vendas],
    SAMEPERIODLASTYEAR(D_Calendario[Date])
)

-- Variação YoY (%)
Variação YoY % =
VAR _Atual    = [Total Vendas]
VAR _Anterior = [Vendas Ano Anterior]
RETURN
DIVIDE(_Atual - _Anterior, _Anterior, 0)

-- Vendas Acumuladas no Ano (YTD)
Vendas YTD =
TOTALYTD(
    [Total Vendas],
    D_Calendario[Date]
)

-- Lucro Acumulado no Ano (YTD)
Lucro YTD =
TOTALYTD(
    [Total Lucro],
    D_Calendario[Date]
)

-- Média Móvel 3 Meses
Media Movel 3M =
AVERAGEX(
    DATESINPERIOD(
        D_Calendario[Date],
        LASTDATE(D_Calendario[Date]),
        -3,
        MONTH
    ),
    [Total Vendas]
)
```

---

### Medidas com CALCULATE e FILTER

```dax
-- Vendas apenas do Segmento Government
Vendas Government =
CALCULATE(
    [Total Vendas],
    D_Segmento[Segmento] = "Government"
)

-- Lucro em Países com Vendas > 1 Milhão
Lucro Paises Top =
CALCULATE(
    [Total Lucro],
    FILTER(
        D_Pais,
        CALCULATE([Total Vendas]) > 1000000
    )
)

-- % das Vendas sobre o Total Geral
% do Total =
DIVIDE(
    [Total Vendas],
    CALCULATE([Total Vendas], ALL(F_Vendas)),
    0
)
```

---

## 🔧 Funções DAX Utilizadas

| Função | Categoria | Descrição |
|--------|-----------|-----------|
| `SUMMARIZE` | Tabela | Agrupa e resume dados |
| `ADDCOLUMNS` | Tabela | Adiciona colunas calculadas à tabela |
| `SELECTCOLUMNS` | Tabela | Seleciona colunas específicas |
| `RANKX` | Estatística | Gera ranking/chave sequencial |
| `CALENDAR` | Data/Hora | Cria tabela de datas |
| `CALCULATE` | Filtro | Modifica o contexto de filtro |
| `FILTER` | Filtro | Retorna tabela filtrada |
| `ALL` | Filtro | Remove filtros do contexto |
| `RELATED` | Relacionamento | Traz valor de tabela relacionada |
| `DIVIDE` | Matemática | Divisão segura (evita erro div/0) |
| `SWITCH` | Lógica | Condicional múltipla |
| `TOTALYTD` | Time Intelligence | Acumulado no ano |
| `SAMEPERIODLASTYEAR` | Time Intelligence | Mesmo período do ano anterior |
| `DATESINPERIOD` | Time Intelligence | Intervalo dinâmico de datas |
| `AVERAGEX` | Iteração | Média iterando sobre tabela |
| `FORMAT` | Texto | Formata valores como texto |

---

## 🔄 Processo de Desenvolvimento

```
1. Importar Financial Sample (tabela flat)
         ↓
2. Criar dimensões via DAX (SUMMARIZE + ADDCOLUMNS)
         ↓
3. Criar Fato via DAX (SELECTCOLUMNS + RELATED)
         ↓
4. Criar D_Calendario via DAX (CALENDAR + ADDCOLUMNS)
         ↓
5. Configurar relacionamentos no Modelo
         ↓
6. Criar medidas base (SUM, COUNT, DIVIDE)
         ↓
7. Criar medidas calculadas e de tempo
         ↓
8. Ocultar tabela original e colunas auxiliares
         ↓
9. Validar resultados nos visuais
         ↓
10. Publicar no Power BI Service
```

---

## ✅ Checklist do Desafio

- [x] Tabela original importada e analisada
- [x] `D_Produto` criada com DAX e Surrogate Key
- [x] `D_Descontos` criada com rótulos descritivos
- [x] `D_Segmento` criada com DAX
- [x] `D_Pais` criada com DAX
- [x] `D_Calendario` criada com atributos completos de tempo
- [x] `F_Vendas` criada como tabela fato com FKs e métricas
- [x] Relacionamentos N:1 configurados corretamente
- [x] Medidas base criadas (Vendas, Lucro, Unidades)
- [x] Medidas calculadas criadas (Margem %, Ticket Médio)
- [x] Medidas de inteligência de tempo criadas (YTD, YoY)
- [x] Tabela original ocultada do painel de campos
- [x] Modelo publicado no Power BI Service

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- DAX (Data Analysis Expressions)
- Power Query (importação da fonte)
- Power BI Service (publicação)

---

## 🚀 Como Reproduzir

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/dax-modelagem-power-bi.git

# 2. Abra o arquivo no Power BI Desktop
dax_financial_model.pbix

# 3. Se necessário, atualize o caminho da fonte de dados
# Transformar Dados → Configurações da Fonte de Dados
```

---

## 🔗 Links

- 🔗 [Relatório publicado no Power BI Service](#) *(substitua pelo seu link)*
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
- [Referência DAX — Microsoft](https://learn.microsoft.com/pt-br/dax/)
- [CALCULATE — DAX Guide](https://dax.guide/calculate/)
- [Time Intelligence — Microsoft](https://learn.microsoft.com/pt-br/power-bi/guidance/dax-time-intelligence-functions)
- [Star Schema com DAX](https://learn.microsoft.com/pt-br/power-bi/guidance/star-schema)
