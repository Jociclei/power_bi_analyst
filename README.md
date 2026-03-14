# 📊 Relatório Financial Sample — Power BI Analyst

> Desafio prático do módulo de Power BI | Bootcamp DIO

---

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como parte do desafio prático do curso **Power BI Analyst** da [DIO (Digital Innovation One)](https://www.dio.me/). O objetivo foi criar um relatório interativo com três páginas utilizando o dataset **Financial Sample**, explorando visuais de mapas, gráficos e KPIs.

---

## 🗂️ Estrutura do Repositório

```
📁 power-bi-financial-report/
├── 📄 README.md
├── 📊 financial_report.pbix        ← Arquivo do relatório Power BI
├── 📂 data/
│   └── financial_sample.xlsx       ← Base de dados utilizada
└── 📂 screenshots/
    ├── pagina_1.png
    ├── pagina_2.png
    └── pagina_3.png
```

---

## 📄 Páginas do Relatório

### Página 1 — Visão Geral de Vendas
Replicada conforme o curso. Contém:
- Cartões de KPI: Total de Vendas, Lucro e Unidades Vendidas
- Gráfico de barras: Vendas por Produto
- Segmentador de dados por Ano e Mês

### Página 2 — Análise por Segmento e País
Replicada conforme o curso. Contém:
- Gráfico de colunas: Lucro por Segmento
- Tabela: Desempenho por País
- Segmentador de dados por Segmento

### Página 3 — Distribuição Geográfica *(página autoral)*
Criada de forma independente como parte do desafio. Contém:

| Visual | Descrição |
|--------|-----------|
| 🗺️ **Mapa de Vendas** | Soma de `Sales` e `Units Sold` por País |
| 🗺️ **Mapa de Lucro** | Soma de `Profit` por País |
| 🥧 **Pizza — Lucro por Segmento** | Distribuição percentual do lucro entre os segmentos |

> **Dicas de ferramentas (tooltips)** configuradas nos mapas exibindo: Vendas totais, Lucro e Unidades vendidas.

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/)
- Microsoft Excel (Financial Sample)
- Power BI Service (publicação)

---

## 📦 Dataset

Os dados utilizados são o **Financial Sample** disponibilizado no repositório do curso:

🔗 [https://github.com/julianazanelatto/power_bi_analyst](https://github.com/julianazanelatto/power_bi_analyst)

---

## 🚀 Como Visualizar o Relatório

### Opção 1 — Power BI Desktop
1. Clone este repositório
```bash
git clone https://github.com/seu-usuario/power-bi-financial-report.git
```
2. Abra o arquivo `financial_report.pbix` no Power BI Desktop

### Opção 2 — Power BI Service
🔗 [Acesse o relatório publicado aqui](#) *(substitua pelo seu link)*

---

## ✅ Checklist do Desafio

- [x] Página 1 replicada
- [x] Página 2 replicada
- [x] Página 3 criada com os 3 visuais obrigatórios
- [x] Nomes dos visuais renomeados para contexto claro
- [x] Tooltips configurados nos mapas
- [x] Layout organizado e coerente
- [x] Relatório publicado no Power BI Service
- [x] Projeto salvo em `.pbix`

---

## 👤 Autor

Feito com 💙 como parte do portfólio de projetos de dados.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](#)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](#)

---

## 📚 Referências

- [Repositório base do curso — julianazanelatto](https://github.com/julianazanelatto/power_bi_analyst)
- [Documentação Power BI](https://learn.microsoft.com/pt-br/power-bi/)
- [DIO — Digital Innovation One](https://www.dio.me/)
