
# Atividade 02 — Banco de Dados NoSQL com MongoDB

## Disciplina
Banco de Dados / Big Data


##Alunos:
- Alberto Zilio
- Roni Pereira

## Objetivo da Atividade
O objetivo desta atividade é demonstrar o uso de um banco de dados **NoSQL**, realizando:

- Importação de uma base de dados com mais de 500 registros
- Inserção dos dados em um banco NoSQL
- Execução de consultas sobre os dados

---

# Base de Dados Utilizada

Dataset escolhido:

**Automobile Dataset**

Fonte oficial:
https://archive.ics.uci.edu/dataset/10/automobile

Este dataset contém informações sobre automóveis, incluindo:

- fabricante (make)
- tipo de combustível
- número de cilindros
- tamanho do motor
- consumo
- preço
- dimensões do veículo

A base possui aproximadamente **205 registros** e **26 atributos**.

---

# Ambiente Utilizado

O desenvolvimento foi realizado utilizando:

- **Google Colab**
- **Python**
- **MongoDB Atlas (Cloud)**
- Biblioteca **PyMongo**

Ferramentas utilizadas:

| Ferramenta | Função |
|--------|--------|
Google Colab | Ambiente de execução Python |
MongoDB Atlas | Banco NoSQL em nuvem |
PyMongo | Driver de conexão com MongoDB |
Pandas | Manipulação e tratamento dos dados |

---

# Modelo NoSQL Escolhido

O modelo NoSQL utilizado foi:

**Banco de Dados Orientado a Documentos**

Tecnologia:

**MongoDB**

Neste modelo os dados são armazenados como **documentos JSON**, permitindo:

- estrutura flexível
- armazenamento sem esquema fixo
- facilidade para consultas e agregações

---

# Etapas do Projeto

## 1 — Download dos dados

Os arquivos do dataset foram armazenados em um repositório GitHub e baixados automaticamente pelo notebook utilizando a API do GitHub.

Exemplo de código utilizado:

```python
api_url = f"https://api.github.com/repos/{OWNER}/{REPO}/contents/{quote(FOLDER)}?ref={BRANCH}"
```

Os arquivos são então salvos localmente no ambiente do Colab.

---

# 2 — Carregamento e preparação da base

Os dados foram carregados utilizando a biblioteca **Pandas**.

```python
df = pd.read_csv("/content/data/automobile/imports-85.data")
```

Durante o tratamento foram realizadas as seguintes operações:

- remoção de caracteres inválidos
- conversão de nomes de colunas para **snake_case**
- substituição de valores ausentes
- conversão de tipos numéricos

---

# 3 — Conexão com MongoDB Atlas

Foi criado um cluster gratuito no **MongoDB Atlas**.

A conexão foi realizada utilizando **PyMongo**.

Exemplo:

```python
from pymongo import MongoClient
client = MongoClient(uri)
```

O banco utilizado foi:

```
atividade_nosql
```

Coleção criada:

```
automobile
```

---

# 4 — Inserção dos dados no MongoDB

O DataFrame foi convertido em documentos JSON.

```python
records = df.to_dict("records")
collection.insert_many(records)
```

Cada linha do dataset se tornou um documento no MongoDB.

Exemplo de documento:

```json
{
  "make": "toyota",
  "fuel_type": "gas",
  "engine_size": 130,
  "horsepower": 111,
  "price": 16500
}
```

---

# 5 — Consultas no Banco de Dados

Foram realizadas consultas utilizando o modelo de consulta do MongoDB.

## Consulta 1 — Carros da marca Toyota

```python
collection.find({"make":"toyota"})
```

---

## Consulta 2 — Carros com preço maior que 15000

```python
collection.find({"price":{"$gt":15000}})
```

---

## Consulta 3 — Quantidade de carros por fabricante

```python
pipeline = [
 {"$group":{"_id":"$make","quantidade":{"$sum":1}}},
 {"$sort":{"quantidade":-1}}
]
```

---

## Consulta 4 — Média de preço por tipo de carroceria

```python
pipeline = [
 {"$group":{"_id":"$body_style","media_preco":{"$avg":"$price"}}}
]
```

---

# Estrutura do Banco

```
Cluster0
 └── atividade_nosql
      └── automobile
           ├── documento JSON
           ├── documento JSON
           ├── documento JSON
```

Cada documento representa um veículo.

---

# Conclusão

O uso do banco de dados **MongoDB** permitiu armazenar os dados do dataset de forma flexível utilizando documentos JSON.

O modelo **NoSQL orientado a documentos** mostrou-se adequado para esse tipo de aplicação, permitindo consultas eficientes e agregações estatísticas sobre os dados.

A integração entre **Python, Pandas e MongoDB** facilitou o processo de ingestão e análise dos dados.

---

# Autor

Aluno: **Alberto Zilio**

Projeto desenvolvido para fins acadêmicos.
