# ⭐ Star Schema para Cenários de Vendas — Power BI

> Desafio prático — Modelagem Dimensional com Star Schema | Bootcamp DIO

---

## 📌 Sobre o Projeto

Projeto desenvolvido como parte do bootcamp **Power BI Analyst** da [DIO](https://www.dio.me/). O objetivo foi criar um modelo dimensional no formato **Star Schema (Esquema Estrela)** a partir de uma tabela única (`Financial Sample`), decompondo-a em uma **tabela fato** e múltiplas **tabelas dimensão**, seguindo as boas práticas de modelagem para BI.

---

## 🎯 Objetivos do Desafio

- Compreender a diferença entre modelo relacional e modelo dimensional
- Criar um **Star Schema** a partir de uma tabela desnormalizada
- Definir a **tabela fato** com métricas de negócio
- Criar **tabelas dimensão** com dados descritivos
- Gerar uma **dimensão de calendário** via DAX
- Organizar o modelo para otimizar consultas no Power BI

---

## 🗂️ Estrutura do Repositório

```
📁 star-schema-vendas/
├── 📄 README.md
├── 📊 star_schema_vendas.pbix        ← Arquivo Power BI
├── 📂 data/
│   └── financial_sample.xlsx         ← Tabela original (fonte)
└── 📂 screenshots/
    ├── diagrama_star_schema.png
    ├── tabela_fato.png
    ├── dimensao_produto.png
    ├── dimensao_calendario.png
    └── modelo_completo.png
```

---

## ⭐ Diagrama — Star Schema

```
                    ┌─────────────────┐
                    │  Dim_Calendario  │
                    │─────────────────│
                    │ SK_Data (PK)    │
                    │ Data            │
                    │ Ano             │
                    │ Trimestre       │
                    │ Mês             │
                    │ Nome do Mês     │
                    │ Dia             │
                    │ Dia da Semana   │
                    └────────┬────────┘
                             │
┌──────────────┐    ┌────────▼────────┐    ┌─────────────────┐
│  Dim_Produto │    │   Fato_Vendas   │    │   Dim_Empresa   │
│──────────────│    │─────────────────│    │─────────────────│
│ SK_Produto   │◄───│ SK_Produto (FK) │    │ SK_Empresa (PK) │
│ Produto      │    │ SK_Segmento(FK) │───►│ Empresa         │
│ Faixa Descto │    │ SK_Data (FK)    │    │ País            │
└──────────────┘    │ SK_Empresa (FK) │    └─────────────────┘
                    │─────────────────│
┌──────────────┐    │  Unidades Vend. │
│ Dim_Segmento │◄───│  Valor Vendas   │
│──────────────│    │  COGS           │
│ SK_Segmento  │    │  Lucro          │
│ Segmento     │    │  Desconto       │
└──────────────┘    │  Valor Bruto    │
                    └─────────────────┘
```

---

## 📐 Detalhamento das Tabelas

### ⚡ Fato_Vendas (Tabela Fato)

Contém as **métricas quantitativas** do negócio — os fatos mensuráveis.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `SK_Produto` | INT | Chave estrangeira → Dim_Produto |
| `SK_Segmento` | INT | Chave estrangeira → Dim_Segmento |
| `SK_Data` | INT | Chave estrangeira → Dim_Calendario |
| `SK_Empresa` | INT | Chave estrangeira → Dim_Empresa |
| `Unidades_Vendidas` | DECIMAL | Quantidade de unidades vendidas |
| `Valor_Vendas` | DECIMAL | Receita bruta de vendas |
| `COGS` | DECIMAL | Custo dos produtos vendidos |
| `Lucro` | DECIMAL | Lucro líquido (Sales - COGS) |
| `Desconto` | DECIMAL | Valor do desconto concedido |
| `Vendas_Brutas` | DECIMAL | Valor antes dos descontos |

> A tabela fato **não contém descrições** — apenas chaves e métricas.

---

### 📦 Dim_Produto (Dimensão Produto)

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `SK_Produto` | INT | Surrogate Key (PK) |
| `Produto` | TEXT | Nome do produto |
| `Faixa_Desconto` | TEXT | Categoria de desconto aplicado |

---

### 🏢 Dim_Segmento (Dimensão Segmento)

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `SK_Segmento` | INT | Surrogate Key (PK) |
| `Segmento` | TEXT | Segmento de mercado |

---

### 🌍 Dim_Empresa (Dimensão Empresa/Localização)

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `SK_Empresa` | INT | Surrogate Key (PK) |
| `Empresa` | TEXT | Nome da empresa / unidade |
| `País` | TEXT | País de operação |

---

### 📅 Dim_Calendario (Dimensão Tempo)

Criada via **DAX** com a função `CALENDAR()` ou `CALENDARAUTO()`.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `SK_Data` | INT | Surrogate Key no formato YYYYMMDD |
| `Data` | DATE | Data completa |
| `Ano` | INT | Ano (ex: 2024) |
| `Trimestre` | INT | Número do trimestre (1–4) |
| `Nome_Trimestre` | TEXT | Ex: "T1 2024" |
| `Mês` | INT | Número do mês (1–12) |
| `Nome_Mes` | TEXT | Ex: "Janeiro" |
| `Dia` | INT | Dia do mês |
| `Dia_Semana` | INT | Número do dia da semana |
| `Nome_DiaSemana` | TEXT | Ex: "Segunda-feira" |
| `É_FimDeSemana` | BOOL | TRUE se sábado ou domingo |

#### 💻 Código DAX — Dim_Calendario

```dax
Dim_Calendario =
VAR _MinDate = MIN(Fato_Vendas[Data])
VAR _MaxDate = MAX(Fato_Vendas[Data])
RETURN
ADDCOLUMNS(
    CALENDAR(_MinDate, _MaxDate),
    "Ano",            YEAR([Date]),
    "Trimestre",      QUARTER([Date]),
    "Nome_Trimestre", "T" & QUARTER([Date]) & " " & YEAR([Date]),
    "Mês",            MONTH([Date]),
    "Nome_Mes",       FORMAT([Date], "MMMM"),
    "Dia",            DAY([Date]),
    "Dia_Semana",     WEEKDAY([Date], 2),
    "Nome_DiaSemana", FORMAT([Date], "DDDD"),
    "É_FimDeSemana",  IF(WEEKDAY([Date], 2) >= 6, TRUE, FALSE),
    "SK_Data",        YEAR([Date]) * 10000
                    + MONTH([Date]) * 100
                    + DAY([Date])
)
```

---

## 🔄 Processo de Criação — Passo a Passo

### 1. Análise da tabela original
```
Financial Sample contém tudo em uma única tabela "flat":
produto, segmento, país, data, valores — tudo misturado.
```

### 2. Identificação das dimensões
```
Perguntas feitas:
  "Por qual produto?" → Dim_Produto
  "Por qual segmento?" → Dim_Segmento
  "Em qual país/empresa?" → Dim_Empresa
  "Em qual data/período?" → Dim_Calendario
```

### 3. Criação via Power Query
- Duplicar a tabela original para cada dimensão
- Selecionar apenas as colunas relevantes
- Remover duplicatas (`Remover Duplicatas`)
- Adicionar coluna de índice como **Surrogate Key**
- Renomear colunas para padrão claro

### 4. Criação da Fato_Vendas
- Partir da tabela original
- Fazer Merge com cada dimensão para trazer a SK correspondente
- Remover todas as colunas descritivas (ficam apenas FKs e métricas)

### 5. Dim_Calendario via DAX
- Criar tabela calculada com o código DAX acima
- Marcar como **Tabela de Datas** no Power BI

### 6. Configurar relacionamentos
```
Power BI Desktop → Exibição de Modelo
  Fato_Vendas[SK_Produto]   → Dim_Produto[SK_Produto]   (N:1)
  Fato_Vendas[SK_Segmento]  → Dim_Segmento[SK_Segmento] (N:1)
  Fato_Vendas[SK_Empresa]   → Dim_Empresa[SK_Empresa]   (N:1)
  Fato_Vendas[SK_Data]      → Dim_Calendario[SK_Data]   (N:1)
```

---

## 💡 Conceitos Aplicados

### Star Schema vs Snowflake
| Critério | Star Schema ✅ | Snowflake |
|----------|---------------|-----------|
| Dimensões normalizadas | Não | Sim |
| Performance de query | Maior | Menor |
| Complexidade | Baixa | Alta |
| Uso em BI/Power BI | Recomendado | Evitar |

### Surrogate Key vs Natural Key
| | Surrogate Key ✅ | Natural Key |
|-|-----------------|-------------|
| Definição | Chave artificial (INT sequencial) | Chave do negócio (CPF, código) |
| Vantagem | Independente de mudanças no negócio | Familiar para o usuário |
| Uso no modelo | PK das dimensões | Manter como atributo descritivo |

---

## ✅ Checklist do Desafio

- [x] Tabela original (Financial Sample) importada
- [x] `Dim_Produto` criada com Surrogate Key
- [x] `Dim_Segmento` criada com Surrogate Key
- [x] `Dim_Empresa` criada com país e empresa
- [x] `Dim_Calendario` criada via DAX com atributos de tempo
- [x] `Fato_Vendas` criada com métricas e chaves estrangeiras
- [x] Relacionamentos N:1 configurados no modelo
- [x] Tabela original ocultada do relatório
- [x] Dim_Calendario marcada como tabela de datas
- [x] Modelo validado e publicado no Power BI Service

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- Power Query (transformações M)
- DAX (criação da Dim_Calendario)
- Power BI Service (publicação)

---

## 🚀 Como Reproduzir

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/star-schema-vendas.git

# 2. Abra o arquivo no Power BI Desktop
star_schema_vendas.pbix

# 3. Atualize o caminho do arquivo Excel se necessário
# Power Query → Configurações da fonte de dados
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
- [Star Schema — Microsoft](https://learn.microsoft.com/pt-br/power-bi/guidance/star-schema)
- [Função CALENDAR — DAX](https://learn.microsoft.com/pt-br/dax/calendar-function-dax)
- [Melhores práticas de modelagem](https://learn.microsoft.com/pt-br/power-bi/guidance/model-date-tables)
