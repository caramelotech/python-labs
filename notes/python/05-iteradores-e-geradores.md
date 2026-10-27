# Iteradores e Geradores

Todo `for` que você já escreveu funciona por trás de um mecanismo comum: o protocolo de iteração. Entender esse mecanismo explica por que `for item in lista` e `for linha in arquivo` funcionam da mesma forma, mesmo sendo coisas tão diferentes - e abre caminho para criar suas próprias sequências, geradas sob demanda em vez de guardadas inteiras na memória.

## O protocolo de iteração

Um objeto é **iterável** se implementa `__iter__`, que devolve um **iterador**. Um iterador implementa `__next__`, que devolve o próximo item a cada chamada, e levanta `StopIteration` quando não há mais itens:

```python
numeros = [1, 2, 3]

iterador = iter(numeros)     # chama numeros.__iter__()
next(iterador)                 # 1
next(iterador)                 # 2
next(iterador)                 # 3
next(iterador)                 # StopIteration
```

Um `for` faz exatamente isso por trás dos panos: pega um iterador do objeto, chama `next()` repetidamente, e para quando captura o `StopIteration` - só que de forma automática e silenciosa.

```python
for n in numeros:
    print(n)

# é equivalente, na prática, a:
iterador = iter(numeros)
while True:
    try:
        n = next(iterador)
    except StopIteration:
        break
    print(n)
```

## Generators

Escrever uma classe com `__iter__`/`__next__` manualmente funciona, mas dá trabalho. Uma função geradora faz a mesma coisa de forma muito mais simples, usando `yield` em vez de `return`:

```python
def contador(limite: int):
    numero = 0
    while numero < limite:
        yield numero
        numero += 1

for n in contador(5):
    print(n)  # 0, 1, 2, 3, 4
```

A diferença entre `yield` e `return` é fundamental: `return` termina a função e entrega um valor final; `yield` **pausa** a função, entrega um valor, e retoma de onde parou na próxima chamada a `next()`. É assim que um generator consegue produzir uma sequência potencialmente infinita, sem nunca guardar todos os valores na memória de uma vez - cada valor só existe no momento em que é pedido (avaliação **lazy**).

```python
def numeros_pares():
    n = 0
    while True:          # sequência infinita, sem problema
        yield n
        n += 2

pares = numeros_pares()
next(pares)  # 0
next(pares)  # 2
next(pares)  # 4
```

Para ver a diferença de memória na prática: uma lista com um milhão de números ocupa espaço para guardar o milhão de valores de uma vez. Um generator que produz os mesmos números ocupa só o espaço de controle da função pausada, independente de quantos valores vai gerar no total.

## Generator expressions

Assim como list comprehension é uma forma compacta de criar uma lista, uma generator expression cria um generator - mesma sintaxe de comprehension, mas com `()` em vez de `[]`:

```python
quadrados_lista = [n ** 2 for n in range(1_000_000)]   # cria a lista inteira na memória, já
quadrados_gerador = (n ** 2 for n in range(1_000_000))  # não cria nada ainda, só descreve como gerar

sum(quadrados_gerador)  # consome o generator, calculando cada valor sob demanda
```

Use generator expression quando você vai percorrer os valores uma única vez e não precisa guardar a coleção inteira (por exemplo, dentro de `sum()`, `max()` ou um `for`). Use list comprehension quando você precisa acessar os itens por índice, percorrer mais de uma vez, ou saber o tamanho (`len()`) - um generator já consumido não pode ser percorrido de novo.

## Módulo `itertools`

A biblioteca padrão traz funções prontas para combinar e transformar iteráveis sem precisar escrever o laço manualmente:

```python
import itertools

# encadeia vários iteráveis em um só, sem criar uma lista intermediária
list(itertools.chain([1, 2], [3, 4]))        # [1, 2, 3, 4]

# todas as combinações possíveis de ordem
list(itertools.permutations("abc", 2))        # [('a','b'), ('a','c'), ('b','a'), ...]

# agrupa itens consecutivos iguais
list(itertools.groupby("aaabbbcca"))          # [('a', ...), ('b', ...), ('c', ...), ('a', ...)]

# repete um iterável indefinidamente (cuidado com laços infinitos)
ciclo = itertools.cycle([1, 2, 3])
```

Vale a pena conhecer o módulo antes de reimplementar essas combinações na mão - quase sempre já existe uma função pronta e otimizada para o que você precisa.

## Referências

- [O tutorial do Python - Geradores (pt-BR)](https://docs.python.org/pt-br/3/tutorial/classes.html#generators)
- [Documentação oficial - módulo itertools](https://docs.python.org/3/library/itertools.html)
- [Real Python - How to Use Generators and yield in Python](https://realpython.com/introduction-to-python-generators/)
