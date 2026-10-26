# Introdução ao Python

Python é uma linguagem de programação de código aberto criada por Guido van Rossum, nos Países Baixos. O desenvolvimento começou no fim dos anos 1980 e a primeira versão foi publicada em 1991. O nome vem do grupo de comédia britânico Monty Python, não da cobra.

Hoje a linguagem é mantida pela comunidade e pela Python Software Foundation (PSF), e está entre as mais usadas do mundo.

## A filosofia do Python

Python foi pensado para que o código seja **fácil de ler**. A ideia é que se lê código muito mais vezes do que se escreve, então a clareza vale mais do que a esperteza.

Essa filosofia está resumida no **Zen of Python**, que você pode ler no próprio interpretador:

```python
import this
```

Alguns princípios do Zen:

- *Beautiful is better than ugly* - bonito é melhor que feio
- *Explicit is better than implicit* - explícito é melhor que implícito
- *Simple is better than complex* - simples é melhor que complexo
- *Readability counts* - legibilidade conta

## Características principais

### Interpretada e com tipagem dinâmica

Você executa o código direto, sem uma etapa de compilação manual. O tipo de uma variável é definido pelo valor que ela guarda no momento, e pode mudar:

```python
valor = 42        # int
valor = "texto"   # agora é str
```

Os tipos ainda existem e são verificados em tempo de execução: somar um número com um texto gera um erro (`TypeError`). Por isso se diz que Python tem tipagem **dinâmica e forte**.

### Indentação faz parte da sintaxe

Em Python, os blocos de código são definidos pela **indentação**, não por chaves. O padrão é usar 4 espaços:

```python
idade = 20

if idade >= 18:
    print("Pode votar")
else:
    print("Ainda não pode votar")
```

Isso obriga todo código Python a ter a mesma cara - e é um dos motivos de ele ser tão legível.

### Type hints opcionais

Desde o Python 3.5, você pode anotar os tipos das variáveis e funções. O interpretador não verifica essas anotações, mas editores e ferramentas como o mypy e o Pyright usam para encontrar erros antes de o programa rodar:

```python
def saudacao(nome: str) -> str:
    return f"Olá, {nome}!"
```

Em projetos de backend, type hints são praticamente obrigatórios: frameworks como o FastAPI usam essas anotações para validar dados e gerar documentação.

### Multiparadigma

Python permite programar de forma procedural, orientada a objetos ou funcional - e misturar os estilos quando faz sentido:

```python
class Cachorro:
    def __init__(self, nome: str):
        self.nome = nome

    def latir(self) -> str:
        return f"{self.nome}: au au!"


caramelos = [Cachorro("Paçoca"), Cachorro("Biscoito")]
latidos = [c.latir() for c in caramelos]  # list comprehension
```

### "Baterias incluídas"

A biblioteca padrão do Python é enorme: lidar com arquivos, JSON, datas, expressões regulares, HTTP, banco SQLite, testes, linha de comando e muito mais, sem instalar nada. Para o resto, existe o [PyPI](https://pypi.org/), o repositório oficial de pacotes, com centenas de milhares de bibliotecas.

## Onde o Python é usado

- **Backend e APIs** - Django, FastAPI e Flask; Instagram e Spotify usam Python em partes importantes dos seus sistemas
- **Dados e inteligência artificial** - pandas, NumPy, scikit-learn, PyTorch e o ecossistema de LLMs
- **Automação e scripts** - tarefas repetitivas, integração entre sistemas, DevOps e testes automatizados
- **Ensino** - é uma das linguagens mais usadas para ensinar programação

Este lab foca na linguagem e no desenvolvimento backend. Dados e IA têm labs próprios no Caramelo Labs.

## Anatomia de um programa Python

```python
def main() -> None:
    nome = input("Qual é o seu nome? ")
    print(f"Olá, {nome}! Bem-vindo ao Python.")


if __name__ == "__main__":
    main()
```

- **`def main() -> None:`** - declara uma função. O `-> None` é um type hint dizendo que ela não retorna valor.
- **`input(...)`** - lê um texto digitado no terminal.
- **`f"Olá, {nome}!"`** - uma **f-string**, que insere o valor de variáveis dentro do texto.
- **`if __name__ == "__main__":`** - só chama `main()` quando o arquivo é executado diretamente, não quando é importado por outro módulo. É uma convenção que você vai ver em quase todo script Python.

Repare que não há ponto e vírgula nem chaves: o fim da linha encerra a instrução e a indentação define os blocos.

## O jeito Python de escrever código

A comunidade chama o código que segue as convenções da linguagem de **pythônico**. Alguns hábitos:

- Seguir a [PEP 8](https://peps.python.org/pep-0008/), o guia de estilo oficial: `snake_case` para variáveis e funções, `PascalCase` para classes, 4 espaços de indentação
- Usar um formatador (como o `ruff format`) para não discutir estilo
- Preferir construções da linguagem a laços manuais quando deixam o código mais claro, como list comprehensions e `enumerate`
- Pedir perdão em vez de permissão: tentar a operação e tratar a exceção (`try`/`except`) costuma ser mais pythônico do que verificar tudo antes

## Referências

- [Documentação oficial do Python (pt-BR)](https://docs.python.org/pt-br/3/)
- [O tutorial do Python](https://docs.python.org/pt-br/3/tutorial/index.html)
- [PEP 20 - The Zen of Python](https://peps.python.org/pep-0020/)
- [PEP 8 - Style Guide for Python Code](https://peps.python.org/pep-0008/)
- [History and License - documentação oficial](https://docs.python.org/3/license.html)
