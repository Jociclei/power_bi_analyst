# 🔄 Processando e Transformando Dados com Power BI

> Desafio prático — ETL, Power Query e Modelagem de Dados | Bootcamp DIO

---

## 📌 Sobre o Projeto

Projeto desenvolvido como parte do bootcamp **Power BI Analyst** da [DIO](https://www.dio.me/). O foco deste desafio foi o processo completo de **ETL (Extract, Transform, Load)** utilizando o **Power Query** do Power BI, conectando a uma base de dados MySQL na nuvem, realizando transformações e preparando os dados para análise.

---

## 🎯 Objetivos do Desafio

- Conectar o Power BI a um banco de dados **MySQL hospedado na Azure**
- Verificar e tratar problemas na base de dados
- Realizar transformações e limpeza de dados via **Power Query**
- Aplicar boas práticas de modelagem para relatórios

---

## 🗂️ Estrutura do Repositório

```
📁 power-bi-transformacao-dados/
├── 📄 README.md
├── 📊 relatorio_transformado.pbix     ← Arquivo Power BI
├── 📂 database/
│   └── script_criacao_bd.sql          ← Script de criação do banco MySQL
├── 📂 screenshots/
│   ├── conexao_mysql.png
│   ├── power_query_transformacoes.png
│   └── modelo_final.png
└── 📂 docs/
    └── instrucoes_desafio.docx
```

---

## 🛢️ Configuração do Banco de Dados

### Instância MySQL — Azure
O banco de dados **company** foi criado e hospedado em uma instância MySQL na **Microsoft Azure**, contendo as seguintes tabelas:

| Tabela | Descrição |
|--------|-----------|
| `employee` | Dados dos funcionários |
| `department` | Departamentos da empresa |
| `dept_location` | Localizações dos departamentos |
| `project` | Projetos em andamento |
| `works_on` | Relação funcionário × projeto |
| `dependent` | Dependentes dos funcionários |

---

## 🔄 Etapas de Transformação — Power Query

### 1. Conexão com MySQL na Azure
- Configuração da string de conexão com servidor, banco e credenciais
- Verificação do carregamento de todas as tabelas

### 2. Verificação e Correção de Problemas

| Problema encontrado | Ação realizada |
|--------------------|----------------|
| Cabeçalhos incorretos | Promovida a primeira linha como cabeçalho |
| Tipos de dados errados | Revisão e correção de cada coluna |
| Valores `null` em colunas críticas | Identificação de funcionários sem gerente (`Super_ssn = null`) — corresponde ao gerente geral |
| Coluna `salary` como texto | Convertida para tipo Número Decimal |

### 3. Separação de Colunas Complexas
```
Coluna "Address" separada em:
  → Número
  → Rua
  → Cidade
  → Estado
```

### 4. Mesclagem de Tabelas

#### Employee + Department (via Merge)
- Objetivo: associar o nome do departamento a cada funcionário
- Tipo de junção: **Left Outer Join** (mantém todos os funcionários)

> ⚠️ **Mesclar vs Atribuir (Append):**
> - **Mesclar (Merge):** combina tabelas horizontalmente com base em uma chave — usado para enriquecer dados (ex: trazer nome do depto para cada funcionário)
> - **Atribuir (Append):** empilha tabelas verticalmente — usado para unir registros do mesmo tipo (ex: duas tabelas de funcionários de filiais diferentes)
>
> Neste caso, **Merge** é o correto pois queremos cruzar informações de tabelas relacionadas.

#### Gerente por Departamento
Criada uma tabela auxiliar com:
```
Department → Merge com Employee (via Mgr_ssn = Ssn)
Resultado: nome do gerente associado a cada departamento
```

### 5. Junção Funcionário + Gerente (Auto-relacionamento)
```
Tabela employee mesclada com ela mesma:
  employee.Super_ssn = employee.Ssn
  → Nova coluna: "Nome do Gerente"
```

### 6. Tabela Colaboradores × Gerente (para hierarquia)
Criada tabela simplificada contendo apenas:
- Nome do colaborador
- Nome do gerente direto

### 7. Mesclagem de Nome e Sobrenome
```
Fname + Minit + Lname → "Nome Completo"
(concatenação com espaços, coluna Minit tratada para ausência de valor)
```

### 8. Eliminação de Colunas Desnecessárias
Removidas todas as colunas que não serão utilizadas nos visuais, reduzindo o tamanho do modelo e melhorando a performance.

### 9. Contagem de Colaboradores por Gerente
```
Agrupamento por "Nome do Gerente"
→ Coluna derivada: "Qtd. Colaboradores"
```

### 10. Verificação de Departamentos sem Gerente
- Identificação de registros com `Mgr_ssn` nulo
- Análise: departamentos sem gerente atribuído foram marcados como **"Sem Gerente"**

---

## 📐 Modelo de Dados Final

Após todas as transformações, o modelo ficou estruturado como:

```
employee ──────< works_on >────── project
    │
    ├──── department ──── dept_location
    │
    └──── dependent
```

- Relacionamentos definidos entre as tabelas
- Cardinalidades ajustadas (1:N e N:M via tabela associativa)
- Direções de filtro configuradas para otimizar as consultas

---

## ✅ Checklist do Desafio

- [x] Instância MySQL criada na Azure
- [x] Banco de dados `company` populado com o script SQL
- [x] Conexão Power BI → MySQL configurada
- [x] Cabeçalhos e tipos de dados corrigidos
- [x] Valores nulos analisados e tratados
- [x] Coluna `Address` separada em partes
- [x] Colunas `Fname`, `Minit`, `Lname` mescladas em "Nome Completo"
- [x] Tabela `employee` + `department` mesclada (Merge)
- [x] Tabela de Colaboradores por Gerente criada
- [x] Contagem de colaboradores por gerente realizada
- [x] Tabela de Departamentos + Gerente criada
- [x] Colunas desnecessárias removidas
- [x] Relatório publicado no Power BI Service

---

## 💡 Principais Aprendizados

```
✔ Conexão com bancos de dados em nuvem via Power BI
✔ Diferença prática entre Merge (mesclar) e Append (atribuir)
✔ Tratamento de valores nulos com critério (não apagar sem analisar)
✔ Auto-relacionamento para hierarquia de gerentes
✔ Boas práticas: remover colunas desnecessárias antes de carregar
✔ Tipos de dados corretos impactam diretamente nos cálculos DAX
```

---

## 🛠️ Tecnologias Utilizadas

- [Power BI Desktop](https://powerbi.microsoft.com/pt-br/desktop/)
- Power Query (M Language)
- MySQL 8.0
- Microsoft Azure (MySQL Flexible Server)
- Power BI Service

---

## 🚀 Como Reproduzir o Projeto

### Passo 1 — Criar o banco no MySQL (Azure ou local)
```sql
-- Execute o script disponível em /database/script_criacao_bd.sql
source script_criacao_bd.sql;
```

### Passo 2 — Conectar o Power BI
```
Power BI Desktop
  → Obter Dados
  → Banco de Dados MySQL
  → Servidor: <seu-servidor>.mysql.database.azure.com
  → Banco: company
```

### Passo 3 — Aplicar as transformações
```
Power BI Desktop
  → Transformar Dados (Power Query)
  → Aplicar as etapas descritas neste README
  → Fechar e Aplicar
```

### Passo 4 — Publicar
```
Power BI Desktop
  → Arquivo → Publicar
  → Escolha seu Workspace no Power BI Service
```

---

## 📸 Screenshots

### Conexão MySQL
![Conexão](screenshots/conexao_mysql.png)

### Transformações no Power Query
![Power Query](screenshots/power_query_transformacoes.png)

### Modelo de Dados Final
![Modelo](screenshots/modelo_final.png)

---

## 🔗 Links Úteis

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
- [Power Query — Documentação Microsoft](https://learn.microsoft.com/pt-br/power-query/)
- [MySQL no Azure](https://learn.microsoft.com/pt-br/azure/mysql/)
- [Modelagem no Power BI](https://learn.microsoft.com/pt-br/power-bi/guidance/star-schema)
