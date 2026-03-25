# THoRR-BR: Text-to-SQL com Refinamento e LLM Local

Este projeto implementa um pipeline de **Text-to-SQL baseado em LLMs locais**, utilizando a estratégia de **refinamento de tabelas e colunas (THoRR)** para reduzir ambiguidades e melhorar a geração de consultas SQL.

---

## Visão Geral

O sistema permite que usuários façam perguntas em linguagem natural e obtenham respostas diretamente de um banco de dados SQLite.

O pipeline segue três etapas principais:

1.  **Recuperação de tabelas relevantes (FAISS)**
2.  **Refinamento de colunas (FAISS)**
3.  **Geração de SQL com LLM local**

O sistema também inclui:

* Classificação de intenção
* Execução de queries SQL
* Modo conversacional

---

##  Arquitetura
Usuário → Classificador de Intenção → Pipeline THoRR → LLM → SQL → Banco SQLite → Resultado


##  Estrutura do Projeto

├── main.py
├── config.py
├── setup_database.py
├── analytics.py
├── requirements.txt
├── database.db
├── tables/
├── tables_cleaned/
└── assistant/
    ├── pipeline.py
    ├── executa_sql.py
    ├── intent_classifier.py
    ├── conversation.py
    └── local_llm.py
##  Funcionalidades

###  Interface principal

* Loop interativo via terminal 
* Classificação automática de intenção
* Execução de queries SQL

###  Pipeline THoRR

* Recuperação semântica com FAISS 
* Refinamento de colunas relevantes
* Geração de SQL com contexto reduzido

###  Banco de dados

* SQLite gerado a partir de arquivos Excel 
* Limpeza de dados:

  * remoção de outliers
  * normalização de texto
  * tratamento de nulos

##  Como executar

### 1. Clonar o repositório

```bash
git clone [https://github.com/seu-usuario/seu-repo.git](https://github.com/annezanotto/sbbd_thorr_br.git)


### 2. Criar ambiente virtual (recomendado)

```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows
```

---

### 3. Instalar dependências

```bash
pip install -r requirements.txt
```

---

### 4. Preparar o banco de dados

Certifique-se de ter os arquivos Excel na pasta `tables/`:

```
tables/
  buildings.xlsx
  typologies.xlsx
  units.xlsx
  units_updates.xlsx
```

Execute:

```bash
python setup_database.py
```

Isso irá:

* Criar o banco `database.db`
* Gerar arquivos limpos em `tables_cleaned/`

---

### 5. Rodar o assistente

```bash
python main.py
```

---

##  Exemplos de uso

```
> Quantos imóveis existem em Porto Alegre?
> Qual a média de área das unidades?
> Liste os prédios da Melnick Even
```

---

##  Modelos utilizados

* Embeddings: `intfloat/multilingual-e5-large`
* LLM local: `mistralai/Mistral-7B-Instruct-v0.2` 

---

##  Classificação de Intenção

O sistema classifica automaticamente a pergunta em:

* `SQL_QUERY` → consulta ao banco
* `DATA_ASSISTANCE` → explicação dos dados
* `GENERAL_CONVERSATION` → conversa geral 

---

##  Diferencial do Projeto

✔ Uso de LLM local (sem dependência de API)
✔ Pipeline THoRR para redução de alucinação
✔ Integração com FAISS para busca semântica
✔ Limpeza e avaliação de qualidade de dados
✔ Correção automática de SQL gerado



##  Requisitos

* Python 3.10+
* GPU recomendada (para rodar o modelo local)
* Espaço em disco para modelos HuggingFace

---

##  Observações

* O modelo pode ser pesado (carregamento inicial lento)
* O pipeline pode gerar SQL inválido em alguns casos (há correção automática parcial)
* Os dados são baseados em arquivos Excel locais

---

##  Referência

Este projeto implementa conceitos baseados em:

- KIM, Kihun; KIM, Mintae; LEE, Hokyung; PARK, Seongik; HAN, Youngsub; JEON, Byoung-Ki.  
  **THoRR: Complex Table Retrieval and Refinement for RAG**.  
  LG UPLUS, Seoul, Republic of Korea, 2024.

---

##  Autora

Projeto desenvolvido como parte de TCC / pesquisa em Text-to-SQL e LLMs.


