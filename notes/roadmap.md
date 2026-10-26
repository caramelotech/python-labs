# Python Roadmap

Um guia prático e progressivo para aprender Python e construir aplicações de backend.

## 🧱 1. Fundamentos de Programação

### Lógica de Programação

- Algoritmos e resolução de problemas
- Pseudocódigo
- Linguagens compiladas vs interpretadas

### Conceitos básicos

- Variáveis e constantes
- Tipos de dados
- Comentários
- Entrada e saída no terminal

### Controle de fluxo

- Condicionais
- Estruturas de repetição
- Funções

## 🐍 2. Primeiros Passos com Python

### Ambiente

- Instalação do Python
- REPL
- Ambientes virtuais (`venv`) e `pip`
- uv
- Editor com Pylance ou Pyright

### Sintaxe

- Indentação e blocos
- Tipos básicos: `int`, `float`, `str`, `bool`, `None`
- Conversão de tipos
- Operadores
- f-strings

### Controle de fluxo em Python

- `if`, `elif` e `else`
- `for` com `range`, `enumerate` e `zip`
- `while`
- `match` (pattern matching estrutural)

### Funções

- Parâmetros posicionais e nomeados
- Valores padrão
- `*args` e `**kwargs`
- Funções como objetos e `lambda`
- Escopo e closures

## 📦 3. Estruturas de Dados

- Listas e tuplas
- Dicionários e conjuntos
- Fatiamento
- List, dict e set comprehensions
- Módulo `collections` (`Counter`, `defaultdict`, `deque`, `namedtuple`)
- Strings e bytes

## 🧩 4. Orientação a Objetos

- Classes, instâncias e `self`
- Atributos de classe e de instância
- Herança e `super()`
- Métodos especiais (dunder methods)
- `@property`, `@classmethod` e `@staticmethod`
- Dataclasses
- Protocolos e classes abstratas (`abc`)

## ⚠️ 5. Exceções

- `try`, `except`, `else` e `finally`
- Exceções da biblioteca padrão
- Exceções customizadas
- Gerenciadores de contexto (`with`)

## 🗂️ 6. Módulos e Pacotes

- `import` e organização em módulos
- Pacotes e `__init__.py`
- `if __name__ == "__main__"`
- `pyproject.toml`
- Publicação no PyPI

## 🏷️ 7. Type Hints

- Anotações básicas
- `list[int]`, `dict[str, int]`, `X | None`
- `TypedDict`, `Protocol` e generics
- Verificação estática com mypy ou Pyright

## 🔁 8. Python Avançado

- Iteradores e geradores (`yield`)
- Decorators
- Context managers customizados
- `functools` e `itertools`
- Descritores e metaclasses (conceitual)

## ⚡ 9. Concorrência

- O GIL e o que ele significa
- `threading` para tarefas de I/O
- `multiprocessing` e `concurrent.futures`
- `asyncio`, `async` e `await`
- Python sem GIL (free-threaded)

## 📚 10. Biblioteca Padrão

- `pathlib` e `os`
- `json` e `csv`
- `datetime` e `zoneinfo`
- `re` (expressões regulares)
- `logging`
- `argparse`
- `sqlite3`

## 🧪 11. Testes e Qualidade

- `pytest`: testes, fixtures e parametrização
- Mocks com `unittest.mock`
- Cobertura com `coverage`
- Ruff (formatação e lint)
- `pre-commit`

## 🌐 12. Desenvolvimento Web e APIs

### HTTP e REST

- Métodos, status codes e cabeçalhos
- REST e boas práticas de API
- Cliente HTTP com `httpx` ou `requests`

### Frameworks

- FastAPI e Pydantic
- Django e Django REST Framework
- Flask

### Servidores

- WSGI vs ASGI
- Uvicorn e Gunicorn

## 🗄️ 13. Banco de Dados

- SQL e modelagem relacional
- SQLAlchemy (Core e ORM)
- Migrações com Alembic
- Django ORM
- Redis

## 🔐 14. Segurança

- Autenticação vs autorização
- Hash de senhas (bcrypt, Argon2)
- JWT
- OAuth 2 e OIDC
- Variáveis de ambiente e gestão de segredos
- OWASP Top 10

## 📨 15. Tarefas Assíncronas e Mensageria

- Celery
- Filas com Redis e RabbitMQ
- Kafka

## 📊 16. Observabilidade

- Logging estruturado
- Métricas com Prometheus
- Rastreamento distribuído com OpenTelemetry
- Monitoramento de erros (Sentry)

## ☁️ 17. Containers e Deploy

- Docker para aplicações Python
- Docker Compose
- CI/CD com GitHub Actions
- Deploy em cloud (AWS, GCP, Azure, Render, Railway)

## 🎯 18. Tópicos Avançados

- Performance e profiling (`cProfile`)
- Gerenciamento de memória
- Extensões em C e Rust (PyO3)
- Arquitetura de aplicações (camadas, hexagonal)

## 🚀 Próximos Passos - Da Sintaxe à Produção

1. Domine a base: estruturas de dados, funções, classes e exceções
2. Use type hints e escreva testes com `pytest` desde o início
3. Construa uma API com FastAPI, Pydantic e SQLAlchemy
4. Adicione autenticação, logging estruturado e tarefas assíncronas
5. Empacote com Docker, publique com CI/CD e observe com OpenTelemetry

## 🧠 Dica

> Foque em **projetos reais** ao invés de só teoria.

Sugestões:

- Script que automatiza uma tarefa chata do seu dia a dia
- CLI com `argparse` e testes
- API REST completa com FastAPI, PostgreSQL e autenticação
- Bot para Discord ou Telegram

🔥 **Regra de ouro:**
Aprenda - Aplique - Ensine - Repita
