# Monitor de Preços de Medicamentos (PMVG)

Este projeto é um sistema completo para monitorar, analisar e exportar dados de preços de medicamentos. A aplicação importa planilhas públicas da tabela PMVG (Preço Máximo de Venda ao Governo), disponibilizadas pela Anvisa, e oferece uma interface web para consulta e extração de informações.

O sistema foi desenvolvido com uma arquitetura de microsserviços, utilizando Docker para orquestrar o back-end (API REST) e o front-end (SPA).

## ✨ Funcionalidades Principais

* **Importação de Dados**: Um comando customizado para importar dados de medicamentos a partir de planilhas `.xls`.
* **Busca e Filtragem Avançada**: Uma API robusta que permite buscar textualmente e filtrar itens por laboratório, faixa de preço e outros atributos.
* **Painel de Estatísticas**: Endpoint dedicado para calcular e exibir estatísticas (menor, maior, média e mediana) sobre os dados filtrados em tempo real.
* **Exportação para XLS**: Funcionalidade na interface para selecionar múltiplos itens e exportá-los em um novo arquivo `.xls`.
* **Interface Reativa**: Front-end moderno construído com Vue.js 3, oferecendo uma experiência de usuário fluida e interativa.

## 🛠️ Tecnologias Utilizadas

| Área               | Tecnologia                                        |
| ------------------ | ------------------------------------------------- |
| **Back-end** | Django 4, Django REST Framework                   |
| **Front-end** | Vue 3 (Composition API), Pinia, Vite              |
| **Banco de Dados** | PostgreSQL                                        |
| **Containerização**| Docker, Docker Compose                            |
| **Testes** | Pytest (Back-end), Vitest/Jest (Front-end)        |

---

## 🚀 Como Executar o Projeto Localmente

É necessário ter o **Docker** e o **Docker Compose** instalados.

### 1. Clone o Repositório

```bash
git clone <URL_DO_SEU_REPOSITORIO>
cd <NOME_DO_SEU_REPOSITORIO>
```

### 2. Suba os Containers

O comando a seguir irá construir as imagens e iniciar os serviços de API (back-end), Web (front-end) e o banco de dados.

```bash
docker-compose up --build -d
```

### 3. Importe os Dados Iniciais

Para popular o banco de dados, execute o comando de ingestão de dados. O projeto já inclui um arquivo de exemplo em `sample_data/`.

```bash
docker-compose exec api python manage.py ingest_licitacoes sample_data/xls_conformidade_gov_20250414_195721251.xls
```

O comando utiliza `update_or_create` para evitar duplicatas e permitir re-execuções seguras.

### 4. Acesse as Aplicações

* **Front-end (Vue)**: [http://localhost:3000](http://localhost:3000)
* **Back-end (API Django)**: [http://localhost:8000/api/items/](http://localhost:8000/api/items/)

### 5. Para Parar a Execução

Para derrubar todos os containers e remover os volumes, use o comando:

```bash
docker-compose down -v
```

---

## 🧪 Como Rodar os Testes

Os testes automatizados garantem a qualidade e a integridade do código.

### Testes do Back-end (Pytest)

```bash
docker-compose exec api pytest
```

### Testes do Front-end (Vitest/Jest)

```bash
docker-compose exec web npm run test
```

---

## 📖 Documentação da API

A API REST oferece os seguintes endpoints:

### Listar e Filtrar Itens

* **Endpoint**: `GET /api/items/`
* **Descrição**: Retorna uma lista paginada (100 itens por página) de medicamentos.
* **Parâmetros de Query**:
    * `search=<termo>`: Busca textual.
    * `laboratorio=<nome>`: Filtra por nome do laboratório.
    * `preco_min=<valor>`: Filtra por preço mínimo.
    * `preco_max=<valor>`: Filtra por preço máximo.

### Obter Estatísticas

* **Endpoint**: `GET /api/items/stats/`
* **Descrição**: Retorna um objeto JSON com estatísticas (`min`, `max`, `mean`, `median`) calculadas sobre os itens que correspondem aos filtros aplicados.
* **Parâmetros de Query**: Aceita os mesmos parâmetros do endpoint de listagem.

---

## 📂 Estrutura do Projeto

```
.
├── .github/
│   └── workflows/
│       └── ci.yaml
├── backend/
│   ├── core/
│   │   ├── management/
│   │   │   └── commands/
│   │   │       └── ingest_licitacoes.py
│   │   ├── tests/
│   │   └── ...
│   ├── settings.py
│   └── manage.py
├── frontend/
│   ├── src/
│   └── ...
├── sample_data/
│   └── xls_conformidade_gov_20250414_195721251.xls
├── docker-compose.yml
└── README.md
```
