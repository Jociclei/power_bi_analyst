# 🎨 Relatório Criativo — Data Analytics com Power BI

> Desafio prático — Análise de Dados com Design Criativo e Storytelling | Bootcamp DIO

---

## 📌 Sobre o Projeto

Projeto desenvolvido como parte do bootcamp **Power BI Analyst** da [DIO](https://www.dio.me/). O diferencial deste desafio foi criar um relatório que vai além da funcionalidade — unindo **análise de dados robusta** com **design criativo**, narrativa visual (storytelling) e uma experiência de usuário elaborada, como um produto de BI profissional.

---

## 🎯 Objetivos do Desafio

- Criar um relatório com identidade visual própria e coesa
- Aplicar técnicas de **Data Storytelling** na apresentação dos dados
- Explorar recursos avançados de design no Power BI
- Combinar visuais nativos e customizados para comunicar insights
- Publicar e compartilhar o relatório como produto final

---

## 🗂️ Estrutura do Repositório

```
📁 data-analytics-relatorio-criativo/
├── 📄 README.md
├── 📊 relatorio_criativo.pbix            ← Arquivo Power BI
├── 📂 data/
│   └── financial_sample.xlsx             ← Base de dados
├── 📂 assets/
│   ├── background_pagina1.png            ← Fundo customizado P1
│   ├── background_pagina2.png            ← Fundo customizado P2
│   ├── background_pagina3.png            ← Fundo customizado P3
│   ├── icone_vendas.svg
│   ├── icone_lucro.svg
│   └── icone_produtos.svg
└── 📂 screenshots/
    ├── capa.png
    ├── pagina_1_performance.png
    ├── pagina_2_tendencias.png
    └── pagina_3_detalhes.png
```

---

## 🖼️ Design e Identidade Visual

### Paleta de Cores

| Uso | Cor | Hex |
|-----|-----|-----|
| Primária (fundo escuro) | Azul Petróleo | `#0D1B2A` |
| Secundária (destaques) | Azul Elétrico | `#1B9AAA` |
| Acento positivo | Verde Menta | `#06D6A0` |
| Acento negativo | Coral | `#EF476F` |
| Texto principal | Branco Suave | `#F8F9FA` |
| Texto secundário | Cinza Claro | `#ADB5BD` |
| Superfícies / Cards | Azul Médio | `#162032` |

### Tipografia
- **Títulos:** Segoe UI Semibold — 18–24pt
- **Subtítulos:** Segoe UI — 14–16pt
- **Corpo / Rótulos:** Segoe UI Light — 10–12pt
- **KPIs:** Segoe UI Bold — 28–36pt

### Princípios de Design Aplicados
- **Hierarquia visual:** elementos mais importantes com maior peso visual
- **Espaço negativo:** uso estratégico de espaço em branco para respiração
- **Contraste:** fundo escuro com elementos claros para leitura em ambientes dim
- **Consistência:** mesma linguagem visual em todas as páginas
- **Alinhamento em grid:** todos os elementos alinhados a uma grade de 8px

---

## 📄 Páginas do Relatório

### 🏠 Capa / Menu de Navegação
Página inicial com identidade visual do relatório e botões de navegação para cada seção.

```
┌─────────────────────────────────────────┐
│           SALES ANALYTICS               │
│         Financial Overview              │
│                                         │
│  [📊 Performance]  [📈 Tendências]      │
│  [🌍 Geografia]    [🔎 Detalhes]        │
└─────────────────────────────────────────┘
```

---

### Página 1 — Performance Comercial

**Narrativa:** *"Como estamos performando em relação às nossas metas?"*

| Visual | Tipo | Insight comunicado |
|--------|------|--------------------|
| Gauge / Velocímetro | Meta vs Realizado | % de atingimento da meta de vendas |
| KPI Cards animados | Vendas / Lucro / Unidades / Margem | Visão rápida dos números-chave |
| Barras com benchmark | Produto vs Média geral | Quais produtos estão acima da média |
| Treemap | Segmento × Receita | Proporção visual dos segmentos |
| Sparklines | Tendência 12 meses | Mini-gráfico de tendência por produto |
| Segmentador estilizado | Ano / Trimestre | Controle temporal |

---

### Página 2 — Tendências e Sazonalidade

**Narrativa:** *"Quando vendemos mais e por quê?"*

| Visual | Tipo | Insight comunicado |
|--------|------|--------------------|
| Área empilhada | Vendas mensais por segmento | Sazonalidade e composição |
| Heatmap (Matriz) | Mês × Produto → Lucro | Meses quentes por produto |
| Linhas duplas | Vendas vs Ano anterior (YoY) | Crescimento ou queda |
| Ribbon Chart | Ranking de produtos por mês | Mudanças no ranking ao longo do tempo |
| Cartão dinâmico | Melhor mês / Pior mês | Extremos automáticos via DAX |
| Segmentador | Segmento | Filtro contextual |

---

### Página 3 — Análise Geográfica

**Narrativa:** *"Onde estão nossas oportunidades?"*

| Visual | Tipo | Insight comunicado |
|--------|------|--------------------|
| Mapa preenchido (choropleth) | País → Intensidade de Vendas | Calor de vendas por região |
| Mapa de bolhas | País → Volume de Lucro | Tamanho = magnitude do lucro |
| Barras horizontais ranqueadas | Top 5 países | Ranking visual de mercados |
| KPI por país | Margem % por localização | Rentabilidade geográfica |
| Tooltip personalizado | Hover no mapa → detalhes | Vendas, Lucro, Unidades, Margem |

---

### Página 4 — Detalhamento (Drill-through)

**Narrativa:** *"Qual é a história por trás dos números?"*

| Visual | Tipo | Insight comunicado |
|--------|------|--------------------|
| Tabela detalhada | Todas as métricas por linha | Dados granulares sob demanda |
| Histograma | Distribuição de ticket médio | Padrão de compra dos clientes |
| Waterfall Chart | Contribuição por segmento ao lucro | O que aumenta e o que reduz |
| Decomposition Tree | Drill-down de vendas | Análise exploratória livre |
| Botão voltar | Navegação | Retorno à página de origem |

---

## 📏 Medidas DAX do Projeto

### KPIs Dinâmicos

```dax
-- Meta de Vendas (hipotética para fins de comparação)
Meta Vendas =
CALCULATE(
    [Total Vendas],
    SAMEPERIODLASTYEAR(financials[Date])
) * 1.10       -- Meta = 10% acima do ano anterior

-- % Atingimento da Meta
Atingimento Meta % =
DIVIDE([Total Vendas], [Meta Vendas], 0)

-- Ícone de performance (para uso em cartões)
Ícone Performance =
SWITCH(
    TRUE(),
    [Atingimento Meta %] >= 1,    "✅ Acima da Meta",
    [Atingimento Meta %] >= 0.9,  "⚠️ Próximo da Meta",
    "❌ Abaixo da Meta"
)
```

### Análise de Tendência

```dax
-- Variação vs Mês Anterior
Variação MoM % =
VAR _Atual = [Total Vendas]
VAR _Anterior =
    CALCULATE(
        [Total Vendas],
        DATEADD(financials[Date], -1, MONTH)
    )
RETURN
DIVIDE(_Atual - _Anterior, _Anterior, 0)

-- Melhor Mês (para cartão dinâmico)
Melhor Mês =
CALCULATE(
    FORMAT(
        FIRSTDATE(financials[Date]),
        "MMMM/YYYY"
    ),
    TOPN(1,
        SUMMARIZE(financials,
            financials[Date].[Month],
            "Venda", [Total Vendas]
        ),
        [Venda], DESC
    )
)

-- Média Móvel 3 Meses
Media Movel 3M =
AVERAGEX(
    DATESINPERIOD(
        financials[Date],
        LASTDATE(financials[Date]),
        -3, MONTH
    ),
    [Total Vendas]
)
```

### Métricas Geográficas

```dax
-- Ranking de País por Vendas
Ranking País =
RANKX(
    ALL(financials[Country]),
    [Total Vendas],
    ,
    DESC,
    Dense
)

-- Participação do País no Total
% País no Total =
DIVIDE(
    [Total Vendas],
    CALCULATE([Total Vendas], ALL(financials[Country])),
    0
)
```

---

## ✨ Recursos Avançados Implementados

### 🔖 Sistema de Bookmarks
```
Bookmark "Vista Vendas"   → Gráfico de Vendas visível
Bookmark "Vista Lucro"    → Gráfico de Lucro visível
Bookmark "Vista Unidades" → Gráfico de Unidades visível
```
Botões de toggle estilizados com ícones alternam entre os três estados.

### 🎭 Tooltips Personalizados
Página de tooltip criada com mini-dashboard contendo:
- Miniatura do gráfico de tendência
- KPIs compactos (Vendas, Lucro, Margem)
- Aparece ao passar o mouse em qualquer visual de país ou produto

### 🔍 Drill-through
Configurado na Página 4:
- Clique com botão direito em qualquer produto → vai para Página 4 filtrada
- Botão "Voltar" retorna automaticamente à página de origem

### 📱 Layout Responsivo
- Versão Desktop: 1280×720px
- Versão Mobile: configurada via "Exibição de Telefone" no Power BI

---

## ✅ Checklist do Desafio

- [x] Identidade visual criativa e coesa definida
- [x] Paleta de cores e tipografia padronizadas
- [x] Backgrounds customizados criados para cada página
- [x] Capa / Menu de navegação com botões estilizados
- [x] Página 1 — Performance com gauge e KPIs animados
- [x] Página 2 — Tendências e sazonalidade
- [x] Página 3 — Análise geográfica com mapas
- [x] Página 4 — Drill-through com decomposition tree
- [x] Medidas DAX criadas (KPIs, tendência, geográficas)
- [x] Bookmarks para alternar visuais
- [x] Tooltips personalizados implementados
- [x] Drill-through configurado
- [x] Layout mobile configurado
- [x] Relatório publicado no Power BI Service

---

## 💡 Principais Insights Encontrados

> *(Preencha com os resultados reais do seu relatório)*

- 📊 **Receita total:** R$ ___________
- 📈 **Crescimento YoY:** _____%
- 🏆 **Produto campeão:** ___________
- 🌍 **Maior mercado:** ___________
- 📅 **Melhor trimestre:** ___________
- 💰 **Margem média:** _____%

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- DAX (Data Analysis Expressions)
- Power Query (M Language)
- Figma *(para criação dos backgrounds customizados)*
- Power BI Service

---

## 🚀 Como Reproduzir

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/data-analytics-relatorio-criativo.git

# 2. Abra o arquivo no Power BI Desktop
relatorio_criativo.pbix

# 3. Atualize o caminho da fonte se necessário
# Transformar Dados → Configurações da Fonte de Dados

# 4. Para recriar os backgrounds:
# Abra os arquivos em /assets/ no Figma ou PowerPoint
```

---

## 🔗 Links

- 🔗 [Relatório no Power BI Service](#) *(substitua pelo seu link)*
- 🔗 [Template Figma — Backgrounds](#) *(opcional: link do Figma)*
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
- [Drill-through no Power BI](https://learn.microsoft.com/pt-br/power-bi/create-reports/desktop-drillthrough)
- [Tooltips de relatório](https://learn.microsoft.com/pt-br/power-bi/create-reports/desktop-tooltips)
- [Power BI Mobile](https://learn.microsoft.com/pt-br/power-bi/consumer/mobile/mobile-apps-for-mobile-devices)
