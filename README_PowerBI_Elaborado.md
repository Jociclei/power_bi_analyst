# 📊 Relatório Elaborado — Financial Sample | Power BI Analyst

> Desafio prático — Relatório com navegação, segmentadores e indicadores | Bootcamp DIO

---

## 📌 Sobre o Projeto

Relatório interativo desenvolvido com o dataset **Financial Sample** do Power BI, como parte do bootcamp **Power BI Analyst** da [DIO](https://www.dio.me/). O foco deste desafio foi a construção de um relatório mais elaborado, com atenção à **experiência do usuário**: navegação fluida entre páginas, segmentadores visuais, botões com imagem e uso de indicadores para alternar entre visuais.

---

## 🗂️ Estrutura do Repositório

```
📁 power-bi-financial-elaborado/
├── 📄 README.md
├── 📊 relatorio_financial.pbix       ← Arquivo principal do relatório
├── 📂 data/
│   └── financial_sample.xlsx         ← Base de dados utilizada
└── 📂 screenshots/
    ├── pagina_1_overview.png
    └── pagina_2_detalhes.png
```

---

## 📄 Páginas do Relatório

### Página 1 — Visão Executiva de Vendas

Estrutura e elementos presentes:

| Elemento | Descrição |
|----------|-----------|
| 🃏 **Cartões KPI** | Total de Vendas, Lucro Líquido, Unidades Vendidas e COGS |
| 📊 **Gráfico de barras** | Vendas por Produto |
| 📈 **Gráfico de linhas** | Evolução mensal de Vendas e Lucro |
| 🗺️ **Mapa** | Distribuição de vendas por País |
| 🔘 **Segmentadores** | Filtros por Ano, Segmento e País |
| 🖱️ **Botões de navegação** | Setas para ir à Página 2 |
| 🖼️ **Botões com imagem** | Ícones personalizados para ações e filtros |

### Página 2 — Análise Detalhada por Segmento e Produto

| Elemento | Descrição |
|----------|-----------|
| 🥧 **Gráfico de pizza** | Lucro por Segmento |
| 📊 **Gráfico de colunas** | Vendas x Lucro por Produto |
| 📋 **Tabela detalhada** | País, Segmento, Produto, Vendas e Lucro |
| 🔘 **Segmentadores** | Filtros por Segmento e Período |
| 🖱️ **Botões de navegação** | Retorno à Página 1 |
| 🔀 **Indicadores + Botões** | Alternância entre visão de Vendas e visão de Lucro no mesmo espaço |

---

## ✨ Recursos e Técnicas Aplicadas

### 🔘 Botões de Navegação
Botões configurados com ação **"Navegação de Página"** permitindo transitar entre as páginas sem usar as abas nativas, oferecendo uma experiência de aplicativo.

### 🖼️ Botões com Imagem
Ícones personalizados inseridos como botões, com estados de hover e pressionado configurados, associados a ações de filtro ou navegação.

### 📌 Segmentadores Visuais
Segmentadores estilizados no formato de **lista** e **dropdown**, sincronizados entre páginas para manter o contexto de filtro ao navegar.

### 🔀 Indicadores (Bookmarks) + Botões
Uso de **indicadores** para salvar dois estados do relatório (ex: gráfico de Vendas visível / gráfico de Lucro visível) e **botões** para alternar entre eles — simulando abas dentro da mesma página.

```
Fluxo dos Indicadores:
  [Botão "Ver Vendas"] → Indicador A (gráfico de vendas visível)
  [Botão "Ver Lucro"]  → Indicador B (gráfico de lucro visível)
```

### 🎨 Layout e Design
- Fundo personalizado com formas e objetos para estruturar visualmente cada seção
- Paleta de cores consistente em todo o relatório
- Tipografia padronizada nos títulos e rótulos

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- Power BI Service (publicação e compartilhamento)
- Microsoft Excel — Financial Sample

---

## 📦 Dataset

Base de dados fornecida pelo curso:

🔗 [https://github.com/julianazanelatto/power_bi_analyst](https://github.com/julianazanelatto/power_bi_analyst)

**Principais campos utilizados:**

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `Sales` | Numérico | Valor total de vendas |
| `Profit` | Numérico | Lucro obtido |
| `Units Sold` | Numérico | Unidades vendidas |
| `COGS` | Numérico | Custo dos produtos vendidos |
| `Product` | Texto | Nome do produto |
| `Segment` | Texto | Segmento de mercado |
| `Country` | Texto | País da venda |
| `Date` | Data | Data da transação |

---

## 🚀 Como Visualizar

### Opção 1 — Power BI Desktop
```bash
# Clone o repositório
git clone https://github.com/seu-usuario/power-bi-financial-elaborado.git

# Abra o arquivo no Power BI Desktop
relatorio_financial.pbix
```

### Opção 2 — Power BI Service
🔗 [Acesse o relatório publicado](#) *(substitua pelo seu link após publicar)*

---

## ✅ Checklist do Desafio

- [x] Estrutura visual definida com layout elaborado
- [x] Botões de navegação entre páginas
- [x] Segmentadores estilizados e funcionais
- [x] Botões com imagem associada
- [x] Indicadores (bookmarks) para alternar visuais
- [x] Página 1 completa com KPIs, gráficos e mapa
- [x] Página 2 completa com análise detalhada
- [x] Relatório publicado no Power BI Service
- [x] Repositório organizado no GitHub

---

## 📸 Preview do Relatório

### Página 1 — Visão Executiva
![Página 1](screenshots/pagina_1_overview.png)

### Página 2 — Análise Detalhada
![Página 2](screenshots/pagina_2_detalhes.png)

---

## 👤 Autor

Desenvolvido como parte do portfólio de projetos de dados.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](#)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](#)
[![DIO](https://img.shields.io/badge/DIO-E94D5F?style=for-the-badge&logo=dio&logoColor=white)](https://www.dio.me/)

---

## 📚 Referências

- [Repositório base — julianazanelatto](https://github.com/julianazanelatto/power_bi_analyst)
- [Documentação Power BI — Microsoft](https://learn.microsoft.com/pt-br/power-bi/)
- [Indicadores no Power BI](https://learn.microsoft.com/pt-br/power-bi/create-reports/desktop-bookmarks)
- [Botões no Power BI](https://learn.microsoft.com/pt-br/power-bi/create-reports/desktop-buttons)
