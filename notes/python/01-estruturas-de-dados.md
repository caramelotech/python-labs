# Estruturas de Dados

Em [Fundamentos](/labs/python/fundamentos/03-sintaxe-e-tipos/) você viu os tipos básicos: números, texto, booleanos. Mas quase todo programa real também precisa guardar **coleções** de valores - uma lista de pedidos, um cadastro de usuários, um conjunto de tags únicas. Esta nota aprofunda as quatro estruturas embutidas do Python e as ferramentas da biblioteca padrão construídas sobre elas.

## Listas e tuplas

Lista e tupla guardam sequências ordenadas de itens, mas diferem em um ponto decisivo: listas são **mutáveis** (você pode alterar o conteúdo depois de criadas), tuplas são **imutáveis**.

```python
numeros = [1, 2, 3]       # lista
numeros.append(4)          # [1, 2, 3, 4] - ok, lista é mutável

ponto = (10, 20)           # tupla
ponto[0] = 99               # TypeError: 'tuple' object does not support item assignment
```

Essa imutabilidade tem uma vantagem prática: tuplas podem ser usadas como chave de dicionário (porque são _hashable_), listas não podem:

```python
distancias = {(0, 0): 0, (1, 1): 1.41}  # ok, tupla como chave
distancias_erradas = {[0, 0]: 0}         # TypeError: unhashable type: 'list'
```

Ambas aceitam indexação positiva e negativa, e fatiamento (`slicing`) para extrair uma parte da sequência:

```python
letras = ["a", "b", "c", "d", "e"]

letras[0]      # 'a'
letras[-1]     # 'e' (último item)
letras[1:3]    # ['b', 'c'] (do índice 1 até o 3, exclusive)
letras[::2]    # ['a', 'c', 'e'] (passo 2)
letras[::-1]   # ['e', 'd', 'c', 'b', 'a'] (invertido)
```

Os métodos mais usados de lista:

```python
tarefas = ["estudar"]

tarefas.append("revisar")       # adiciona no final
tarefas.insert(0, "planejar")   # insere em uma posição
tarefas.remove("estudar")       # remove pelo valor
tarefas.pop()                   # remove e retorna o último item
tarefas.sort()                  # ordena a própria lista
tarefas.reverse()               # inverte a própria lista
```

Tuplas têm só os métodos que não alteram o conteúdo, como `count()` e `index()`.

## Dicionários e conjuntos

Um dicionário guarda pares chave-valor, com acesso direto pela chave em vez de por posição:

```python
usuario = {"nome": "Ana", "idade": 25}

usuario["nome"]           # 'Ana'
usuario.get("email")      # None (não gera erro se a chave não existir)
usuario.get("email", "sem email")  # 'sem email' (valor padrão)

for chave, valor in usuario.items():
    print(f"{chave}: {valor}")
```

Desde o Python 3.7, a ordem de inserção das chaves é **garantida** pela especificação da linguagem, não é mais um detalhe de implementação do CPython - iterar um dicionário sempre segue a ordem em que as chaves foram adicionadas.

Um conjunto (`set`) guarda itens únicos, sem ordem definida, e é otimizado para testar se um item pertence a ele ou para operações entre conjuntos:

```python
tags_post_a = {"python", "backend", "api"}
tags_post_b = {"python", "frontend"}

tags_post_a | tags_post_b   # união: {'python', 'backend', 'api', 'frontend'}
tags_post_a & tags_post_b   # interseção: {'python'}
tags_post_a - tags_post_b   # diferença: {'backend', 'api'}

"python" in tags_post_a     # True, e essa checagem é muito mais rápida que em uma lista
```

Quando você precisa de um conjunto que não pode ser alterado (e, por isso, pode ser usado como item de outro conjunto, ou como chave de dicionário), existe o `frozenset`, a versão imutável do `set`.

## Comprehensions

Comprehensions criam uma coleção nova a partir de outra, aplicando uma transformação e/ou um filtro em uma única linha - no lugar de um `for` que cria a lista vazia e vai fazendo `append`:

```python
numeros = range(10)

# list comprehension
quadrados = [n ** 2 for n in numeros]                  # [0, 1, 4, 9, 16, ...]
pares = [n for n in numeros if n % 2 == 0]              # [0, 2, 4, 6, 8]

# dict comprehension
quadrados_dict = {n: n ** 2 for n in numeros}           # {0: 0, 1: 1, 2: 4, ...}

# set comprehension
letras_unicas = {letra for letra in "abracadabra"}      # {'a', 'b', 'r', 'c', 'd'}
```

Também é possível combinar condicional e laços aninhados, mas com moderação - comprehension com mais de um `for` ou mais de uma condição costuma ficar mais difícil de ler que um laço comum escrito por extenso:

```python
matriz = [[1, 2], [3, 4], [5, 6]]
achatada = [item for linha in matriz for item in linha]  # [1, 2, 3, 4, 5, 6]
```

## Módulo `collections`

A biblioteca padrão traz estruturas especializadas que resolvem problemas comuns melhor do que lista/dict puros:

```python
from collections import Counter, defaultdict, deque, namedtuple, ChainMap

# Counter: conta ocorrências automaticamente
contagem = Counter("abracadabra")
contagem.most_common(2)   # [('a', 5), ('b', 2)]

# defaultdict: fornece um valor padrão para chave ausente, sem precisar checar antes
grupos = defaultdict(list)
grupos["frutas"].append("maçã")   # não precisa inicializar a lista primeiro

# deque: fila de duas pontas, O(1) para inserir/remover no início ou no fim
fila = deque([1, 2, 3])
fila.appendleft(0)   # [0, 1, 2, 3]
fila.pop()            # remove do final

# namedtuple: tupla com campos nomeados, mais legível que índices numéricos
Ponto = namedtuple("Ponto", ["x", "y"])
p = Ponto(10, 20)
p.x, p.y   # 10, 20

# ChainMap: visão combinada de vários dicionários, sem criar uma cópia
padrao = {"tema": "claro"}
usuario = {"idioma": "pt-BR"}
config = ChainMap(usuario, padrao)
config["tema"]  # 'claro' (busca em usuario primeiro, depois em padrao)
```

Uma lista comum já serve como fila de duas pontas na prática, mas remover ou inserir no início de uma lista custa O(n) - ela precisa deslocar todos os outros itens. Um `deque` faz a mesma operação em O(1), por isso é a escolha certa quando você remove/adiciona bastante nas pontas.

## Outras estruturas úteis

`heapq` implementa uma fila de prioridade (heap binário): o menor item sempre fica acessível em O(1), e inserir/remover custa O(log n) - útil para algoritmos de agendamento ou "top N" sem ordenar a lista inteira.

```python
import heapq

numeros = [5, 1, 8, 2, 9]
heapq.heapify(numeros)        # transforma a lista em um heap, em lugar
heapq.heappush(numeros, 0)     # insere mantendo a propriedade de heap
heapq.heappop(numeros)         # remove e retorna o menor item (0)
```

Uma pilha (LIFO - último a entrar, primeiro a sair) é só uma lista usada com `append()`/`pop()` no final. Uma fila (FIFO - primeiro a entrar, primeiro a sair) deve usar `deque`, pelo motivo de performance explicado acima:

```python
pilha = []
pilha.append("a")
pilha.append("b")
pilha.pop()   # 'b' (o último que entrou)

fila = deque()
fila.append("a")
fila.append("b")
fila.popleft()   # 'a' (o primeiro que entrou)
```

## Complexidade das operações

Saber o custo aproximado (Big-O) de cada operação ajuda a escolher a estrutura certa:

| Operação                  | Lista | Dict       | Set        | Tupla        |
| ------------------------- | ----- | ---------- | ---------- | ------------ |
| Acesso por índice/chave   | O(1)  | O(1) médio | -          | O(1)         |
| Busca por valor (`in`)    | O(n)  | O(1) médio | O(1) médio | O(n)         |
| Inserir/remover no final  | O(1)  | O(1) médio | O(1) médio | - (imutável) |
| Inserir/remover no início | O(n)  | -          | -          | -            |

A tabela completa e oficial está na documentação do Python, referenciada abaixo. O ponto prático: se você testa pertencimento (`in`) com frequência em uma coleção grande, um `set` ou `dict` costuma ser muito mais rápido que uma lista.

## Strings e bytes (aprofundando)

Strings já apareceram em [Sintaxe e Tipos Básicos](/labs/python/fundamentos/03-sintaxe-e-tipos/), mas vale reforçar: `str` em Python é uma sequência **imutável** de caracteres - cada "modificação" (`.upper()`, `.replace()`, concatenação) na verdade cria uma string nova, nunca altera a original.

```python
texto = "Caramelo"
maiusculo = texto.upper()
texto is maiusculo   # False, são objetos diferentes
```

Quando você trabalha com dados binários (arquivos, rede, criptografia), usa `bytes` em vez de `str`. Strings guardam texto (caracteres Unicode); bytes guardam números de 0 a 255 - e converter de um para o outro exige escolher uma codificação, quase sempre UTF-8:

```python
texto = "café"
dados = texto.encode("utf-8")   # b'caf\xc3\xa9' (bytes)
de_volta = dados.decode("utf-8") # 'café' (str de novo)
```

`bytearray` é a versão mutável de `bytes`, útil quando você precisa modificar dados binários no lugar sem recriar o objeto a cada mudança.

## Referências

- [O tutorial do Python - Estruturas de dados (pt-BR)](https://docs.python.org/pt-br/3/tutorial/datastructures.html)
- [Complexidade de tempo dos tipos embutidos - documentação oficial](https://docs.python.org/3/library/time-complexity.html)
- [Documentação oficial - módulo collections](https://docs.python.org/3/library/collections.html)
- [Real Python - Python Dictionary Comprehensions](https://realpython.com/python-dictionary-comprehension/)
