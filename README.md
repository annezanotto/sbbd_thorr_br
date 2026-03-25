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

```
.
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
```


    
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

##  Exemplos de Perguntas por Nível de Complexidade

As consultas abaixo foram utilizadas para avaliar o desempenho do sistema em diferentes níveis de complexidade no cenário de Text-to-SQL.

###  Level 1: queries with simple filters

* Quantos edifícios estão localizados na cidade de São Paulo?
* Quantos edifícios estão no bairro Bela Vista?
* Qual a rua de endereço do empreendimento Mirador?
* Qual a quantidade de andares no empreendimento Mirador?
* Liste os id_unidade que possuem um preço maior que R$ 2.000.000.
* Liste os tipos de tipologia que possuem exatamente 3 quartos.
* Qual a área privativa máxima entre as unidades do tipo apartamento?
* Quantas unidades têm exatamente 1 suíte e 2 quartos?
* Quantas unidades com área acima de 150m² estão disponíveis para venda?

---

###  Level 2: queries involving joins and aggregations

* Qual a quantidade de empreendimentos da incorporadora Melnick Even?
* Quais empreendimentos são classificados como residencial e não são de Minha Casa Minha Vida?
* Qual a área média das unidades no bairro Jardim Europa?
* Qual o preço médio das unidades localizadas no bairro Bela Vista?
* Qual o desconto médio das unidades que pertencem à incorporadora Chies?
* Qual a média de desconto para unidades com 4 quartos?
* Qual a média de andares dos edifícios?
* Qual a média da quantidade de banheiros nas unidades localizadas em Porto Alegre?
* Qual a área média das unidades para cada tipo de empreendimento?
* Qual o valor médio de preço para cada tipo de tipologia?

---

###  Level 3: queries with more complex logic and multiple relations

* Liste todos os nomes de empreendimentos com status "pronto novo" e do tipo vertical.
* Liste os empreendimentos que possuem mais de 20 andares e mais de 3 torres.
* Liste os bairros que têm empreendimentos com mais de 15 andares.
* Qual a maior quantidade de vagas de estacionamento registrada em uma única tipologia?
* Liste os empreendimentos que têm tipologias com área total maior que 500 metros quadrados.
* Qual a quantidade média de banheiros em tipologias com 3 quartos?
* Qual o valor máximo já registrado em um preço de unidade?
* Qual o desconto médio geral de todas as unidades?
* Qual o preço médio das unidades que não estão mais disponíveis?
* Qual o preço da unidade mais cara e a qual empreendimento ela pertence?
* Quantas unidades o empreendimento Tendence possui?

---

Esses exemplos cobrem desde consultas simples até cenários mais complexos, permitindo avaliar a robustez do pipeline THoRR na geração de consultas SQL a partir de linguagem natural.


