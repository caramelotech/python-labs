# Projetos

Mini projetos para aplicar o que foi aprendido de forma integrada.

## Como usar

1. Leia o enunciado do projeto
2. Planeje antes de codificar - defina módulos, funções, classes e fluxo
3. Implemente incrementalmente, em um ambiente virtual próprio
4. Escreva testes com `pytest` para as partes principais antes de avançar

## Projeto 01 - Jogo de Adivinhação

**Nível:** iniciante

**Objetivo:** Construir um jogo interativo no terminal.

**Descrição:**
O programa sorteia um número entre 1 e 100 e pede palpites até a pessoa acertar, dizendo se o palpite foi alto ou baixo.

**Requisitos:**

- [ ] Sorteia o número com o módulo `random`
- [ ] Lê os palpites com `input` e trata entradas que não são números
- [ ] Mostra quantas tentativas foram necessárias
- [ ] Pergunta se a pessoa quer jogar de novo

**Exemplo de uso:**

```
Adivinhe o número (1 a 100): 50
Muito alto!
Adivinhe o número (1 a 100): 25
Muito baixo!
Adivinhe o número (1 a 100): 37
Acertou em 3 tentativas!
```

## Projeto 02 - Lista de Tarefas (CLI)

**Nível:** intermediário

**Objetivo:** Criar uma ferramenta de linha de comando que persiste dados em arquivo.

**Descrição:**
Construa um gerenciador de tarefas que recebe subcomandos pela linha de comando e salva as tarefas em um arquivo JSON.

**Requisitos:**

- [ ] Subcomandos `add`, `list`, `done` e `remove`, definidos com `argparse`
- [ ] Tarefas representadas por uma `dataclass` com `id`, `titulo` e `concluida`
- [ ] Persistência em `tarefas.json` com o módulo `json` e `pathlib`
- [ ] Erros de leitura e escrita do arquivo tratados e exibidos ao usuário
- [ ] Testes com `pytest` usando a fixture `tmp_path` para não tocar no arquivo real

**Exemplo de uso:**

```
$ python tarefas.py add "Estudar decorators"
Tarefa 1 adicionada.

$ python tarefas.py list
[ ] 1 - Estudar decorators

$ python tarefas.py done 1
Tarefa 1 concluída.
```

## Projeto 03 - API REST de Livros

**Nível:** intermediário

**Objetivo:** Construir uma API HTTP com validação automática e documentação.

**Descrição:**
Crie uma API que permite cadastrar, listar, buscar e remover livros, guardando os dados em memória.

**Requisitos:**

- [ ] API com FastAPI, executada com `uvicorn` (ou `fastapi dev`)
- [ ] Rotas `GET /livros`, `GET /livros/{id}`, `POST /livros` e `DELETE /livros/{id}`
- [ ] Modelos de entrada e saída com Pydantic
- [ ] Códigos de status corretos (`200`, `201`, `204`, `404`, `422`)
- [ ] Documentação interativa acessível em `/docs`
- [ ] Testes com `pytest` e o `TestClient` do FastAPI

**Exemplo de entregável:**

```
$ curl -X POST localhost:8000/livros -H 'Content-Type: application/json' \
    -d '{"titulo":"Python Fluente","autor":"Luciano Ramalho"}'
{"id":1,"titulo":"Python Fluente","autor":"Luciano Ramalho"}
```

## Projeto 04 - API com Banco de Dados

**Nível:** avançado

**Objetivo:** Evoluir a API de livros para um serviço com persistência, autenticação e deploy em container.

**Descrição:**
Substitua o armazenamento em memória do Projeto 03 por um banco de dados e prepare o serviço para produção.

**Requisitos:**

- [ ] Persistência em PostgreSQL com SQLAlchemy e migrações com Alembic
- [ ] Configuração por variáveis de ambiente (`pydantic-settings`), sem segredos no código
- [ ] Autenticação com JWT nas rotas de escrita
- [ ] Logging estruturado das requisições
- [ ] `Dockerfile` e `docker-compose.yml` subindo a API e o banco
- [ ] Testes de integração rodando contra um banco de teste

**Exemplo de entregável:**

```
$ docker compose up -d
$ curl localhost:8000/livros
[{"id":1,"titulo":"Python Fluente","autor":"Luciano Ramalho"}]
```
