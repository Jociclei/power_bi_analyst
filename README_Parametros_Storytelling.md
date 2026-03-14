# 🎯 Relatório com Parâmetros e Storytelling — Power BI

> Desafio prático — Parâmetros de Campo, Narrativa Visual e Apresentação ao Cliente | Bootcamp DIO

---

## 📌 Sobre o Projeto

Evolução do relatório criativo desenvolvido no módulo anterior, agora com a adição de **parâmetros de campo dinâmicos** e uma página de **storytelling** estruturada como uma apresentação executiva ao cliente. O foco foi transformar dados em narrativa — conduzindo o observador por uma história com começo, meio e conclusão.

---

## 🎯 Objetivos do Desafio

- Criar parâmetros de campo baseados em **categorias**
- Criar parâmetros de campo baseados em **valores** (Profit, Sales etc.)
- Manter a estilização do relatório anterior
- Construir uma **história visual** (storytelling) para apresentação ao cliente
- Produzir uma página executiva limpa, clara e persuasiva

---

## 🗂️ Estrutura do Repositório

```
📁 power-bi-parametros-storytelling/
├── 📄 README.md
├── 📊 relatorio_parametros.pbix          ← Arquivo Power BI
├── 📂 data/
│   └── financial_sample.xlsx
├── 📂 assets/
│   ├── background_parametros.png
│   └── background_storytelling.png
└── 📂 screenshots/
    ├── parametro_categorias.png
    ├── parametro_valores.png
    └── pagina_storytelling.png
```

---

## ⚙️ Parâmetros de Campo Criados

### 📂 Parâmetro 1 — Por Categoria

**Nome do parâmetro:** `Dimensão Análise`

Permite ao usuário escolher **qual dimensão categórica** deseja visualizar no eixo X dos gráficos, tornando o relatório explorável de forma livre.

| Opção disponível | Campo original |
|-----------------|----------------|
| Produto | `financials[Product]` |
| Segmento | `financials[Segment]` |
| País | `financials[Country]` |
| Faixa de Desconto | `financials[Discount Band]` |
| Mês | `D_Calendario[NomeMes]` |
| Trimestre | `D_Calendario[NomeTrimestre]` |

**Como foi criado:**
```
Power BI Desktop
  → Modelagem
  → Novo Parâmetro
  → Campos
  → Adicionar os campos desejados
  → ✅ Adicionar segmentador à página
```

**Medida gerada automaticamente:**
```dax
Dimensão Análise =
{
    ("Produto",       NAMEOF('financials'[Product]),       0),
    ("Segmento",      NAMEOF('financials'[Segment]),       1),
    ("País",          NAMEOF('financials'[Country]),        2),
    ("Faixa Desconto",NAMEOF('financials'[Discount Band]), 3),
    ("Mês",           NAMEOF('D_Calendario'[NomeMes]),     4),
    ("Trimestre",     NAMEOF('D_Calendario'[NomeTrimestre]),5)
}
```

---

### 📊 Parâmetro 2 — Por Valor

**Nome do parâmetro:** `Métrica Análise`

Permite ao usuário escolher **qual métrica numérica** deseja analisar nos visuais, sem precisar criar múltiplos gráficos para cada KPI.

| Opção disponível | Medida DAX |
|-----------------|------------|
| Vendas Líquidas | `[Total Vendas]` |
| Lucro | `[Total Lucro]` |
| Unidades Vendidas | `[Total Unidades]` |
| COGS | `[Total COGS]` |
| Margem % | `[Margem Lucro %]` |
| Vendas Brutas | `SUM(financials[Gross Sales])` |

**Medida gerada automaticamente:**
```dax
Métrica Análise =
{
    ("Vendas Líquidas",   NAMEOF('Medidas'[Total Vendas]),    0),
    ("Lucro",             NAMEOF('Medidas'[Total Lucro]),     1),
    ("Unidades Vendidas", NAMEOF('Medidas'[Total Unidades]),  2),
    ("COGS",              NAMEOF('Medidas'[Total COGS]),      3),
    ("Margem %",          NAMEOF('Medidas'[Margem Lucro %]),  4),
    ("Vendas Brutas",     NAMEOF('Medidas'[Vendas Brutas]),   5)
}
```

---

## 📄 Visuais com Parâmetros

### Visual 1 — Gráfico de Barras Dinâmico (Categoria × Valor)

```
Eixo X:  → [Dimensão Análise] (parâmetro de categoria)
Eixo Y:  → [Métrica Análise]  (parâmetro de valor)
Legenda: → Segmento (fixo para contexto)
```

O usuário escolhe **o que** quer ver no eixo e **qual métrica** analisar — um único gráfico substitui dezenas de combinações.

---

### Visual 2 — Gráfico de Linhas Dinâmico (Evolução Temporal)

```
Eixo X:  → D_Calendario[NomeMes] (fixo — eixo temporal)
Eixo Y:  → [Métrica Análise] (parâmetro de valor)
Detalhe: → [Dimensão Análise] (parâmetro de categoria)
```

Permite visualizar a evolução de qualquer métrica, detalhada por qualquer categoria.

---

## 📖 Página de Storytelling — Apresentação ao Cliente

### 🎬 A Narrativa: *"De onde viemos, onde estamos e para onde vamos"*

A página foi estruturada em **três atos**, como uma apresentação executiva:

---

### Ato 1 — O Cenário Atual 📍
*"Aqui está onde estamos hoje"*

> Bloco visual com os 4 KPIs principais dispostos em destaque:
> **Vendas Totais · Lucro · Margem % · Crescimento YoY**
>
> Acompanhado de uma frase de abertura contextualizada:
> *"Em [ano], a empresa registrou [X] em vendas, com margem de [Y]% — [crescimento/queda] de [Z]% em relação ao ano anterior."*

---

### Ato 2 — O que Está Funcionando 🏆
*"Nossos motores de crescimento"*

> Gráfico de barras horizontais ranqueando os **Top 5 Produtos por Lucro**,
> ao lado de um gráfico de pizza mostrando a **participação dos segmentos**.
>
> Caixa de texto narrativa:
> *"Paseo lidera em volume de lucro, enquanto o segmento Government representa a maior fatia de receita — indicando onde concentrar esforços de expansão."*

---

### Ato 3 — A Oportunidade à Frente 🚀
*"Para onde devemos olhar"*

> Mapa geográfico destacando países com **alto volume e baixa margem**
> (oportunidade de otimização) vs países com **alta margem e baixo volume**
> (oportunidade de escala).
>
> Caixa de texto conclusiva:
> *"Mercados como [País X] apresentam margem superior a [Y]%, mas participação abaixo de [Z]% no total. Há espaço para crescimento sem pressionar custos."*

---

### Layout da Página de Storytelling

```
┌─────────────────────────────────────────────────────────┐
│  LOGO        TÍTULO DO RELATÓRIO          DATA / PERÍODO │
├──────────┬──────────┬──────────┬──────────────────────── │
│  VENDAS  │  LUCRO   │ MARGEM % │  CRESCIMENTO YoY        │
│  TOTAIS  │  TOTAL   │          │                          │
├──────────┴──────────┴──────────┴────────────────────────┤
│                                                           │
│  "O Cenário Atual"   │   "O que está funcionando"        │
│  [Texto narrativo]   │   [Gráfico Top 5 Produtos]        │
│                      │   [Pizza por Segmento]             │
├──────────────────────┴────────────────────────────────── │
│                                                           │
│  "A Oportunidade"    │   [Mapa Oportunidade vs Escala]   │
│  [Texto narrativo]   │                                   │
│                      │   ● Alta margem, baixo volume     │
│                      │   ● Alto volume, baixa margem     │
├──────────────────────┴────────────────────────────────── │
│  [← Voltar]                          Página 5 de 5       │
└─────────────────────────────────────────────────────────┘
```

---

## 🎨 Estilização — Consistência com o Relatório Anterior

| Elemento | Especificação |
|----------|---------------|
| Fundo | `#0D1B2A` (azul petróleo escuro) |
| Cards KPI | `#162032` com borda `#1B9AAA` |
| Cor positiva | `#06D6A0` (verde menta) |
| Cor negativa | `#EF476F` (coral) |
| Fonte títulos | Segoe UI Semibold 18pt branco |
| Fonte corpo | Segoe UI Light 11pt `#ADB5BD` |
| Segmentadores | Estilo dropdown, fundo `#162032` |
| Botões parâmetro | Estilo lista, seleção em `#1B9AAA` |

---

## 💡 Por que Parâmetros de Campo?

| Sem Parâmetros | Com Parâmetros |
|----------------|----------------|
| 1 gráfico por combinação | 1 gráfico para todas as combinações |
| Relatório estático | Relatório exploratório |
| Usuário dependente do analista | Usuário autônomo na análise |
| Muitas páginas e visuais | Interface limpa e enxuta |

> **Resultado:** um relatório mais inteligente, flexível e profissional — que respeita o tempo do cliente e amplia o poder de análise.

---

## ✅ Checklist do Desafio

- [x] Parâmetro de categoria criado (`Dimensão Análise`)
- [x] Parâmetro de valor criado (`Métrica Análise`)
- [x] Visual 1: gráfico dinâmico categoria × valor
- [x] Visual 2: gráfico de linhas dinâmico
- [x] Segmentadores dos parâmetros estilizados
- [x] Estilização consistente com o relatório anterior
- [x] Página de storytelling criada
- [x] Narrativa em 3 atos estruturada
- [x] Textos narrativos inseridos nos visuais
- [x] Layout executivo limpo e profissional
- [x] Relatório publicado no Power BI Service

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- DAX — Parâmetros de Campo
- Power BI Service (publicação)

---

## 🚀 Como Reproduzir

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/power-bi-parametros-storytelling.git

# 2. Abra no Power BI Desktop
relatorio_parametros.pbix

# 3. Para recriar os parâmetros do zero:
# Modelagem → Novo Parâmetro → Campos
# Adicione os campos/medidas desejados
# Marque "Adicionar segmentador à página"
```

---

## 🔗 Links

- 🔗 [Relatório no Power BI Service](#) *(substitua pelo seu link)*
- 🔗 [Repositório base do curso](https://github.com/julianazanelatto/power_bi_analyst)
- 🔗 [Relatório anterior (base deste projeto)](#)

---

## 👤 Autor

Desenvolvido como parte do portfólio de projetos de dados.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](#)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](#)
[![DIO](https://img.shields.io/badge/DIO-E94D5F?style=for-the-badge&logo=dio&logoColor=white)](https://www.dio.me/)

---

## 📚 Referências

- [Repositório base — julianazanelatto](https://github.com/julianazanelatto/power_bi_analyst)
- [Parâmetros de campo — Microsoft Docs](https://learn.microsoft.com/pt-br/power-bi/create-reports/power-bi-field-parameters)
- [Storytelling com dados](https://learn.microsoft.com/pt-br/power-bi/guidance/report-design-tips)
- [Referência DAX](https://learn.microsoft.com/pt-br/dax/)
