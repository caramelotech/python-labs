# Configurando o Ambiente

Antes de escrever qualquer código Python, você precisa de três coisas:

1. **Interpretador Python** - o programa que lê e executa o seu código
2. **Ambiente virtual** - uma pasta isolada com as dependências de cada projeto
3. **Editor com suporte a Python** - para ter autocompletar, verificação de tipos e formatação automática

Este guia usa o Python 3. O Python 2 chegou ao fim da vida em 2020 e não deve ser usado em projetos novos.

## Instalando o Python

Baixe o instalador oficial em [python.org/downloads](https://www.python.org/downloads/) e escolha a versão estável mais recente.

### Windows

1. Execute o instalador `.exe`
2. **Marque a opção "Add python.exe to PATH"** na primeira tela - é o erro mais comum de instalação
3. Clique em "Install Now" e reabra o terminal ao final

O instalador também traz o **`py`**, um lançador que escolhe a versão do Python instalada:

```bash
py --version
py -3.13 script.py
```

Também é possível instalar com:

```bash
winget install Python.Python.3.13
```

Troque `3.13` pela versão estável mais recente.

### macOS

O macOS pode trazer um Python antigo do sistema - não use esse. Instale pelo instalador oficial ou pelo Homebrew:

```bash
brew install python
```

### Linux

A maioria das distribuições já vem com Python 3. Confira a versão e, se precisar, instale pelo gerenciador de pacotes (no Ubuntu, `sudo apt install python3 python3-venv python3-pip`).

No Linux e no macOS, o comando costuma ser `python3` (e `pip3`), porque `python` pode não existir ou apontar para outra versão.

## Verificando a instalação

```bash
python --version     # Windows
python3 --version    # macOS e Linux
```

A saída deve ser algo como:

```
Python 3.X.Y
```

## O REPL

Digite `python` (ou `python3`) sem argumentos para abrir o **REPL**, o modo interativo. Ele executa cada linha assim que você aperta Enter - ótimo para testar ideias rápidas:

```
>>> 2 + 3
5
>>> nome = "Caramelo"
>>> nome.upper()
'CARAMELO'
>>> exit()
```

## Ambientes virtuais

Cada projeto Python tem suas próprias dependências, e versões diferentes de uma mesma biblioteca podem entrar em conflito. Um **ambiente virtual** (`venv`) é uma pasta com uma cópia isolada do Python e das bibliotecas de um único projeto.

Crie o ambiente na pasta do projeto:

```bash
python -m venv .venv
```

Ative o ambiente:

```bash
# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# macOS e Linux
source .venv/bin/activate
```

Com o ambiente ativo, o nome `(.venv)` aparece no início do prompt, e tudo que você instalar com `pip` fica só dentro dele:

```bash
pip install requests
pip freeze > requirements.txt   # registra as dependências do projeto
deactivate                      # sai do ambiente
```

A pasta `.venv/` não vai para o Git - quem clonar o projeto recria o ambiente e instala as dependências com `pip install -r requirements.txt`.

## uv: a alternativa moderna

O [uv](https://docs.astral.sh/uv/) é um gerenciador de projetos e pacotes Python muito rápido, que junta em uma ferramenta o que antes exigia `pip`, `venv` e instaladores de versões do Python:

```bash
uv init ola-python     # cria um projeto com pyproject.toml
cd ola-python
uv add requests        # adiciona uma dependência (e cria o ambiente virtual)
uv run main.py         # executa dentro do ambiente do projeto
```

Para começar, `venv` e `pip` são suficientes e vêm com o Python. Conhecer o uv vale a pena assim que você começar projetos maiores.

## Escolhendo um editor

| Editor         | Como configurar                                                                                                                     |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| **VS Code**    | Instale a extensão oficial [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) (traz o Pylance)           |
| **PyCharm**    | IDE da JetBrains dedicada ao Python, já vem pronta para uso                                                                         |
| **Neovim / Vim** | Configure um language server como o Pyright ou o basedpyright pelo cliente LSP de sua preferência                                   |

No VS Code, use o comando "Python: Select Interpreter" para escolher o Python do ambiente virtual do projeto (`.venv`).

Para formatar e verificar o código, instale o [Ruff](https://docs.astral.sh/ruff/) (há extensão para o VS Code) e ative a **formatação ao salvar**.

## Primeiro programa

Crie um arquivo `main.py`:

```python
print("Olá, Caramelo Tech!")
```

E execute:

```bash
python main.py
```

```
Olá, Caramelo Tech!
```

## Comandos essenciais

| Comando                            | O que faz                                                  |
| ---------------------------------- | ---------------------------------------------------------- |
| `python arquivo.py`                | Executa um script                                          |
| `python -m venv .venv`             | Cria um ambiente virtual                                   |
| `pip install pacote`               | Instala uma biblioteca no ambiente ativo                   |
| `pip install -r requirements.txt`  | Instala as dependências listadas no arquivo                |
| `pip list`                         | Lista as bibliotecas instaladas                            |
| `python -m pytest`                 | Executa os testes (depois de instalar o `pytest`)          |
| `ruff format .` / `ruff check .`   | Formata e verifica o código (depois de instalar o `ruff`)  |

## Sem instalar nada

Para testar um trecho rápido, use o [Python Tutor](https://pythontutor.com/), que executa o código passo a passo e mostra as variáveis na memória, ou um notebook no [Google Colab](https://colab.research.google.com/).

## Problemas comuns

### `'python' não é reconhecido` (Windows)

O Python não foi adicionado ao `PATH`. Rode o instalador de novo, escolha "Modify" e marque "Add Python to environment variables", ou use o lançador `py`. Se o comando abrir a Microsoft Store, desative os atalhos em "Configurações > Aplicativos > Aliases de execução do aplicativo".

### `error: externally-managed-environment` (Linux e macOS)

O sistema impede instalar pacotes no Python global para não quebrar ferramentas do sistema. Crie e ative um ambiente virtual antes de usar o `pip`.

### Execução de scripts desabilitada ao ativar o `.venv` (PowerShell)

Libere scripts locais para o seu usuário:

```bash
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

## Referências

- [Downloads - python.org](https://www.python.org/downloads/)
- [Usando Python no Windows - documentação oficial](https://docs.python.org/pt-br/3/using/windows.html)
- [venv - documentação oficial](https://docs.python.org/pt-br/3/library/venv.html)
- [Installing packages using pip and virtual environments - Python Packaging User Guide](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)
- [uv - documentação](https://docs.astral.sh/uv/)
