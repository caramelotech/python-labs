# Módulos

Conforme um programa cresce, manter tudo em um único arquivo vira um problema. Módulos são a forma de dividir código em partes organizadas e reutilizáveis - e também como você usa código que outras pessoas já escreveram.

## O que é um módulo

Todo arquivo `.py` é, por definição, um módulo. Se você tem um arquivo `calculadora.py`, ele já pode ser importado por outro arquivo como o módulo `calculadora`.

- **Módulo**: um único arquivo `.py`
- **Pacote**: uma pasta com vários módulos relacionados (visto na próxima seção)
- **Biblioteca**: um conjunto de módulos/pacotes distribuído para reuso (a biblioteca padrão do Python, ou algo instalado via `pip`)

## Importando módulos

A forma mais simples importa o módulo inteiro, e você acessa o conteúdo com `modulo.nome`:

```python
import math

print(math.sqrt(16))   # 4.0
print(math.pi)          # 3.141592653589793
```

Quando você só precisa de um ou outro item, `from ... import ...` traz esse item direto para o escopo atual, sem precisar prefixar com o nome do módulo:

```python
from math import sqrt, pi

print(sqrt(16))
print(pi)
```

Para módulos com nome longo (ou para evitar conflito com outro nome já usado), um alias com `as` ajuda:

```python
import datetime as dt

agora = dt.datetime.now()
```

Evite `from modulo import *`: isso importa tudo que o módulo expõe para o escopo atual, sem deixar claro de onde cada nome veio - dificulta a leitura e pode sobrescrever nomes que você já tinha.

A biblioteca padrão do Python já traz módulos prontos para tarefas comuns, sem precisar instalar nada extra:

```python
import random
import datetime
import os
import sys

print(random.randint(1, 10))     # número aleatório entre 1 e 10
print(datetime.date.today())      # data de hoje
print(os.getcwd())                # diretório atual
print(sys.version)                # versão do Python em uso
```

Um detalhe sobre importação: o Python carrega e executa um módulo **uma única vez** por execução do programa, mesmo que ele seja importado em vários arquivos diferentes. Da segunda importação em diante, o Python reaproveita o módulo já carregado em cache (em `sys.modules`), em vez de executar o arquivo de novo.

## Criando módulos próprios

Organizar código em módulos separados é só uma questão de criar arquivos `.py` e importar um no outro:

```python
# arquivo: operacoes.py
def soma(a: float, b: float) -> float:
    return a + b


def subtrai(a: float, b: float) -> float:
    return a - b
```

```python
# arquivo: main.py
from operacoes import soma, subtrai

print(soma(5, 3))      # 8
print(subtrai(5, 3))   # 2
```

### `if __name__ == "__main__":`

Toda vez que um módulo é executado diretamente (`python arquivo.py`), o Python define a variável especial `__name__` como `"__main__"` nesse arquivo. Quando o mesmo arquivo é importado por outro, `__name__` recebe o nome do módulo em vez disso.

Esse comportamento permite escrever um arquivo que funciona tanto como script quanto como módulo importável:

```python
# arquivo: operacoes.py

def soma(a: float, b: float) -> float:
    return a + b


if __name__ == "__main__":
    # só executa quando rodar "python operacoes.py" diretamente
    print(soma(2, 2))
```

Se outro arquivo fizer `import operacoes`, o bloco dentro do `if` **não** executa - só o `def` é processado. É por isso que esse padrão aparece em praticamente todo script Python.

## Pacotes básicos

Um pacote é uma pasta com vários módulos relacionados, mais um arquivo especial `__init__.py` marcando essa pasta como importável:

```
meu_projeto/
├── calculos/
│   ├── __init__.py
│   ├── basicos.py
│   └── avancados.py
└── main.py
```

```python
# main.py
from calculos.basicos import soma
from calculos import avancados
```

O `__init__.py` pode ficar vazio - ele só sinaliza para o Python que aquela pasta é um pacote. (Desde a [PEP 420](https://peps.python.org/pep-0420/), Python 3.3+, também existem "pacotes de namespace" implícitos, que dispensam esse arquivo em casos mais avançados - mas criar o `__init__.py` continua sendo a prática mais simples e previsível para projetos comuns.)

Imports dentro do próprio pacote podem ser absolutos (caminho completo, a partir da raiz do projeto) ou relativos (`from . import avancados`, relativo à posição do próprio módulo). Prefira imports absolutos: são mais fáceis de ler e não quebram se você mover um arquivo dentro do pacote.

## Referências

- [O tutorial do Python - Módulos (pt-BR)](https://docs.python.org/pt-br/3/tutorial/modules.html)
- [PEP 420 - Implicit Namespace Packages](https://peps.python.org/pep-0420/)
- [Real Python - Python Modules and Packages](https://realpython.com/python-modules-packages/)
