# Type Hints

Python é dinamicamente tipado: você não precisa declarar o tipo de uma variável para o código rodar. Mas, conforme um projeto cresce, isso vira uma desvantagem - fica difícil saber, só de olhar uma função, o que ela espera receber e o que ela devolve. Type hints resolvem essa lacuna sem abandonar a flexibilidade da linguagem.

## Anotações básicas

Desde o Python 3.5 ([PEP 484](https://peps.python.org/pep-0484/)), você pode anotar parâmetros, retorno e variáveis com o tipo esperado:

```python
def saudacao(nome: str) -> str:
    return f"Olá, {nome}!"

idade: int = 25
ativo: bool = True
```

O ponto importante: **o interpretador não verifica nada disso em tempo de execução**. Type hints são só metadados - você pode passar um `int` para `saudacao()` e o Python não vai reclamar na hora:

```python
saudacao(42)  # funciona, o Python não valida o type hint
```

Quem lê essas anotações são ferramentas externas: o editor de código (para autocompletar e sinalizar erros antes de rodar), e verificadores estáticos como mypy ou Pyright (próxima seção). Em frameworks como o FastAPI, os type hints vão além da documentação - eles são usados ativamente para validar dados recebidos e gerar a documentação da API automaticamente.

## Tipos compostos

Para coleções, você anota o tipo dos itens dentro de `[]` - sintaxe disponível nativamente desde o Python 3.9 (antes disso, era preciso importar `List`, `Dict` etc. do módulo `typing`):

```python
numeros: list[int] = [1, 2, 3]
config: dict[str, int] = {"tentativas": 3, "timeout": 30}
coordenadas: tuple[float, float] = (10.5, 20.3)
```

Para um valor que pode ser de um tipo **ou** `None` (ou qualquer outra combinação de tipos possíveis), o operador `|` substitui o antigo `Optional[X]`/`Union[X, Y]` desde o Python 3.10 ([PEP 604](https://peps.python.org/pep-0604/)):

```python
def buscar_usuario(id: int) -> dict | None:
    ...

def processa(valor: int | str) -> str:
    ...
```

## Tipos mais avançados

`TypedDict` anota a estrutura esperada de um dicionário - útil quando um dict representa um registro com campos fixos (como um JSON vindo de uma API):

```python
from typing import TypedDict

class Usuario(TypedDict):
    nome: str
    idade: int

def cria_usuario(dados: Usuario) -> None:
    print(dados["nome"])
```

`Protocol` descreve um "contrato" baseado em comportamento, não em herança - qualquer objeto que tenha os métodos esperados é aceito, sem precisar herdar de nenhuma classe especial (é o _duck typing_ formalizado em tipo):

```python
from typing import Protocol

class TemArea(Protocol):
    def area(self) -> float: ...

def imprime_area(forma: TemArea) -> None:
    print(forma.area())

# qualquer classe com um método area() satisfaz o Protocol, sem herança
```

Generics permitem escrever uma estrutura ou função que funciona com qualquer tipo, mas mantendo a informação de qual tipo foi usado em cada uso específico. Desde o Python 3.12 ([PEP 695](https://peps.python.org/pep-0695/)), a sintaxe ficou mais direta, com os parâmetros de tipo declarados entre `[]` junto da classe ou função:

```python
class Pilha[T]:
    def __init__(self) -> None:
        self.itens: list[T] = []

    def empilhar(self, item: T) -> None:
        self.itens.append(item)

    def desempilhar(self) -> T:
        return self.itens.pop()

pilha_de_inteiros = Pilha[int]()
pilha_de_inteiros.empilhar(10)
```

Antes do 3.12, a mesma coisa exigia declarar um `TypeVar` separadamente (`T = TypeVar("T")`) - a sintaxe antiga ainda funciona e aparece bastante em código já existente, mas a nova é a recomendada para projetos novos.

## Verificação estática

Type hints só têm valor se alguma ferramenta realmente as verificar. As duas mais usadas:

- **mypy** - o verificador estático original e mais usado da comunidade, roda como um comando separado (`mypy arquivo.py`)
- **Pyright** - mais rápido, desenvolvido pela Microsoft, é o que dá o autocompletar e os avisos em tempo real no VS Code (via extensão Pylance)

```bash
mypy main.py
```

Nenhum dos dois impede o programa de rodar com tipos "errados" - eles só avisam **antes** de rodar, como um linter focado em tipos. Em projetos de backend (APIs com FastAPI, por exemplo), rodar o verificador estático no CI é uma forma barata de pegar uma categoria inteira de bugs antes que cheguem à produção.

## Referências

- [Documentação oficial - módulo typing](https://docs.python.org/3/library/typing.html)
- [PEP 604 - Allow writing union types as X | Y](https://peps.python.org/pep-0604/)
- [PEP 695 - Type Parameter Syntax](https://peps.python.org/pep-0695/)
- [mypy - documentação oficial](https://mypy.readthedocs.io/)
